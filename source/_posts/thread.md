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
 
   1. 多线程可以通过继承Thread 类，实现runnable 或者callable 接口三中方法来创建线程类， 可以通过一个线程类 开启 多个线程，线程体中的资源对于每一个线程都是独立的。  也可以通过不同的线程类开启 多线程，当多个不同的线程访问 同一资源是就发生的并发问题，也就是同步问题。 

   2. 线程 的状态 : 线程被创建后，可能处于运行或者 就绪状态（等待cpu 时间片）， 执行过程中如果遇到sleep，会切换到阻塞状态。时间到来后，自己到就绪状态， 如果遇到wait，将释放锁，不再执行，等待被notify 。

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


##### Lock

  1. ReentrantLock ： 可重入，抢不到，不要无限循环。到队列中等待，等到锁被释放后通知去抢

  2. Semaphore： 需要多个线程获取同一把锁去做一件事

  3. ReadWriteLock: 读写锁，写的时候锁住，读的时候可以允许多个线程。 为了保证可重入性，可以使用ReentrantReadWriteLock

  4. CountDownLatch: 一个线程需要等待其他线程结束后才工作。（有个计数器，某个线程结束，计数器减一，如果技术器为0，那么一直等待的线程就可以工作）

  5. CyclicBarrier: 几个线程必须互相等待


 ##### Lock 接口中的方法

   1. lock 方法，获取锁，需要在finally 中调用 unlock 释放锁。

   2. tryLock 方法，会立即返回boolean 值，true 得到锁， false 未获取锁

   3. lockInterruptibly 方法， 如果有两个线程，线程1 先获得锁，并且阻塞了。 线程2 可以调用interrupt 方法就 可以中断线程2





###### 线程池的使用
    1. single Thread Executor:  只有一个线程的线程池，提交的任务按顺序执行。 Excutors.newSingleThreadExecutor()  相当于单线程


    2. Cached Thread Pool: 线程池里有很多线程需要同时执行，老的可用线程 被新任务处罚重新执行，如果线程超过60s 没有被执行，那么将被终止并从线程池中移除。Excetors.newCachedThreadPool()


    3. Fixed Thread Pool: 有固定线程数的线程池，如果没有任务执行，就一直等待。 Excutors.newFixedThreadPool(4)
     
      
         一般和cpu 保持一致，int cpuNum=RunTime。getRunTime().avaliableProcessors()


    4. Scheduled Thread pool: 用来调用即将执行的任务的线程池 excutors.newScheduledThreadPool()


    5. Single Thread Scheduled Pool: 只有一个线程，用来调度任务在指定时间执行。



##### 向线程池中提交任务:

 1.  通过excute 方法传入runnable 接口


   ```java

    	pool.execute(new Runnable() {
				@Override
				public void run() {
					System.out.println("thread name: " + Thread.currentThread().getName());
					try {
						Thread.sleep(1000);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
		});


   ```

2. 通过 submit 方法 传入一个Callable 接口， 从Future中get结果，这个方法是会被阻塞的，一直要等到线程任务返回结果。
    
    ```java

            Future<String> submit = pool.submit(new Callable<String>(){
				@Override
				public String call() throws Exception {
					//System.out.println("a");
					Thread.sleep(5000);
					return "b--"+Thread.currentThread().getName();
				}			   
			   });
			//从Future中get结果，这个方法是会被阻塞的，一直要等到线程任务返回结果
			System.out.println(submit.get());

    ```

 3. 对于schedulerPool来说，调用schedule提交任务时，则可按延迟，按间隔时长来调度线程的运行    

   ```java

    	submit = exec.schedule(new TaskCallable(i), random.nextInt(10), TimeUnit.SECONDS);
  
   ```



 #####   	BlockingQueue<String>  用来控制线程的同步工具

    主要方法： 
      
         插入： add（obj）把一个对象加入到BlockingQueue 中，如果可以容纳，返回true，否则抛出异常
          
               put（obj）把一个对象加入BlockingQueue 中，如果可以容纳，返回true，否则线程被阻断，等有空间了加入，否则一直阻塞。

              offer（obj） 把一个对象加入到BlockingQueue 中，如果可以容纳，返回true，否则 返回fase

         
         读取： pull（time） 取出BlockingQueue 中排在首位的对象，如果不能立即取出 等待time 时间，如果还是没有，返回null

               take(): 取出时，如果BlockingQueue 为空，则阻塞。 
            




