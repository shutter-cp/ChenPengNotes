**ChenPengNotes**	
[TOC]

# 多线程

## 1 多线程demo
code
```java
public class MyThredDo {
    public static class MyThred extends Thread{
        @Override
        public void run() {
            for (int i = 0; i < 20; i++) {
                try {
                    Thread.sleep(20);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("run: "+ i);
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        MyThred myThred = new MyThred();
        myThred.start();
        for (int i = 0; i < 20; i++) {
            Thread.sleep(20);
            System.out.println("main:" +i);
        }
    }
}

```
运行结果：
```
main:0
run: 0
main:1
run: 1
main:2
run: 2
main:3
run: 3
main:4
run: 4
main:5
run: 5
main:6
run: 6
main:7
run: 7
main:8
run: 8
run: 9
main:9
main:10
run: 10
main:11
run: 11
run: 12
main:12
main:13
run: 13
run: 14
main:14
main:15
run: 15
run: 16
main:16
run: 17
main:17
run: 18
main:18
run: 19
main:19
```
## 2 Thread的方法

### 2.1 获取线程的名字
- 使用Thread类中的方法getName();       
String getName();
- 可以获取到当前正在执行的线程，使用线程中的getName()获取线程的名字         
static Thread currentThread()返回当前正在执行的线程对象的引用

code：
```java
public class MyGetThreadName {
    /**
     * 获取线程名称的第一种方式
     * @Author chenpeng
     * @Description //TODO
     * @Date 20:53
     * @Param
     * @return
     **/
    public static class GetThreadName1 extends Thread{
        @Override
        public void run() {
            String threadName = getName();
            System.out.println(threadName);
        }
    }

    /**
     * 获取线程名称的第二种方式
     * @Author chenpeng
     * @Description //TODO
     * @Date 20:58
     * @Param
     * @return
     **/
    public static class GetThreadName2 extends Thread{
        @Override
        public void run() {
            Thread thread = Thread.currentThread();
            System.out.println(thread.getName());
        }
    }

    public static void main(String[] args){
        new GetThreadName1().start();
        new GetThreadName1().start();

        new GetThreadName2().start();
        new GetThreadName2().start();

        System.out.println(Thread.currentThread().getName());
    }
}
```

### 2.2 设置线程的名字
- 使用Thread中的方法setName
- 创建一个带参数的构造方法，参数传递线程的名称，调用父类的带参构造方法，把线程名传递给父类，让父类给子类线程起一个名字

code：
```java
public class MySetThreadName {
    public static class SetThreadName extends Thread{
        public SetThreadName() { }
        public SetThreadName(String threadName){
            super(threadName);
        }

        @Override
        public void run() {
            System.out.println(Thread.currentThread().getName());
        }
    }
    
    public static void main(String[] args){
        //第一种设置线程名字的方法
        SetThreadName setThreadName = new SetThreadName();
        setThreadName.setName("1号线程");
        setThreadName.start();

        //第二种设置线程名字的方法
        new SetThreadName("二号线程").start();
    }
}

```

## 2.3 sleep()
`Thread.sleep(20);`停止多少毫秒后继续执行


### 2.4 Runnable接口实现多线程
- 创建一个Runnable接口的实现类
- 在实现类中重写run()方法，设置线程任务
- 创建接口实现类的对象
- 创建一个Thread，将创建的接口实现类传入
- 调用Thread中的start()方法，开启新的线程的run方法

code:
```java
public class MyRunnable {
    public static class RunnableImpl implements Runnable{
        @Override
        public void run() {
            for (int i = 0; i < 20; i++) {
                System.out.println("run:  "+Thread.currentThread().getName()+"   "+i);
                try {
                    Thread.sleep(20);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
    public static void main(String[] args) throws InterruptedException {
        RunnableImpl runnable = new RunnableImpl();
        Thread thread = new Thread(runnable);
        thread.start();
        for (int i = 0; i < 20; i++) {
            System.out.println("run:  "+Thread.currentThread().getName()+"   "+i);
            Thread.sleep(20);
        }
    }
}
```

> 区别
> Runnable接口实现多线程的好处    
> 1. 避免了只能继承一个类的局限性
> 2. 增强了长须的扩展性，降低了程序的耦合性
>      实现了Runnable接口的方式，把设置线程任务和开启线程进行了分离（解耦）
>     

