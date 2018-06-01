##### dubbo 的使用

 当web 表现层和服务层分开，放在不同的工程（war包）中时。 表现层和服务层需要通信， 比较高效的就是使用中间件 dubbo

 dubbo 的使用过程：

1. 服务的提供者，启动容器。 向注册中心进行注册。

2. 服务的调用方，会向注册中心询问（subscribe)

3. 注册中心会向调用方返回 服务信息（ip，端口后等）

4. 服务调用方就可以调用服务服务

5. 存在一个监控中心，可以监控服务之间的调用次数等（monitor）
    
     官方提供了一个dubbo-admin war包作为监控中心，部署到tomcat下即可。

    如果监控中心和注册中心放在同一台机器上，可以不用做任何配置。否则要修改 tomcat/webapp/dubbo-admin/WEB-INF/dubbo.properties

    监控中心再jdk1.8 下面会出问题

##### dubbo 的使用方法
 
 服务的发布方增加 <dubbo:service interface="xxx" ref="服务的实现">

  ```xml

  <bean id="xxxService" class="cn.xiechengxu.xxxServiceImpl"/>

 <dubbo:protocol name="dubbo" port="20880"/>
 <dubbo:service interface="cn.xiechengxu.xxxService" ref="xxxService">

 ```

服务的调用方增加 <dubbo:reference id="xxx" interface="和服务提供方接口名一致">

  ```xml
    <dubbo:reference id="xxxService" interface="cn.xiechengxu.xxxService">

     <bean id="xxxAction" class="cn.xxx.XxxAction">
     <property name="xxxService" ref="xxxService">
     </bean>

  ```

服务提供方和调用方都要配置注册中心: <dubbo:registry protocol="zookeeper" address="localhost:2181"/>



#### mybatis 分页插件 PageHelper

