##### spring aop

 aop 采用的是横向抽取机制。 这个是相对于纵向继承来说的。

 主要采用的是代理机制：

静态代理：
   1. 真实对象  
   2. 代理对象  
   3. 共同需要实现的接口  
   4. 代理对象持用真实对象的引用。

**** 实现runnable 接口的线程类，也采用的是静态代理的方式。

动态代理：
1. 真实对象，实现某一个接口
2. 代理类
3. 在代理类中通过反射，生成一个代理对象。 
4. 代理类实现 InvocationHandler, 可以在代理类的invoke 方法中进行增强


##### 代理机制

  1. jdk 动态代理
         只能对实现了接口的类生成代理, 实际上是生成了一个真是对象的代理类，利用反射。

  2. CGLib 代理
         可以对所有类生成代理, 实际上是生成了一个真实对象的子类。


        1. 真实对象，没有实现接口

        2. 代理类中创建 代理对象。 利用核心类Enhancer类中的方法

             //设置父类
             enHancer.setSuperclass(productDao.getClass);
 
             // 设置回调,代理类实现MethodInterceptor接口
               enHancer.setCallback(this);

         3. 在代理类中 的intercept 中对方法进行增强      
 
             
     spring框架中，如果目标类实现了接口，就会使用jdk动态代理。 如果目标类中没有实现接
     口，就会使用cglib动态代理。  底层会自动切换。


 ##### AspecJ 一个基于java 语言的AOP 框架。


 #####  AOP 中的术语

    1. Joinpoint(连接点):

        所有可以被拦截（增强）的方法

    2. Pointcut (切入点):
        
        真正被拦截(增强)的方法
        
    3. Advice (通知/增强)

        增强的代码（比如 记录日志的代码），一般指的是方法级别的增强。

         1> 前置通知 --> 目标方法执行前运行

         2> 后置通知

         3> 环绕通知

         4> 异常抛出通知
 
         5> 引介通知 （类级别的）



    4. Introduction(引介)

        一个特殊方式的增强, 类级别的增强。 在原有类上添加一个属性或者方法。

    5. Target (目标对象)

        要增加的那个类。

    6. Weaving (织入)
       
     把增加应用到目标对象创建出信心的代理对象的过程， 也就是把日志记录增加到add方法之前的过程

    7. Proxy(代理)：
     
      一个类被aop 织入增强，就会产生一个代理类。

    8. 切面：

      切入点和通知的结合   （在add 方法上应用增强）， 允许有多个切点和多个advice

       1> Advisor: 不带有切点的切面。 （也就是对目标类所有的方法进行增强）

       2> PointCutAdvisor: 具有切点的切面。 （也就是对目标类中某些方法进行增强）

         
##### spring aop 使用过程：

1. 导入spring aop 相关的jar包

2. 确定被代理的对象
      1》接口+实现类

3. 编写增加的代码 --> 前置增强 可以实现MethodBeforeAdvice 接口     

4. 生成代理对象， spring 中可以通过配置生成代理
 
    通过 ProxyFactoryBean 来生成代理
 
 以上方式生成代理对象，每个目标类都要进行配置。这样不合理。

##### 自动代理

1. BeanNameAutoProxyCreator 根据Bean 名称创建代理

2. DefaultAdvisorAutoProxyCreator 根据Advisor 本身包含的信息创建代理

3. AnnotationAwareAspectJAutoProxyCreator 基于Bean 中的AspectJ 注解 进行自动代理


******自动代理是基于后处理bean（在Bean 创建的过程中完成的增强。 生成Bean 就是代理 ）


   


