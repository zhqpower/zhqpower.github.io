
title: spring学习笔记
date: 2017-02-03 19:25:42
tags:
---

# spring学习笔记
#### Ioc基础
 ###### ioc的概念：Inversion of Control,即“控制反转”

 - 将对象的实例化交给Ioc容器来完成。
    在使用sping前，如果A类需要B类，A类需要自己去负责B类的实例化。


   使用spring 控制反转，从容器中得到bean

   在spring配置文件中，可以使用Import元素来加载其他的资源文件，这样就可以把资源文件按照模块划分了

        1，按照类型拿bean,按照类型拿bean，该类型的bean在spring中只能有一个
            factory.getBean(HelloWorld.class);

        2，按照名字拿bean
        		hello=(HelloWorld)factory.getBean("hello");

        3，按照名字和类型拿bean
           hello=factory.getBean("hello",HelloWorld.class);

        4, 通过别名获取bean
         - bean的id元素只能按照标准的xml元素名称来写
              SpringMVC当中，名字就作为url的映射路径；
              Spring提供了name，在name属性里面就可以为bean起任何的名字 (别名)
              通过别名也是得到bean的实例
              在name属性中，可以通过空格/，/；来起多个别名
              一般情况下，只需要配置id就足够了。


#### spring测试

           告诉junit，使用什么环境来运行junit测试。
            SpringJUnit4ClassRunner:告诉junit，在运行JUNIT方法之前，首先启动spring容器
                   Spring会自动的把这测试对象也作为spring管理的一个bean
          直接得到Spring启动好的容器了。

          @RunWith(SpringJUnit4ClassRunner.class)

           @ContextConfiguration:用于告诉spring到哪里去加载配置文件(默认的路径是相对路径)
            假如什么配置文件都不写，spring会去当前包找 测试类名称（AliasTest）-context.xml这个文件
            spring测试的核心：测试类应该由spring管理，而不是测试类来管理spring容器。

          @ContextConfiguration

          public class AliasTest {

            告诉spring容器，把spring容器给我一个引用
             通过factory就相当于可以访问到spring容器了。


           @Autowired
           private BeanFactory factory;

           ####测试总是启动 ApplicationContext

#### ApplicationContext

          BeanFactory:是spring里面最最低层的接口，提供了最简单的容器的功能。只提供了实例化对象和拿对象的功能；BeanFactory ApplicationContext
          ApplicationContext：应用上下文，它是spring的一种更高级的容器；提供了更多的有用的功能；
          1，ApplicationContext继承了BeanFactory接口，所以，ApplicationContext也能像beanfactory从容器中得到bean；
          2，ApplicationContext提供的额外的功能：
             1，国际化的功能；
             2，消息发送/响应机制；
             3，统一加载资源的功能；
             4，AOP的功能；
          3，ApplicationContext的使用：

               手动初始化ApplicationContext

         @Test
         public void test() {
          // ClassPathXmlApplicationContext:代表，从classpath开始加载使用xml的配置文件
          ApplicationContext ctx = new ClassPathXmlApplicationContext(
            "spring/day1/container/ContainerTest-context.xml");

#### spring 实例化bean的时机

        BeanFactory在启动的时候不会去实例化bean；只有在从容器中拿bean的时候才会实例化；
        ApplicationContext在启动的时候就把所有的bean全部实例化了。
        在ApplicationContext中，可以为bean配置lazy-init=true来让bean延迟实例化；

        哪种好？
        延迟实例化的优点：应用启动的时候占用资源很少；对资源要求较高的应用，比较有优势；
        非延迟实例化的优点：
            1，所有的bean在启动的时候都加载，系统运行的速度快；
            2，在启动的时候所有的bean都加载了，我就能在系统启动的时候，尽早的发现系统中的配置问题
            3，建议web应用，在启动的时候就把所有的bean都加载了。(把费时的操作放到系统启动中完成)


### DI- Dependency Injection, 即"依赖注入"
- Ioc 容器注入某个对象所需要的外部资源

  IoC容器就是具有依赖注入功能的容器，IoC容器负责实例化、定位、配置应用程序中的对象及建立这些对象间的依赖

 由IoC容器管理的那些组成你应用程序的对象我们就叫它Bean， Bean就是由Spring容器初始化、装配及管理的对象，除此之外，bean就与应用程序中的其他对象没有什么区别了

 Spring IoC容器通过读取配置文件中的配置元数据，通过元数据对应用中的各个对象进行实例化及装配。一般使用基于xml配置文件进行配置元数据，而且Spring与配置文件完全解耦的，可以使用其他任何可能的方式进行配置元数据，比如注解、基于java文件的、基于属性文件的配置都可以。
