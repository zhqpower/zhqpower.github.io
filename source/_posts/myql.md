##### mysql 数据优化

1. 表的设计要合理（满足3NF） 3范式

2. 创建适当的索引 （主键索引| 唯一索引| 普通索引 | 全文索引 | 空间索引）

3. 对SQL 语句优化 --> 定位慢查询（explain）

4. 使用 分表技术（水平和垂直分表）

5. 读写分离

6. 创建适当的存储过程，函数，触发器

7. 对my.ini 优化。 优化配置

8. 软硬件升级


##### 范式(最高级6NF)

 1NF: 表的属性（列） 具有庸原子性，即表的列不能再分割。本身含义精准,如列名为省或市 就不满足了。 不能有重复的列

       只要是关系型数据库，天然满足1NF
  
       
2NF: 表中不能存在完全一样的一条记录，就满足2NF。一般情况通过设置一个主键来搞定。而且该主键是自增的。


3NF： 如果列的内容可以推导出来（显示，隐士），就不要单独用一列来存放。

    可以用一个外键关联, 来推导出来。

反3NF： 通常情况下，表设计要严格遵守3NF， 有时也有例外。     



#####  优化策略

  1. 使用索引来搞定

     1》 添加主键索引 --> alert table  emp add primary key (empno)

  2. 定位慢查询

      默认情况下，mysql 不会记录慢查询。 在测试时，可以指定mysql记录慢查询。


      mysql 慢查询日志文件中 Query_Time  查询时间， Lock_time: 等待时间


3. explain 的使用

   基本用法：explain + sql\G 


#####  索引

  1. 索引的创建
     1》 主键索引的创建： a. 创建表的时候，指定某列或者某几列为主键，这样就有了主键索引。
                        b. 创建完表后， 指定主键索引。

         主键索引的特点：一个表只能有一个主键。 一个主键可以指向多列。（复合主键） 主键索引效率最高。 主键索引的列，不能为空，不能重复

    2》唯一索引的创建，      

                a. create unique index + 索引名称 on +表名+ （列名） 必须指定索引名称

                b，alert table + 表名 + add unique on + （列名)

      特点: 一张表中可以有多个唯一索引。 唯一索引如果指定not null， 唯一索引可以为null，而且可以有多个。  唯一索引效率也高，可以考虑多个



   3》 普通索引

      a.  create   index + 索引名称 on  +表名+ （列名）
      b.  alter table +表名 add index +（列名）

     特点： 一张表中可以有多个普通索引， 一个普通索引可以指向多列


    4》 全文索引

        主要针对文章，汉子，英文的检索，可以快速检索到文章中的关键字

         mysql 默认的全文索引，只对MyISAM 存储引擎起作用。 只支持英文。


        

  2. 索引的查询:
     1> desc 表名
     2> show keys  from 表名
     3> show index from 表名
     4> show indexes form 表名
    
      
 3. 删除索引

    drop index 索引名称 on 表名

    alter table 表名 drop index 索引名

        
 ####sql 语句优化和索引的正确使用

   1. 如果创建了复合索引，只要查询条件使用了最左边的列，索引一般会被使用。

      ```xml

      ALTER TABLE dept add index (dname,loc);

      EXPLAIN select * from dept where dname='xxxxx'  //索引会被使用到

      EXPLAIN select * from dept where loc='xxxxx'

      ```

 
  2. 在like 语句中，如果以 % 、 _ 开头不会使用索引

  3. 如果条件中有or, 则要求or 的所有字段都必须有索引。 


  4. group by +  order by null 进行优化

  5. 尽量使用连接查询

  6.  索引使用情况查询

  show status like 'Handler_reader%'

    handler_read_key 值越大，表示使用索引查询到的次数越多

    handler_read_md_next 值越大，查询低效

  7. 对于精度要求比较高的， 数据类型要使用decimal 不要使用float
   
  ##### 分表技术和分区技术

    当一个表很大时，可以考虑添加索引，当索引不能解决时，可以使用分表或者分区技术。


       水平分表是指 将大表拆分到不同的小表。   每个小表的结构和大表结构是一样的。

  
   
