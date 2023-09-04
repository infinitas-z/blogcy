---
title: Java网络编程
date: 2023-08-03 14:50:20
categories: 学习笔记
tags: [Java]
top_img: https://pic2.imgdb.cn/item/644d1bbd0d2dde5777ca565a.png
cover: https://pic2.imgdb.cn/item/644d1bbd0d2dde5777ca565a.png
---

## 常用API

数学类，包装类跳过 

### 时间类，Date,Calendar

```java
import java.util.Calendar;
import java.util.Date;
public class Demo1 {
    public static void main(String[] args) {
        Date date = new Date();
        System.out.println(date);//系统时间
        System.out.println(date.getYear()+1900);//从1900开始算
        System.out.println(date.getMonth()+1);//从0开始
        System.out.println(date.getTime());//获取时间的毫秒形式 返回long
        System.out.println("==============================");
        Calendar cal = Calendar.getInstance();
        cal.set(Calendar.DATE,cal.get(Calendar.DATE)+30);//日期增加30天
        System.out.println(cal.get(Calendar.YEAR));
        System.out.println(cal.get(Calendar.MONTH)+1);//还是从0开始
        System.out.println(cal.get(Calendar.DATE));
        System.out.println(cal.getTime());//返回Data对象
        cal.setTime(date);//可以把Date转化为Calendar
    }
}

```

格式化时间

```java
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
import java.util.Scanner;

public class Demo1 {
    public static void main(String[] args) throws Exception{
        Date date = new Date();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//自定义格式
        String format = sdf.format(date);//格式化时间
        System.out.println(format);
        System.out.println("================");
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个时间(yyyy-MM-dd HH:mm:ss)");
        String s = sc.nextLine();
        SimpleDateFormat inputsdf =new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date parse = inputsdf.parse(s); //将字符串转换为时间
        String format1 = sdf.format(parse);
        System.out.println(format1);
    }
}
```

### 字符串

常用方法

```java
public class Demo1 {
    public static void main(String[] args) throws Exception{
        String s = "zxcvbnmasgdhjk";
        System.out.println(s.charAt(0));//返回第0个字符
        //字符串是不可变的数据类型
        String s1 = s.concat("哈哈哈");//字符串末尾拼接
        System.out.println(s1);
        System.out.println(s1.contains("哈哈哈"));//判断字符串中是否包含该字符串，返回布尔
        // startsWith 或 endsWith 判断已XXX结尾或开头
        System.out.println(s1.endsWith("哈哈哈"));
        // equals 或 equalsIgnoreCase 判断字符串是否相同，后者忽视大小写
        String s2 = "1or2";
        //替换指定字符串
        String replace = s2.replace("1", "123");
        System.out.println(replace);
        //split 切割字符串,返回数组
        String s4 ="11_11_123_132";
        String[] s3 = s4.split("_");
        System.out.println(s3[2]);
        //substring(起始位置，终止位置)，终止位置取不到
        //trim() 去除左右两端多余的空格
        /*valueOf(xxx) 将参数返回为字符串，
        也可也把变量加空字符串(xx+"")来变成字符串*/
    }
}
```

在一个字符串数组中，输出每个同学的平均分

```java
public class Demo1 {
    public static void main(String[] args) throws Exception{
        double sum=0;
        String name ="";
       String[] stus = {"小绿_数学_12_语文_33_英语_42","小红_数学_54_语文_33_英语_97","小黄_数学_92_语文_23_英语_82"};
        for (String s : stus) {
            String[] s1 = s.split("_");

            Pattern pattern = Pattern.compile("\\d+");//正则表达式，匹配一个或多个字符
            for (int i =0 ;i<s1.length;i++) {
                Matcher matcher = pattern.matcher(s1[i]);//创建matcher对象进行匹配操作
                if(matcher.find()){//查找匹配项
                    sum+=Integer.parseInt(s1[i]);
                }//查找匹配项
                if(s1[i].equals("小绿") || s1[i].equals("小红") || s1[i].equals("小黄")){
                     name = s1[i];
                }
                if(i==s1.length-1){
                    System.out.println(name+"的平均分是"+sum/3);
                    sum=0;
                    name="";
                }
            }
        }
    }
}
```

