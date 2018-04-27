title: spring学习笔记02
date: 2017-02-03 19:25:42
tags: spring
categories: 后端
---

#### spring 中bean生命周期

1. bean 对象的实例化

2. 封装属性

3. 如果bean 实现了BeanNameAware 执行setBeanName

4. 如果bean 实现了BeanFactoryAware 或者 ApplicationContextAware  执行设置工厂 setBeanFactory 或者 执行设置上下文对象 setApplicationContext 

5. 如果存在类，实现了BeanPostProcessor (后处理bean)，执行postProcessBeforeInitialization


6. 如果Bean实现了InitializingBean 执行afterPropertiesSet

7. 调用 <bean init-method="init"> 执行指定的init 方法

8. 如果存在类实现了BeanPostProcessor (处理bean), 执行postProcessAfterInitialization

9. 执行业务处理

10. 如果Bean 实现了DisposableBean 执行destory

11. 调用<bean destroy-mothod="destorymethod"> 执行指定的销毁方法 destorymethod


#### 类中属性注入的方式：
 
 1. 接口注入

 2. 构造器注入

 3. setter 方法注入


spring 中支持 构造器和setter方法注入, p名称空间注入，SPEL