### 2.5 匿名内部类方法实现线程创建
```java
public class InnerClassThread {
    public static void main(String[] args){
        //继承Thread方法
        new Thread(){
            @Override
            public void run() {
                for (int i = 0; i < 20; i++) {
                    System.out.println(Thread.currentThread().getName()+"  "+i);
                }
            }
        }.start();

        //调用接口Runnable方法实现
        new Thread(new Runnable(){
            @Override
            public void run() {
                for (int i = 0; i < 20; i++) {
                    System.out.println(Thread.currentThread().getName()+"  "+i);
                }
            }
        }).start();

    }
}
```

## 3 多线程
> 并发: 两个或多个事件在同一个时间段内发生  
交替执行，一个人（CPU）吃两个菜（线程）   
> 并行: 两个或多个事件在同意时刻发生
同时执行，两个人（CPU）吃两个菜（线程）

## 4 线程与进程

进程：所有的应用程序都需要进入内存中执行。点击应用程序，程序进入内存，进入内存的程序叫进程。      
线程：线程属于进程，是进程中的一个执行单元，负责程的执行。是通往cpu的通道。效率高，多个线程之间互不影响。

## 5 线程调度
- 分时调度
    所有线程轮流使用CPU的使用权，平均分配每个线程占用CPU的时间。
- 抢占式调度
    优先让优先级高的线程使用CPU，如果线程的优先级相同，呢么就会随机选择一个来执行（线程随机性），java为抢占式调度。


# 线程安全_多线程_线程池
## 1. 线程安全demo
1. code：
```java
public class RunnerableImpl implements Runnable {

    /**
     * 设置电影票数量
     * @Author chenpeng
     * @Description //TODO
     * @Date 23:57
     * @Param
     * @return
     **/
    private static int ticket = 100;

    @Override
    public void run() {
        while (ticket>0){
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
                new RuntimeException();
            }
            System.out.println(Thread.currentThread().getName()+"  第"+ticket+"张票");
            ticket--;
        }
    }
}
```

```java
public class RunTest {
    public static void main(String[] args){
        RunnerableImpl run = new RunnerableImpl();

        Thread thread = new Thread(run);
        Thread thread1 = new Thread(run);
        Thread thread2 = new Thread(run);

        thread.start();
        thread1.start();
        thread2.start();
    }
}
```


2. 结果：
```
Thread-0  第100张票
Thread-1  第100张票
Thread-2  第100张票
Thread-1  第97张票
Thread-0  第97张票
Thread-2  第97张票
....
....
Thread-2  第1张票
Thread-0  第0张票
Thread-1  第-1张票
```

3. 说明：      
_为什么会出现这种情况呢?    
1.重复卖第100张票_         
**Thread-0**  **Thread-1**  **Thread-2**同时进入程序，并且在最早到`ticket--;`的线程执行之前 1，2，3最晚到达syso的线程已经到达；       
_2.为什么出现卖不存在的票？_   
因为在票数为1的时候      
**Thread-0**在代码执行完`while (ticket>0)`进入循环，执行到sleep就失去了cpu，进入等待模式       
这个时候**Thread-1**得到cpu开始执行`while (ticket>0)`进入循环，执行到sleep也失去cpu，进入等待模式        
**Thread-2**得到cpu开始执行`while (ticket>0)`进入循环，执行到sleep也失去cpu，进入等待     
都还未执行`ticket--;` 那么他们都认为还有1张票可以卖        
等1，2，3逐渐苏醒，就出现了卖第0张票和第-1张票的情况。


## 2. 如何解决线程安全问题

### 2.1 同步代码块synchronized
```
synchronized (锁对象){
   访问了共享数据的代码块
}
```
注意事项：
1. 通过代码块中的锁对象，可以使用任意对象
2. 但是必须保证多个线程使用的是同一个对象
3. 锁对象的作用：
    1. 把同步代码块锁住，只让一个线程在其中访问
     
    