### StringBuffer和StringBuilder

两者类似，都代表可变字符串，线程上的区别,一个线程同步一个线程不同步，appen():往后拼接字符串,insert（）指定位置插入字符串,

toString,将其变为字符串

```java
  StringBuilder sb = new StringBuilder();//默认16容量，超出时自动提升
  sb.capacity() //返回当前容量
  sb.append("Hello,world!") //拼接
  sb.reverse() //反转
  sb.replace(1,3,"你好") //索引部分替换成自定义字符串
  sb.delete(1,3) //删除索引部分
  sb.insert(3,"ii") //索引部分插入一段
```



### DecimalFormat

用于格式化小数

```java
  public static void main(String[] args) throws Exception{
       double d=10/3.0;
        System.out.println(d);
        //.小数点
        //0和# 表示数字
        //保留两位小数
        DecimalFormat decimalFormat = new DecimalFormat(".00");
        //格式化
        String format = decimalFormat.format(d);
        System.out.println(format);
    }
```

案例 用时间戳写个小案例,计算10s之内的手速

```java
public class Demo1 {
    public static void main(String[] args) throws Exception{
        System.out.println("按Enter键，开始！");
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();
        Calendar calendar = Calendar.getInstance();
        calendar.set(Calendar.SECOND,Calendar.SECOND+10); //增加10s
        Date end = calendar.getTime(); //获取data对象
        long endTime = end.getTime(); //获取时间戳
        int count =0;
        while (endTime - new Date().getTime() >=0){ 
            scanner.nextLine();
            System.out.println("你按了");
            count++;
        }
        System.out.println("你一共按了"+count);
        DecimalFormat decimalFormat = new DecimalFormat(".00");
        System.out.println("你的手速是"+decimalFormat.format(count / 10.0)+"次/秒");
    }
}
```

## 容器

### ArrayList

ArrayList 和 LinkedList区别，ArrayList的内存是连续的，自动扩容。LinkedList的内存可以不连续，下一个内存位置是通过后继指针去寻找，LinkedList查询效率低，但是LinkedList在增删元素的时候效率高。他删除元素只需让后继指针指向另一个就可以

在列表中取出来的数据都是Object类型的，

### Set

HashSet:无序和不重复    TreeSet:默认进行排序和不重复

### Map

HashMap:    自动排序  

如果put了相同的key，原来的key会被顶掉，KeySet()把Map中的所有的Key打包成一个Set集合

### iterator(迭代器)

作用就是避免集合的内部结构暴露

```java
public class Demo1 {
    public static void main(String[] args) throws Exception{
//        ArrayList<String> list = new ArrayList();
//        list.add("a");
//        list.add("b");
//        list.add("c");
//        list.add("d");
//        Iterator<String> iterator = list.iterator();
//        //next拿出下一个元素
//        String next = iterator.next();
//        System.out.println(next);
//        //hasNext 判断下一个元素是否是空的
//        while (iterator.hasNext()){
//            String next2 = iterator.next();
//            System.out.println(next2);
//        }
        HashMap Map = new HashMap();
        Map.put("1","aaa");
        Map.put("2","bbb");
        Map.put("3","ccc");
        Map.put("4","ddd");
        //方法一,把key生成一个set集合然后去获取
//        Set setkey = Map.keySet();
//        Iterator iterator = setkey.iterator();
//        while(iterator.hasNext()){
//            String next = (String) iterator.next();
//            System.out.println(Map.get(next));
//        }
        //方法二 生成Entry，直接获得
        Set set = Map.entrySet();
        Iterator iterator = set.iterator();
        while (iterator.hasNext()){
            Map.Entry next = (Map.Entry)iterator.next();
            System.out.println(next.getKey());
            System.out.println(next.getValue());
        }
    }
}
```

### Collections工具类

sort(List list)排序，默认升序 reverse(List list)反转集合中的元素

