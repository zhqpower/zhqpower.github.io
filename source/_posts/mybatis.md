#### mybatis 

1. mybatis 介绍
  
      是开源的持久型框架，


2. jdbc 的问题

   1> 数据库连接频繁的被创建，释放， 浪费系统资源。 如果使用数据库连接池就会解决这个问题，

   2> SQL 语句写在Java 代码中，存在硬编码问题

   3> 使用preparedStatement 向占位符传参数存在硬编码。

   4> 对结果解析存在硬编码


3. mybatis 架构


    1> mysql 配置文件 （SqlMapConfig.xml+ 若干个和实体类(表)对应的mapper.xml）

    2>  SqlSessionFactory (工厂中的原材料就是1 提供的)

    3>  工厂会产生一个SqlSession，其实SqlSession 就已经可以操作数据库了

    4> Executor 当Sqlsession 调用对数据库的增删改查时，实际上是通过Executor 来执行的. Executor 在Sqlsession 里面

    5> MappedStatement 
      sql 语句被存放在一个pojo 对象里，这里MapperStatement 对象就是作为存放 sql 的pojo 来存在的。 MappedStatement 在Executor里面 

    6> 在MappedStatement 这个环节，有输入和输出

      输入: Map 类型 ，String Integer 等基础数据类型，pojo

      输出:  Map 类型 ，String Integer 等基础数据类型，pojo, List
        
        在MappedStatement向数据库发送请求后，会将查询结果自动映射成输出支持的类型。

4. mybatis 实战初级版
 
     1. 主配置文件 SqlMapConfig.xml  (主要用于配置别名和mapper文件信息)
     
        1> 在configuration标签下使用typeAliases 变迁可以配置别名或者指定包路径

        ```java

            <!-- 别名 包以其子包下所有类   头字母大小都行-->
              <typeAliases>
                    <!-- 		<typeAlias type="cn.xiechengxu.pojo.User" alias="User"/> -->
                    <package name="cn.xiechengxu.pojo"/>
              </typeAliases>

        ```

        2> 在configuration标签下使用environments 配置数据库连接相关的信息。这个和spring整合后就可以废弃掉



        3> 在configuration标签下使用 mappers 配置和实体类（数据表）对应的mapper.xml 文件


        ```java

          <mappers>
          <mapper resource="sqlmap/User.xml"/>
          </mappers>
           ```

2.  在mapper.xml 文件中写sql语句

    ```java
    <mapper namespace="test">
      
      <select id="findUserById"  parameterType="Integer" resultType="cn.xiechengxu.pojo.User">
        select * from user where id = #{v}
      </select>
      
    </mapper>

    ```

3. 在测试类中 构建 SqlSession 来执行sql 语句

   ```java

    String resource="SqlMapConfig.xml";
        InputStream in=Resources.getResourceAsStream(resource);

        SqlSessionFactory sqlSessionFactory=new SqlSessionFactoryBuilder().build(in);

        //创建SqlSession
          SqlSession sqlSession=sqlSessionFactory.openSession();
          //执行sql
          User user=sqlSession.selectOne("test.findUserById",10);
          System.out.println(user);

   ```

      
##### mapper 文件中在写sql时，#{v} 和${value}的区别

  1. 前者可以防止sql注入，后者不行

  2. 前者是占位符，select * from user where id=? 占位符 ? == '五' 
 
   #{} 表示字符串拼接 select * from user where username like '%五%  

   和占位符相比，少了单引号


   应为不能防止sql 注入，所有可以将其改写为#{} 这个占位符的形式

     	select * from user where username like "%"#{value}"%"


  ${} 这种写法，sql注入，可以通过传入 参数  "五%' or '1%' ='1"  这样会生成这样的sql 

     select * from user where username like '%五%' or '1%' ='1%'    
 
  3. 获取插入数据主键ID (使用selectKey 这个标签)

     ```java

      <insert id="insertUser" parameterType="cn.xiechengxu.pojo.User">

       <selectKey keyProperty="id" resultType="Integer" order="AFTER">
		   select  LAST_INSERT_ID()
	     </selectKey>

		insert into user (username,birthday,address,sex ) values (#{username},#{birthday},#{address},#{sex})

	    </insert>
          
     ```

  4. SqlMapConf.xml 中配置别名，可以使用包的形式。 配置map.xml 的位置，也可以使用包。 但是接口类和map.xml 必须在同一个包下，而且名称要相同。 map.xml--map.java    这样可以利用mybatis 提供的mapper 动态代理机制，自动生成一个接口的实现类。
   要使用mapper动态代理机制，map.xml 文件中命名空间的配置必须执行map接口类的全路径

   