```java
public class RunnerableImpl implements Runnable {

    /**
     * 设置电影票数量
     * @Author chenpeng
     * @Description //TODO
     * @Date 23:57
     * @Param
     * @return
     **/
    private static int ticket = 100;

    Object object = new Object();

    @Override
    public void run() {
        while (true){
            synchronized (object){
                if (ticket>0){
                    try {
                        Thread.sleep(20);
                    } catch (InterruptedException e) {
                        new RuntimeException();
                    }
                    System.out.println(Thread.currentThread().getName()+"  第"+ticket+"张票");
                    ticket--;
                }else{
                    break;
                }
            }
        }
    }
}
```

保证了只有一个线程在同步中执行了共享数据        
程序频繁的获取锁，释放锁，程序的效率会降低



### 2.2  同步方法 （带有synchronized修饰的方法）
1. 将访问共享数据的代码抽取出来
2. 将抽取出来的代码方法加上关键字synchronized

code:
```java
public class RunnerableImpl implements Runnable {

    /**
     * 设置电影票数量
     * @Author chenpeng
     * @Description //TODO
     * @Date 23:57
     * @Param
     * @return
     **/
    private static int ticket = 100;
    Object object = new Object();
    @Override
    public void run() {
        while (runDo()){
        }
    }
    
    public synchronized boolean runDo(){
        if (ticket>0){
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
                new RuntimeException();
            }
            System.out.println(Thread.currentThread().getName()+"  第"+ticket+"张票");
            ticket--;
            return true;
        }else{
            return false;
        }
    }
}

```

锁对象是this

### 2.3 静态同步方法
1. 在同步方法中加入 static 
2. 锁对象是 本类的class类 也就是方法的.class方法

### 2.4 Lock锁（jdk1.5之后产生的）
1. Lock锁比synchronized有更广泛的锁定义操作
2. Lock接口中常用的方法
    1. lock();
    2. unlock();
    
code:
```java
public class RunnerableImpl implements Runnable {

    private static int ticket = 100;

    Lock locl = new ReentrantLock();
    @Override
    public void run() {
        while (true){
            locl.lock();
            if (ticket>0){
                try {
                    Thread.sleep(20);
                    System.out.println(Thread.currentThread().getName()+"  第"+ticket+"张票");
                    ticket--;
                } catch (InterruptedException e) {
                    new RuntimeException();
                }finally {
                    locl.unlock();
                }
            }else {
                break;
            }
        }
    }
}
```



## 3. 等待与唤醒机制
### 3.1 线程之间的通行问题 
> 多个线程在处理同一个资源，单处理的动作不一样

生产者与消费之问题       
生产一个消费一个        

为什么要处理线程间通信问题？      
多个线程并发时，cpu是随机切换的，我们需要多个线程同时完成一件事情时，多个线程之间需要一些协调通信，达到多个线程操作一份数据

### 3.2 等待与唤醒机制
>有效的利用资源（餐厅吃东西）
通信: 对餐厅的座位坐判断

1. wait：客人看到没有座位之后，线程不在活动，不再参与调度，进入wait set中，不会去竞争，也就不会抢在cpu资源，这个时候这个线程的状态是Waiting。需要等待一个信号才能从wait set中释放出来，从新进入ready queue中去
2. notify：选取一个wait set来释放：等待最久的一个客人得到座位
3. notifyAll：释放所有的wait set

注：notify之后 也不一定是立马就执行，也是要进行cpu抢占的

code:   
1. 包子对象
```java
public class BaoZi {
    private String baozi;
    private boolean flag = false;

    public String getBaozi() {
        return baozi;
    }

    public void setBaozi(String baozi) {
        this.baozi = baozi;
    }

    public boolean isFlag() {
        return flag;
    }

    public void setFlag(boolean flag) {
        this.flag = flag;
    }
}
```


2. 生产者对象
```java
public class BaoZiBoss implements Runnable{
    private BaoZi baozi;

    public BaoZiBoss(BaoZi baozi) {
        this.baozi = baozi;
    }

    @Override
    public void run() {
        while (true){
            synchronized (baozi){
                if (baozi.isFlag()) {
                    try {
                        baozi.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }

                System.out.println("制作包子中....");
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                baozi.setBaozi("牛肉包子");
                baozi.setFlag(true);

                System.out.println("包子制作好了！");

                baozi.notify();

            }
        }
    }
}
```