## IO流

流的分类：

1. 按照读写方向来分，分为输入流和输出流
2. 按照读写内容区分，分为字节流和字符流 ,字节流适用于非文本信息读取
3. 按照流的功能区分，分为节点流和处理流

### File

```java
public class Demo1{
    public static void main(String[] args) {
        try {
            //默认从根目录创建
            File file = new File("abc/student.txt");
            //创建新的文件
            //file.createNewFile();
            System.out.println(file.getParentFile());//拿到上一层文件夹的对象
            System.out.println(file.getParent()); //返回父级路径(字符串)、
            file.mkdir();//创建单层目录
            file.mkdirs();//创建多级目录
            file.renameTo(new File("abc/new.txt")); //重命名
            file.delete();//删除文件
            //查看相关
            System.out.println(file.exists());//查看文件是否存在
            file.isAbsolute();//查看文件是否为绝对路径
            file.length();//查看文件的大小 字节为单位

            //创建文件的基本步骤
            File file1 = new File("abc/def/new_2.txt");
            //判断上层文件是否存在
            if(!file1.getParentFile().exists()){
                file1.mkdirs();
            }
            file1.createNewFile();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
        }
    }
}
```

### 节点流

节点流：直接连接在文件上的，处理流：套在其他流上的

|        | 输入                | 输出                 |
| ------ | ------------------- | -------------------- |
| 字节流 | InputStream(读数据) | OutputStream(写数据) |
| 字符流 | Reader              | Writer               |

FileinputStream,读文件操作

```java
 public static void main(String[] args) {
        try {
            FileInputStream inputStream = new FileInputStream(new File("a.txt"));
            byte[] bytes = new byte[1024];
            int len =0;
            //获取返回的字符长度
            while((len=inputStream.read(bytes))!=-1){
                String string = new String(bytes,0,len);
                System.out.println(string);
            }
            //清空流，关闭流
            inputStream.flush();
            inputStream.close();
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
        }
    }
```

FileOutputStream :写入文件操作

