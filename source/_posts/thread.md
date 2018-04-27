####多线程学习



##### 线程的停止

   使用外部干预，定义一个标志flag。 在线程体中以这个标志作为条件。 在线程类中提供设置标志的方法。

   ```java

   class Study implements Runnable{

    private boolean flag=true;

    @Override
    public void run() {
        while (flag){
            System.out.println("study thread...");
        }

    }

    public void stop(){
        this.flag=false;
    }
}

   ```


##### join 合并线程

  main线程中join 的其他线程， main线程阻塞，等待join进来的线程执行完成后，继续执行main线程中的业务。

##### yield 暂停线程

 在当前线程中调用 Thread.yield()，会暂停当前线程的执行，去调用其他线程。

 ##### sleep 

 在当前线程中调用 Thread.sleep()， 会阻塞当前线程，不会释放锁。


##### 同步 也称为并发。

synchronized

 可以是锁住一个方法，同一时间，只允许一个方法进行访问。

 可以锁住一块代码




####总结 
 
   1. 多线程可以通过继承Thread 类，实现runnable 或者callable 接口三中方法来创建线程类， 可以通过一个一个线程类 开启 多个线程，线程体中的资源对于每一个线程都是独立的。  也可以通过不同的线程类开启 多线程，当多个不同的线程访问 同一资源是就发生的并发问题，也就是同步问题。 

   2. 线程 的状态 : 线程被创建后，可能处于运行或者 就绪状态（等待cpu 时间片）， 执行过程中如果遇到sleep，会切换到阻塞状态。时间到来后，自己到就绪状态， 如果遇到wait，将释放所，不再执行，等待被notify 。

   3. 解决同步问题时，需要用同一个锁，锁住所有相关的线程体。



#### 被satatic 修饰的成员变量 被 对象 所共享。 

    静态会随着 类的加载而加载，消失而消失。 生命周期很长，对象不存在了，静态修饰的变量还在。  


##### sychronized关键字

 1. 对象锁，一般使用objcet

 2. 同步方法锁， 锁的是this , 也就是当前类的 对象。

 3. 静态方法锁， 锁的是字节码对象， 当前类.class

##### 等待唤醒机制

wait(), notify(), notifyAll() 这三个方法，必须使用在同步代码块中，因为是对持用锁的线程进行的操作。

调用时，必须指明线程持用的锁。---> 锁.notify(); 

一个锁的wait 必须是同一把锁notify   A.wait() -----> A.notify();