3. 消费者对象
```java
public class XiaoFeiZhe implements Runnable {
    private BaoZi baoZi;

    public XiaoFeiZhe(BaoZi baoZi) {
        this.baoZi = baoZi;
    }

    @Override
    public void run() {
        while (true){
            synchronized (baoZi){
                if (baoZi.isFlag()){
                     System.out.println("我吃包子  "+baoZi.getBaozi());
                     baoZi.setFlag(false);
                     baoZi.notify();
                }

                try {
                    baoZi.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```


4. 测试类
```java
public class BaoZiDo {
    public static void main(String[] args){
        BaoZi baoZi = new BaoZi();

        new Thread(new BaoZiBoss(baoZi)).start();

        new Thread(new XiaoFeiZhe(baoZi)).start();


    }
}

```



## 4 线程池
> 线程池的容器是集合（ArrayList，HashSet，`LinkedList<Thread>`,HashMap）

当程序第一次启动时，创建多个集合，保存到一个集合中，使用线程的时候就可以从集合汇总取出一个线程来使用      
`Thread t = linked.removerFist();`  
当使用完线程之后，需要把线程归还给线程池        

在JDK1.5之后，JDK已经自带线程池        
`java.util.concurrent.Executors`线程池的工厂类，用来生成线程池


### 4.1 线程池的好处
1. 降低了资源消耗（减少了创建和销毁线程的次数，每个工作线程可以重复利用）
2. 提高响应速度，当任务到达时，任务可以不需要等到线程创建就可以立即执行
3. 提高线程的可管理性，可根据系统的承受能力，调整线程池中工作的数目


demo：
```java
public class ThreadPoolDemo1 {
      public static void main(String[] args){
          //建立线程池
          ExecutorService executorService = Executors.newFixedThreadPool(2);
          //线程池会一直
          executorService.submit(new TestThread());
          executorService.submit(new TestThread());
          executorService.submit(new TestThread());

          executorService.shutdown();
      }
}
```

阿里巴巴推荐使用：
```
推荐方式1：
  首先引入：commons-lang3包
  ScheduledExecutorService executorService = new ScheduledThreadPoolExecutor(1,
        new BasicThreadFactory.Builder().namingPattern("example-schedule-pool-%d").daemon(true).build());
     
推荐方式 2：
首先引入：com.google.guava包
    ThreadFactory namedThreadFactory = new ThreadFactoryBuilder()
        .setNameFormat("demo-pool-%d").build();

    //Common Thread Pool
    ExecutorService pool = new ThreadPoolExecutor(5, 200,
        0L, TimeUnit.MILLISECONDS,
        new LinkedBlockingQueue<Runnable>(1024), namedThreadFactory, new ThreadPoolExecutor.AbortPolicy());

    pool.execute(()-> System.out.println(Thread.currentThread().getName()));
    pool.shutdown();//gracefully shutdown
            
推荐方式 3：
   spring配置线程池方式：自定义线程工厂bean需要实现ThreadFactory，可参考该接口的其它默认实现类，使用方式直接注入bean
调用execute(Runnable task)方法即可
<bean id="userThreadPool"
        class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor">
        <property name="corePoolSize" value="10" />
        <property name="maxPoolSize" value="100" />
        <property name="queueCapacity" value="2000" />

    <property name="threadFactory" value= threadFactory />
        <property name="rejectedExecutionHandler">
            <ref local="rejectedExecutionHandler" />
        </property>
    </bean>
    //in code
    userThreadPool.execute(thread);
```
好用的方法：
```
//		    public ThreadPoolExecutor(
//  				int corePoolSize, - 线程池核心池的大小。
//                              int maximumPoolSize, - 线程池的最大线程数。
//                              long keepAliveTime, - 当线程数大于核心时，此为终止前多余的空闲线程等待新任务的最长时间。
//                              TimeUnit unit, - keepAliveTime 的时间单位。
//                              BlockingQueue<Runnable> workQueue, - 用来储存等待执行任务的队列。
//                              ThreadFactory threadFactory, - 线程工厂。

//                              RejectedExecutionHandler handler)  - 拒绝策略。
```
![image.png](https://upload-images.jianshu.io/upload_images/15992215-767dc0745e74eee3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


