##### resultType 和 resultMap
 
   返回值类型，可以使用resultType 自动映射 ，也可以使用resultMap手动映射。

   当pojo 里面的属性名和数据表中的字段名不一致时，使用resultMap作手动映射

    
     result 中的标签： id表示 id属性
                   result 表示其他字段属性


##### 动态sql （四大标签）

   
1. if 标签

   ```xml
 
    <if test="sex != null and sex !='' ">
			 sex=#{sex}
		</if>


   ```

2. where 标签

   使用<where> 标签包裹调教部分，可以去掉前and。 免得每次都要写1==1

3. sql 片段 
  
    提取公共的sql 片段

   ```xml
    
     <sql id="selector">
		select * from user
	   </sql>

   ```

   使用时，用include

   ```xml

      <select id="findUserBySexAndName" parameterType="User" resultType="User">
		<include refid="selector"/>
		<where>
		<if test="sex != null and sex !='' ">
			 sex=#{sex}
		</if>

		<if test="username != null and username !='' ">
			and  username=#{username}
		</if>
		</where>

	  </select>

   ```
    

4. foreach 标签


 如果传入的是vo类，那么collection 是vo类中集合的名称

 如果传入的是List， 那么collection 是 list

 如果传入的是数组， 那么collection 是array

 

   
```xml

 <select id="selectUserByids"  parameterType="QueryVo" resultType="User">

		 <include refid="selector"/>
		
		<where>

			
			<foreach collection="array"  item="id" separator="," open="id in (" close=")">

				#{id}

			</foreach>
			
		</where>

	</select>

```

 
 ##### 一对一关联查询

   1. 返回值用resultMap

   2. resultMap 中使用association 做关联

        ```xml
            
            <resultMap id="order" type="Orders">
              <id column="id" property="id"/>
              <result column="user_id" property="userId"/>
              <result column="number" property="number"/>
              <result column="createtime" property="createtime"/>
              <result column="note" property="note"/>

              <association property="user" javaType="User">
                <result column="username" property="username"/>
              </association>

	          </resultMap>

        ```


##### 一对多关联 collection



   


##### mybatis 与spring的整合

  思想：
   1. SqlSessionFactory 对象放入spring 容器中作为容器存在

   2. 传统的dao 开发中，从spring 容器中获得sqlsession对象

         写一个是接口，写一个实现类。写一个mapper.xml  文件和接口不在同一个包下是
         要使用<mapper resource=""/> 这个标签。

         在ApplicationContext.xml 中做如下配置：

          ```xml
            
           <!--原始dao-->
          <bean id="userDao" class="cn.xiechengxu.mybatis.dao.UserDaoImpl">

            <property name="sqlSessionFactory" ref="sqlSessionFactoryBean"/>

          </bean>

          ```

      注入dao的是实现，并且在dao实现中注入依赖，sqlSessionFactory

   3. Mapper 代理形式中，从spring容器中直接获取mapper的代理对象
          
        写一个mapper.java 和mapper.xml 文件，并且放到同一个包下面，这样在sqlMapconfig.xml 文件中
        <mappers> 标签下引入mapper文件时使用包的形式 <package name="mapper文件包路径"/>

       在applicationContext.xml 文件中配置代理mapper

       ```xml

        <!--代理mapper-->
          <bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">

            <property name="sqlSessionFactory" ref="sqlSessionFactoryBean"/>

            <property name="mapperInterface" value="cn.xiechengxu.mybatis.mapper.UserMapper"/>

          </bean>

       ```

      配置了 MapperFactoryBean，以来注入了sqlSessionFactoryBean 和mapper 接口。 这样就可以直接产生mapper 对象。

     mapper代理曾强版，动态扫描.使用MapperScannerConfigurer 类

      配置文件 

          ```xml

            <!--动态代理增强版，扫描-->

            <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <property name="basePackage" value="cn.xiechengxu.mybatis.mapper"/>
             </bean>  

          ```

 4. 数据库的连接以及数据库连接池事物管理都交给spring容器来完成。