```java
 public static void main(String[] args) {
        try {
            //默认先清空文件后写入数据，后面加上true可以直接写入
            FileOutputStream fileOutputStream = new FileOutputStream(new File("a.txt"), true);
            //转换为字节写入数据
            fileOutputStream.write("Hello".getBytes());
            fileOutputStream.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

FileReader  一个个读字符 

```java
public static void main(String[] args) {
        try {
            FileReader fileReader = new FileReader(new File("a.txt"));
            char[] chars = new char[1024];
            int len =0;
            while ((len=fileReader.read(chars))!=-1) {
                System.out.println(new String(chars,0,len));
            }
            fileReader.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

FileWriter

```java
 FileWriter fileWriter = new FileWriter(new File("a.txt"),true);
           fileWriter.write("你好");
```

### 缓冲流（处理流）

带有缓冲区的数据流，在文件中拿字节然后经过缓冲区二次处理在传输

BufferedinputStream,BufferedOutputStream,BufferedReader,BufferedWriter

```java
public class Demo1{
    public static void main(String[] args) throws Exception{
        //因为缓冲流是套节点流，所以要在里面新建一个节点流读取文件
        BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream(new File("a.txt")));
        //字符流对应字符流
        BufferedReader bufferedReader = new BufferedReader(new FileReader(new File("a.txt")));
        //读取文本文件一行
        System.out.println(bufferedReader.readLine());
    }
}
```

### 转换流 (处理流)

只能字节流转换成字符流

InputStreamReader  OutputStreamWriter

 ```java
 //字节流转字符流
  public static void main(String[] args) throws Exception{
         //把字节流转换成字符流，System.in是字节流
         BufferedReader bs = new BufferedReader(new InputStreamReader(System.in));
         System.out.println("从System.in接收到的是:"+bs.readLine());
     }
 //字符流给字节流的桥接，使用指定字符集将其中的字符编码转换为字节
 ```

### 对象流(处理流)

ObjectinputStream , ObjectOutputStream

我们对对象进行序列化操作，因为将对象存到硬盘时，要把对象转换为字节，需要给类添加一个实现Serialiable接口，该类就可以自动被序列化

```java
//先对对象进行序列化操作 实体类继承Serialiable接口 
public static void main(String[] args) throws Exception{
        //序列化操作 将对象转换为字节
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(new File("person.dat")));
        User milet = new User(1, "milet", 29);
        oos.writeObject(milet);
        oos.close();
    }
//再从创建好的字节文件中 反序列化，将字节转换为对象
public static void main(String[] args) throws Exception {
        //进行反序列化，将字节转换成对象，要在类中加入无参构造器
       ObjectInputStream ois= new ObjectInputStream(new FileInputStream(new File("person.dat")));
        User user = (User)ois.readObject();
        System.out.println(user.toString());
    }
```

小练习

用合适的方法文件中的李白改为李太白

```java
/*
唐诗三百首
1.静夜思,李太白,床前明月光，疑是地上霜。举头望明月，低头思故乡。
2.望庐山瀑布，李太白，日照香炉生紫烟，遥看瀑布挂前川，飞流直下三千尺，疑是银河落九天。
3.朝辞白帝彩云间，李太白，千里江陵一日还。两岸猿声啼不住，轻舟已过万重山
*/
//采用副本文件替换源文件的方法
public class Demo1{
    public static void main(String[] args) throws Exception{
        File origin_file = new File("唐诗.txt");
        File new_file = new File("new.txt");
        BufferedReader br = new BufferedReader(new FileReader(origin_file));
        //生成副本文件
        BufferedWriter bw = new BufferedWriter(new FileWriter(new_file));
        String line = "";
        while ((line = br.readLine())!=null){
            line =line.replace("李白","李太白");
            bw.write(line);
            //每一次都另起一行
            bw.newLine();
        }
        br.close();
        bw.flush();
        bw.close();
        //！ 需要先关闭流 再进行文件操作
        //删除源文件，将副本文件改为新文件
        origin_file.delete();
        new_file.renameTo(origin_file);
    }
}
```

## 多线程

### 实现方法

1. 继承Tread类 ，重写run方法

   ```java
   //MyThread 类
   public class MyThread extends Thread{
       @Override
       public void run() {
           //子线程执行的内容写在run中
           for (int i = 0; i < 1000; i++) {
               System.out.println("子线程");
           }
       }
   }
   // Demo1
   public class Demo2 {
       public static void main(String[] args) throws Exception {
           MyThread myThread = new MyThread();
           //启动子线程
           myThread.start();
           for (int i = 0; i < 1000; i++) {
               System.out.println(">>>>>>>>>>>>我是主线程");
           }
       }
   }
   ```

2. 实现Runable接口

   ```java
   //继承Runable接口
   public class MyThread implements Runnable{
       @Override
       public void run() {
           //子线程执行的内容写在run中
           for (int i = 0; i < 1000; i++) {
               System.out.println("子线程");
           }
       }
   }
   //实现自己的runable 
   public class Demo2 {
       public static void main(String[] args) throws Exception {
           //先创建Runable的对象
           MyThread myRunable = new MyThread();
           Thread thread = new Thread(myRunable);
           thread.start();
           for (int i = 0; i < 1000; i++) {
               System.out.println("主线程");
           }
           
       }
   }
   ```

3. 使用Executor框架实现，有返回值 ,转载大佬

   https://blog.csdn.net/aboy123/article/details/38307539?ydreferer=aHR0cHM6Ly9jbi5iaW5nLmNvbS8%3D

### 线程操作

```java
.setPriority(); //设置多个线程的优先级 10为最大
.sleep() //设置线程休眠事件 (毫秒)
.join() //让主线程等待子线程运行完毕
.yield() //让出CPU
.interrupt() //打断正在睡觉的子线程
```

### 线程同步

基本使用

当多个线程共享同一个资源时，我们可以在某一个线程访问此资源时，暂时封锁该线程。等待执行结束时，释放此锁，再运行其他线程 ,缺点 ，有死锁风险

方法一 synchronized关键词 ,有死锁风险，使用场景，两个线程都共享同一个资源并且都会去修改这个资源时。

模拟一个银行取钱的流程,Account类

```java
public class Account {
    private double balance;

    public Account(double balance){
        this.balance=balance;
    }
    //synchronized 一旦进入到此方法，锁定acc
    public synchronized void getMoney(){
        if(this.balance <=0){
            System.out.println("无钱可取");
            return;
        }
        System.out.println("取钱,还有"+this.balance);
        this.balance-=1000;
        System.out.println("还剩"+this.balance);
    }
}
```

创建线程

```java
public class GetMoneyThread extends Thread{
    private Account acc;
    public GetMoneyThread(Account acc){
        this.acc=acc; // 保证柜台和ATM用的是同一个账户
    }
    @Override
    public void run() {
       acc.getMoney();
    }
}
```

Test类

```java
public class Test {
    public static void main(String[] args) {
        Account account = new Account(1000);
        GetMoneyThread atm = new GetMoneyThread(account);
        GetMoneyThread table = new GetMoneyThread(account);
        atm.start();
        table.start();
    }
}
```

方法二：在方法内部使用synchronized(){}语句块,对特定的对象上锁 (常用)

```java
public void getMoney() {
        synchronized (this) { //加入synchronized语句块
            if (this.balance <= 0) {
                System.out.println("无钱可取");
                return;
            }
            System.out.println("取钱,还有" + this.balance);
            this.balance -= 1000;
        }
        System.out.println("还剩" + this.balance);
    }
```

方法三：手动上锁，需要手动去掉锁

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Account {
    private double balance;
    private Lock lock = new ReentrantLock();//创建一个锁
    public Account(double balance){
        this.balance=balance;
    }
    //synchronized 一旦进入到此方法，锁定acc
    public void getMoney() {
        lock.lock(); //上锁
            if (this.balance <= 0) {
                System.out.println("无钱可取");
                return;
            }
            System.out.println("取钱,还有" + this.balance);
            this.balance -= 1000;
        System.out.println("还剩" + this.balance);
        lock.unlock();//解锁
    }
}

```

### 死锁

如果有互相调用的方法被锁定，慎用synchronized

### 生产者消费者模型

BlockingQueue 队列 阻塞队列:当队列中没有数据时，需要拿数据时，队列会将程序阻塞，阻塞到有数据位置再继续工作,模拟一个做面包，买面包的示例

生产者线程

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.atomic.AtomicInteger;
/**
 * 生产者线程
 * */
public class MakeBreadThread extends Thread{
    //线程安全的自增，如果用int进行自增，有可能出现数据不一致的情况
    private static AtomicInteger i = new AtomicInteger();
    //准备装面包的缓冲区(阻塞队列)
    private BlockingQueue<Bread> breadQueue = null;
    public MakeBreadThread(BlockingQueue<Bread> breadQueue){
        this.breadQueue =breadQueue;
    }

    @Override
    public void run() {
        while (true){
            String name = "法式"+i.incrementAndGet();
            Bread bread = new Bread(name);//相当于i++
            try {
                breadQueue.put(bread);//添加数据，加入队列
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("制作了一个面包"+name);
        }
    }
}

```

产品类

```java
/**
 * 产品
 * */
public class Bread {
    private String name;

    public Bread(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

消费者线程

```java
import java.util.concurrent.BlockingQueue;
/**
 * 消费者线程
 * */
public class BuyBreadThread extends Thread{
    private BlockingQueue<Bread> breadQueue;
    public BuyBreadThread(BlockingQueue<Bread> breadQueue){
        this.breadQueue=breadQueue;
    }

    @Override
    public void run() {
        while (true){
            try {
                Bread bread = breadQueue.take();
                System.out.println("我是发送的地方，我购买了"+bread.getName());
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

测试类

```java
import java.util.concurrent.LinkedBlockingQueue;

public class Test {
    public static void main(String[] args) {
        LinkedBlockingQueue<Bread> breads = new LinkedBlockingQueue<>();
        //创建三个生产者
        BuyBreadThread buyThread1 = new BuyBreadThread(breads);
        BuyBreadThread buyThread2 = new BuyBreadThread(breads);
        BuyBreadThread buyThread3 = new BuyBreadThread(breads);
        //创建两个消费者
        MakeBreadThread makeThread1 = new MakeBreadThread(breads);
        MakeBreadThread makeThread2 = new MakeBreadThread(breads);

        buyThread1.start();
        buyThread2.start();
        buyThread3.start();
        makeThread1.start();
        makeThread2.start();
    }
}
```

练习：模拟运用多线程，火车站4个窗口卖1000张票 。

Ticket类

```java
public class Ticket {
    private int id;
    private String name;
    public Ticket(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

TicketThrea类

```java
public class TicketsThread extends Thread{
    private List<Ticket> ticketList;
    private static AtomicInteger i = new AtomicInteger();
    public TicketsThread(List<Ticket> ticketList,String name){
        this.ticketList = ticketList;
        super.setName(name);//设置名字
    }
    @Override
    public void run() {
        while (true){
            //把票卖到最后四张时，分别把4张票分给四个不同的线程，避免再卖到999时，其他三个线程会报错自增
            if(i.get()+4<ticketList.size()){
                try {
                    Thread.sleep(new Random().nextInt(200));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                //实现自增，模拟卖票
                Ticket ticket = ticketList.get(i.incrementAndGet());
                System.out.println(super.getName()+"卖出"+ticket.getName());//打印车票信息
            }else{
                System.out.println(super.getName()+"卖完了");
                break;
            }

        }
    }
}

```

Test类

```java
import java.util.ArrayList;
public class Test {
    public static void main(String[] args) {
        ArrayList<Ticket> tickets = new ArrayList<>();
        //生成1000张票
        for (int i = 0; i < 1000; i++) {
            Ticket ticket = new Ticket(i, "火车票" + i);
            tickets.add(ticket);
        }
        TicketsThread tt1 = new TicketsThread(tickets, "窗口1");
        TicketsThread tt2 = new TicketsThread(tickets, "窗口2");
        TicketsThread tt3 = new TicketsThread(tickets, "窗口3");
        TicketsThread tt4 = new TicketsThread(tickets, "窗口4");
        tt1.start();
        tt2.start();
        tt3.start();
        tt4.start();
    }
}
```

## 网络编程

### TCP编程

模拟服务器和客户端之间的通信,先建立两个线程类，接收消息和发送消息

SendThread类

```java
public class SendThread extends Thread{
    private Socket socket;
    public SendThread(Socket socket){
        this.socket=socket;
    }
    @Override
    public void run() {
        Scanner sc = new Scanner(System.in);
        while(true){
            try {
                OutputStream os = socket.getOutputStream();
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(os));
                bw.write(sc.nextLine());
                bw.newLine();
                bw.flush();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

RaceThead类

```java
public class RaceThread extends Thread{
    private Socket socket;
    //传入socket
    public RaceThread(Socket socket){
        this.socket= socket;
    }
    @Override
    public void run() {
        while (true){
            try {
                InputStream is = socket.getInputStream();
                BufferedReader br = new BufferedReader(new InputStreamReader(is));
                String s = br.readLine();
                System.out.println("接收到数据是"+s);

            } catch (Exception e) {
                e.printStackTrace();
            }

        }
    }
}
```

Server类

```java
public class Sever {
    public static void main(String[] args) throws Exception{
        ServerSocket ss = new ServerSocket(9998);
        Socket socket = ss.accept();
        //线程1 服务器接收数据
        RaceThread rt = new RaceThread(socket);
        rt.start();
        //线程2 服务器发送数据
        SendThread st = new SendThread(socket);
        st.start();
    }
}
```

Client类

```java
public class Client {
    public static void main(String[] args) throws Exception{
        Socket socket = new Socket("localhost", 9998);
        System.out.println("连接服务器成功");
        //线程1 服务器接收数据
        RaceThread rt = new RaceThread(socket);
        rt.start();
        //线程2 服务器发送数据
        SendThread st = new SendThread(socket);
        st.start();
    }
}
```

实现多人聊天程序

```java
/**
 * 工具类，负责发送和接受消息的实现
 * */
public class SocketUtil {
    //发送消息
    public static void send(Socket s , String msg){
        try {
            OutputStream os = s.getOutputStream();
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(os));
            bw.write(msg);
            bw.newLine();
            bw.flush();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    //接收消息
    public static String recesive(Socket socket){
        try {
            InputStream is = socket.getInputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String s = br.readLine();
            return s;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

```java
/**
 * 接收消息的线程
 * */
public class RaceThread extends Thread{
    private Socket socket;
    //传入socket
    public RaceThread(Socket socket){
        this.socket= socket;
    }
    @Override
    public void run() {
        while (true){
            String content = SocketUtil.recesive(socket);
            System.out.println("接收到的消息"+content);
        }
    }
}

```

```java
/**
 * 发送消息的线程
 * */
public class SendThread extends Thread{
    private Socket socket;
    public SendThread(Socket socket){
        this.socket=socket;
    }

    @Override
    public void run() {
        Scanner sc = new Scanner(System.in);
        while(true){
           //把发送消息的功能单独提取出来
            String content = sc.nextLine();
            SocketUtil.send(socket,content);
        }
    }
}

```

```java
/**
 * 服务器发送和接收消息的线程
 * */
public class ServerMsgThread extends Thread{
    private Socket socket;
    private List<Socket> socketList;
    public ServerMsgThread(Socket socket,List<Socket> socketList){
        this.socket= socket;
        this.socketList=socketList;
    }
    @Override
    public void run() {
        while (true){
            //接收消息
            String content = SocketUtil.recesive(socket);
            //把收到的消息发送出去
            for (Socket socket1 : socketList) {
                if(socket1.equals(this.socket)){
                    continue;
                }
                SocketUtil.send(socket1,content);
            }
        }
    }
}
```

```java
/**
 * 客户端
 * */
public class Client {
    public static void main(String[] args) throws Exception{
        Socket socket = new Socket("localhost", 9998);
        System.out.println("连接服务器成功");
        new RaceThread(socket).start();
        new SendThread(socket).start();
    }
}

```

```java
/**
 * 服务器
 * */
public class Server {
    public static void main(String[] args) throws Exception{
        ServerSocket ss = new ServerSocket(9998);
        List<Socket> socketList = new ArrayList<>();
        while (true){
            Socket socket = ss.accept();
            socketList.add(socket);
            //准备一个线程接收信息，然后分别发送给其他客户端
            new ServerMsgThread(socket,socketList).start();
        }
    }
}
```



### UDP编程

Server类

```java
public class Server {
    public static void main(String[] args) throws Exception {
        //不管是发送还是接收都要使用DatagramSocket对象
        DatagramSocket ds = new DatagramSocket(9000);
        //准备接收数据,dp用来接收数据,传入字节数组和容量
        byte[] bytes = new byte[1024];
        DatagramPacket dp = new DatagramPacket(bytes,1024);
        //接收数据
        ds.receive(dp);
        //数据在字节数组中
        String s = new String(bytes, 0, dp.getLength());
        System.out.println("从客户端接收到的数据是"+s);

        //发送数据给客户端
        byte[] bytes1 = "你也不错".getBytes();
        DatagramPacket send_dp = new DatagramPacket(bytes1,bytes1.length, InetAddress.getByName("localhost"),9998);
        ds.send(send_dp);
    }
}

```

Client类

```java
public class Client {
    public static void main(String[] args) throws Exception{
        //从客户端发送数据
        //1.DatagramSocket 码头
        DatagramSocket ds = new DatagramSocket(9998);
        //传递的数据需要字节格式,Packet中需要传入属数据，数据长度，对方地址，端口号
        byte[] bs = "你好".getBytes();
        DatagramPacket dp = new DatagramPacket(bs, bs.length, InetAddress.getByName("localhost"),9000);
        //发送数据
        ds.send(dp);

        //接收服务端传过来的数据
        byte[] bytes = new byte[1024];
        DatagramPacket dp2 = new DatagramPacket(bytes, 1024);
        ds.receive(dp2);
        String s = new String(bytes, 0, dp2.getLength());
        System.out.println("服务端接受的数据有"+s);
    }
}
```

UDP模拟多人聊天室

原理，多个客户端给服务器发送信息，再由服务器分发给其他客户端，通过用户列表来存入客户端信息

```java
/**
 * 发送信息的线程
 * */
public class SendThread extends Thread{
    private DatagramSocket ds;
    public SendThread(DatagramSocket ds){
        this.ds=ds;
    }
    @Override
    public void run() {
        Scanner sc = new Scanner(System.in);
        while (true){
            try {
                String s = sc.nextLine();
                byte[] bs =s.getBytes(); //转化为字节数组
                DatagramPacket packet = new DatagramPacket(bs, bs.length, InetAddress.getByName("localhost"), 9999);
                ds.send(packet);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

```

```java
/**
 * 接收数据线程
 * */
public class ReceThread extends Thread{
    private DatagramSocket ds;
    public ReceThread(DatagramSocket ds){
        this.ds = ds;
    }

    @Override
    public void run() {
        while (true){
            try {
                byte[] bytes = new byte[1024];
                DatagramPacket dp = new DatagramPacket(bytes, 1024);
                ds.receive(dp);
                System.out.println(new String(bytes, 0, dp.getLength()));

            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

```

```java
/**
 * 客户端
 * */
public class Client {
    public static void main(String[] args) throws Exception{
        //为了测试方便，变成用户自己输入端口号
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入你的端口号");
        //为了让输入时避免回车键，把输入内容变成字符串转数字
        int port = Integer.parseInt(sc.nextLine());
        DatagramSocket ds = new DatagramSocket(port);
        //需要在客户端聊天之前发消息给服务器 ，这样才能在聊天前可以把客户端的ip和port加入到clientlist中
        byte[] bs ="有新客户端进入".getBytes(); //转化为字节数组
        DatagramPacket packet = new DatagramPacket(bs, bs.length, InetAddress.getByName("localhost"), 9999);

        //启动线程
        new SendThread(ds).start();
        new ReceThread(ds).start();
    }
}
```

```java
/**
 * 服务器端
 * */
public class Server {
    public static void main(String[] args) throws Exception {
        DatagramSocket ds = new DatagramSocket(9999);
        System.out.println("服务器开启等待连接");
        //客户端列表，存放客户端ip和端口号
        List<HashMap<String,String>> clientList = new ArrayList<>();

        while (true){
            //接收数据
            byte[] bytes = new byte[1024];
            DatagramPacket dp = new DatagramPacket(bytes, 1024);
            ds.receive(dp);
            //获取端口号和ip
            String ip = dp.getAddress().getHostAddress();
            String port = dp.getPort()+"";
            //判断客户端是否首次进入
            boolean flag =true;
            for (HashMap<String, String> list : clientList) {
                if(list.get("ip").equals(ip) && list.get("port").equals(port)){
                    //不是首次进入
                    flag =false;
                    break;
                }
            }
            if(flag=true){
                //把ip和端口加入到客户端列表中
                HashMap<String, String> map = new HashMap<>();
                map.put("ip",ip);
                map.put("port",port);
                clientList.add(map);
            }
            //其他客户端发送数据
            for (HashMap<String, String> list : clientList) {
                if(list.get("ip").equals(ip) && list.get("port").equals(port)){
                    //如果与你自己的ip相等，就跳过这次数据传输
                    continue;
                }else {
                    DatagramPacket dp2 = new DatagramPacket(bytes, 0, dp.getLength(), InetAddress.getByName(list.get("ip")), Integer.parseInt(list.get("port")));
                    ds.send(dp2);
                }
            }
        }

    }
}

```

### Java补充



