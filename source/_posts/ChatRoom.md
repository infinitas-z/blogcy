---
title: Java实现简易聊天室
date: 2023-06-01 21:38:36
categories: Java
tags: [学习笔记]
top_img: https://pic.imgdb.cn/item/6478a138f024cca1739bf681.png
cover: https://pic.imgdb.cn/item/6478a138f024cca1739bf681.png
---
## ChatRoom

### Server类

```java
package com.guangsha.server;


import com.guangsha.entity.User;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.*;


public class Server {
    //用于PrivateChat判断
    static Map map=new HashMap();
    //ServerSocket向服务端操作系统申请服务端口，监听端口
    private ServerSocket serverSocket;

    PrintWriter pw = null;
    //存放所有客户端输出流，用于广播消息
    private PrintWriter[] allOut = {};
    //用户账号密码
    static String username;
    static String pwd;
    //服务端构造方法，用来初始化
    public Server(){
        try {
            System.out.println("正在启动服务端...");
            //实例化ServerSocket,指定端口号
            serverSocket = new ServerSocket(8088);
            System.out.println("服务端启动完毕!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    //服务端开始工作的方法
    public void start(){

        try {
            while(true) {
                System.out.println("等待客户端链接...");
                /*
                accept方法进行截断阻塞，等待客户端，客户端连接后，返回一个Socket实例
                通过实例来与客户端交互
                */
                Socket socket = serverSocket.accept();
                System.out.println("一个客户端链接了！");
                //启动一个线程与该客户端交互
                ClientHandler clientHandler = new ClientHandler(socket);
                Thread t = new Thread(clientHandler);
                t.start();

            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        Server server = new Server();
        server.start();
    }

    /*
    定义线程任务
    * */
    private class ClientHandler implements Runnable{
        private Socket socket;
        //记录Client端IP地址
        private String host;
//        public ClientHandler(String host) {
//            this.host = host;
//        }
        public ClientHandler(Socket socket){
            this.socket = socket;
            //通过socket获取IP地址
            host = socket.getInetAddress().getHostAddress();
        }
        //用户登录判断方法
        private void login() throws IOException {
//            Scanner scanner=new Scanner(System.in);
            //将字节通过网络发送给对方
            OutputStream out = socket.getOutputStream();
            //将写出的字符按照指定字符集转化为字节
            OutputStreamWriter osw = new OutputStreamWriter(out, "UTF-8");
            //把字符写入字符缓冲区
            BufferedWriter bw = new BufferedWriter(osw);
            //通过PrintWrite把缓冲区内的数据送到客户端,自动行刷新
            PrintWriter pw = new PrintWriter(bw, true);
            //获取服务端发回的数据
            InputStream in = socket.getInputStream();
            //通过InputStreamReader方法转换为UTF-8字符
            InputStreamReader isr = new InputStreamReader(in, "UTF-8");
            //读取字符缓冲区的字符
            BufferedReader br = new BufferedReader(isr);
            for(;;){
                //把打印内容输出到客户端
                pw.println("请输入用户名：");
                //读取缓冲区中的用户名
                username = br.readLine();
                pw.println("请输入密码：");
                //读取缓冲区中的密码
                pwd = br.readLine();
                //进行用户名密码的判断
                if (pwd.equals(new User().getMap().get(username))) {
                    pw.println("登录成功！");
                    break;
                } else {
                    pw.println("账号或密码错误，请重试！！！");
                }
            }
        }

        public void run(){
            try{
                login();
                InputStream in = socket.getInputStream();
                InputStreamReader isr = new InputStreamReader(in, "UTF-8");
                BufferedReader br = new BufferedReader(isr);

                OutputStream out = socket.getOutputStream();
                OutputStreamWriter osw = new OutputStreamWriter(out,"UTF-8");
                BufferedWriter bw = new BufferedWriter(osw);
                pw = new PrintWriter(bw,true);
                //将该输出流存入共享数组allOut中
                synchronized (Server.this){
                    //将IP地址和流中的数据放入Map集合中
                    map.put(host,pw);
                    //对allOut数组扩容
                    allOut = Arrays.copyOf(allOut, allOut.length + 1);
                    //将输出流存入数组最后一个位置
                    allOut[allOut.length - 1] = pw;
                }
                //显示在线人数，显示用户上线信息
                sendMessage(host + "上线了,当前在线人数:"+allOut.length);

                String message = null;
                while ((message = br.readLine()) != null) {
                    System.out.println(host + "说:" + message);
                    //私聊信息的判断
                    if(PrivateChat(host,message))continue;
                    sendMessage(host + "说:" + message);
                }
            }catch(IOException e){
                e.printStackTrace();
            }finally{
                //客户端断开链接操作
                for (int i = 0; i < allOut.length; i++) {
                    if (allOut[i] == pw) {
                        allOut[i] = allOut[allOut.length - 1];
                        allOut = Arrays.copyOf(allOut, allOut.length - 1);
                        break;
                    }
                }
                sendMessage(host+"下线了，当前在线人数:"+allOut.length);
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        //广播信息
        private void sendMessage(String message){
            synchronized (Server.this) {
                for (int i = 0; i < allOut.length; i++) {
                    allOut[i].println(message);
                }
            }

        }
//私聊信息判断
        private boolean PrivateChat(String host,String s){
            //字符串以空格分隔赋值到数组中
            String[] str = s.split("\\ ");
            String st1="";
            //判断数组第一个元素是否符合私聊标准
            if(">>".equals(str[0])){
                //拿去数据
                pw = (PrintWriter) map.get(str[1]);
                //排除空格引发的数据丢失
                for (int i = 2; i < str.length; i++) {
                    st1+=str[i];
                }
                pw.println("来自"+host+"私聊消息："+st1);
                return true;
            }
            return false;
        }
    }
}

```

### Client类

```java
package com.guangsha.client;



import java.io.*;
import java.net.Socket;
import java.util.Scanner;

public class Client {
    private Socket socket;
    public Client(){
        try {
            System.out.println("正在链接服务端...");
            Scanner scanner=new Scanner(System.in);
            //实例化Socket传入IP和端口
            socket = new Socket("10.11.194.57",8088);
            System.out.println("与服务端建立链接!");

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /*
    客户端开始工作的方法
    * */
    public void start(){
        try {
            //启动读取服务端发送过消息的线程
            ServerHandler handler = new ServerHandler();
            Thread t = new Thread(handler);
            t.setDaemon(true);
            t.start();

            //将字节通过网络发送给对方
            OutputStream out = socket.getOutputStream();
            //将写出的字符按指定字符集转字节
            OutputStreamWriter osw = new OutputStreamWriter(out,"UTF-8");
            BufferedWriter bw = new BufferedWriter(osw);
            //写出字符串，自动行刷新
            PrintWriter pw = new PrintWriter(bw,true);
            Scanner scanner = new Scanner(System.in);

            while(true) {
                String line = scanner.nextLine();
                //退出判断
                if("exit".equalsIgnoreCase(line)){
                    break;
                }
                pw.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        Client client = new Client();
        client.start();
    }
    /*
    该线程负责接收服务端发送过来的消息
    * */
    private class ServerHandler implements Runnable{
        public void run(){
            //通过socket获取输入流读取服务端发送过来的消息
            try {
                InputStream in = socket.getInputStream();
                InputStreamReader isr = new InputStreamReader(in,"UTF-8");
                BufferedReader br = new BufferedReader(isr);

                String line;
                //循环读取服务端发送过来的每一行字符串
                while((line = br.readLine())!=null){
                    System.out.println(line);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### User类

```java
package com.guangsha.entity;

import java.util.HashMap;
import java.util.Map;

public class User {

    private Map<String,String> map=new HashMap<String,String>();

    public User(Map<String, String> map) {
        this.map = map;
    }

    public Map<String, String> getMap() {
        return map;
    }

    public void setMap(Map<String, String> map) {
        this.map = map;
    }
    //写入用户名密码到Map集合中
    public User() {
        map.put("123","456");
        map.put("456","789");
        map.put("abc","789");
    }
}

```

