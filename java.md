##  反射

- 获取反射方式
  - 对象.getClass()
  - 类名.class
  - Class.forName(“类的全路径”)

- 获取属性

  - ```java
    getField("key")	//获取public修饰的属性
    ```

  - ```java
    getFields()//获取所有public修饰的属性
    ```

  - ```java
    getDeclaredField("key")//获取指定的属性
    ```

  - ``` java
    getDeclaredField()//获取所有属性
    ```

- 获取方法

---



## 接口抽象类区别

- 抽象类中可以有普通方法，接口中都是public abstract方法
- 抽象类中成员变量没有限制，接口中都是public static final

- 抽象类单继承，接口多实现
- 接口是约束行为的有无，强制要求某些类有相同行为，表达什么像什么
- 抽象类代码复用，表达什么是什么，比如宝马是汽车

---

## sleep()、wait()、join()、yield()区别

- ==sleep==
  - 执行该方法后，超过指定时间后继续进入就绪队列，继续竞争cpu
  - 该方法属于Thread
  - 不会释放锁
  
- ==wait==
  - 调用该方法之后当前线程会进入等待状态，只有调用了notily或者notilyAll才会重新进入就绪队列
  
  - 该方法属于object
  
  - 释放锁
  
- ==yield==
  - 执行后让出cpu执行权，进入就绪队列，但是有可能cpu下次分配时依然执行
- ==join==
  - 执行后线程进入阻塞状态，例如线程B调用A的join方法，那么B会进入阻塞队列，直到A执行完成或中断

---

## 垃圾回收算法

- ==引用计数法==
  - 每个对象都有一个计数器，新增一个引用加一，减少一个会减一，会出现循环引用（java不用）
- ==可达性分析法==
  - 从GCRoot开始向下搜索，当没有被引用到的对象会被回收
    - GCRoot包括以下几个：
      - 栈中的变量和对象
      - 方法区中静态变量
      - 本地方法区中的变量和对象
- ==GWT==（StopTheWorld----世界停止）
  - 垃圾回收算法执行时需要将JVM内存冻结，在此状态下，java所有的线程都是停止的，native可以执行，但是不可以和java代码交互


---

## java类加载器

- ==BootStrap ClassLoader==(启动类加载器)
  - 加载jdk核心类，lib

- ==Extension ClassLoade==(扩展类加载器)
  - 加载lib/ext文件夹下的jar

- ==Application ClassLoad==(应用程序类加载器)
  - 负责加载我们自己写的或者引入的文件

- ==双亲委派机制==
  - 向上委派，向下查找



---



## JVM

![image-20220428145127405](https://s2.loli.net/2022/04/28/FasO4M1ub7LYWxR.png)

- ==方法区==
  - 存储类的基本信息（本类以及父类的全限定名，访问修饰符，类型，成员变量等等）

- ==虚拟机栈==
  - 存储方法的执行过程以及局部变量等信息，每执行一个方法就进行一次压栈，执行完成出栈

- ==本地方法栈==
  - 存储一些非java实现的本地方法
- ==堆==
  
  - 存放所有对象的实例，立即回收主要就是在堆中发生所以也叫**GC堆**
  
  - 新生代（8:1:1）
    - Eden区
    - Survivor1区
    - Survivor2区
  
  - 老年代
  - 永久代（jdk8中已废除）

- ==程序计数器==
  - 记录线程执行代码的位置

---

## B树和B+树

- B树
  - 节点排序
  - 一个节点可以存多个元素
- B+树
  - 包含B树的特点
  - 叶子节点之间有指针指向下一个节点
  - 所有元素都存储在叶子节点

---

## MySQL索引类型

- ==普通索引==

  - 最基本的索引，没有任何限制

  - ``` mysql
    直接创建
    CREATE INDEX index_name ON table(column(length))	
    
    修改表结构时创建
    ALTER TABLE table_name ADD INDEX index_name ON (column(length))
    
    创建表时创建
    CREATE TABLE `table` (
        `id` int(11) NOT NULL AUTO_INCREMENT ,
        `title` char(255) CHARACTER NOT NULL ,
        `content` text CHARACTER NULL ,
        `time` int(10) NULL DEFAULT NULL ,    
        PRIMARY KEY (`id`),    
        INDEX index_name (title(length))
    
    )
    ```

- ==唯一索引==

  - 索引列必须唯一，可以为空

  - ```mysql
    创建唯一索引
    CREATE UNIQUE INDEX indexName ON table(column(length))
    
    修改表结构
    ALTER TABLE table_name ADD UNIQUE indexName ON (column(length))
    
    创建表的时候直接指定
    CREATE TABLE `table` (
        `id` int(11) NOT NULL AUTO_INCREMENT ,
        `title` char(255) CHARACTER NOT NULL ,
        `content` text CHARACTER NULL ,
        `time` int(10) NULL DEFAULT NULL ,
        UNIQUE indexName (title(length))
    
    );
    ```

- ==主键索引==

  - 主键默认的索引，不允许有空值

- ==组合索引==

  - 多个字段创建索引，使用组合索引时遵循最左前缀原则

  - ```mysql
    ALTER TABLE `table` ADD INDEX name_city_age (name,city,age);
    ```

- ==全文索引==

  - 主要用来查找文本中的关键字，只有char、varchar、text类型的字段才可以创建索引,

  - ``` a
    直接创建索引
    CREATE FULLTEXT INDEX index_content ON article(content)
    
    修改表结构添加全文索引
    ALTER TABLE article ADD FULLTEXT index_content(content)
    
    创建表的适合添加全文索引
    CREATE TABLE `table` (
        `id` int(11) NOT NULL AUTO_INCREMENT ,
        `title` char(255) CHARACTER NOT NULL ,
        `content` text CHARACTER NULL ,
        `time` int(10) NULL DEFAULT NULL ,    PRIMARY KEY (`id`),
        FULLTEXT (content)
    );
    ```

- ==注意事项==

  - 索引不能包含有null值的列。

  - 使用短索引，比如一个char(255)的列我们如果通过前十位就可以确定它那么创建索引时就可以取前十个字符

  - 使用like语句时如果字符串前面加%是不会走索引的
  - 不能使用函数进行运行，否则索引失效
  - 不能使用！=或者<>
  - 查询只能走一个索引，所以当where中有索引是那么order by中的字段不会走索引

---

## MySQL中explain关键字

- 该关键字可以分析查询语句的执行过程

- 个字段的解释：

- ==id==

  - 表示执行优先级，id越大表示越先被执行，语句中包含子查询时子查询的id比外层查询的id大

- ==select_type==

  - 查询的类型，主要用于区别**普通查询**、**联合查询**、**子查询**等等
  - **SIMPLE**(simple)：简单查询
  - **PRIMARY**(primary)：查询中包含任何复杂的子部分，最外层查询则被标记为PRIMARY。
  - **SUBQUERY**(subquery)：在SELECT或者WHERE中包含了子查询。
  - **DERIVED**(derived)：在FROM列表中包含的子查询被标记为DERIVED（衍生）。MySQL会递归执行这些子查询，把结果放在临时表里。

  - **UNION**(union)：若第二个SELECT出现在UNION之后，则被标记为UNION；若UNION包含在FROM子句的子查询中，外层SELECT将被标记为：DERIVED。
  - **UNION RESULT**(union result)：从UNION表中获取结果的SELECT。

- ==table==

  - 显示这一行数据来自那个表

- ==type==

  - 访问类型（效率从高到底）
  - **system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > All**

  - system
    - 表中只有一行数据
  - const
    - 表示通过一次索引就找到了该数据
  - eq_ref
    - 唯一索引扫描，表中只有一条记录与之对应，比如主键索引和唯一索引
  - ref
    - 非唯一性索引扫描，一个值对应多条记录
  - range
    - 只检索给定范围的行，不会全表扫描，比如条件为 id in （1，4）
  - idnex
    - 也时全表扫描，但是只遍历索引树，所以比all会快一些，比如显示一个表中的所有主键
  - all
    - 全表扫描

- ==possible_key==

  - 显示可能应用在这张表上的索引，一个或者多个，

- ==key==

  - 实际使用的索引，为null表示没有使用索引

- ==key_len==

  - 表示索引字段的最大可能长度（字节），不是实际长度，如果可以为null则加一个字节

- ==ref==
  - 显示与其他表关联的字段，如果为常量则显示const
  
- ==rows==
  - 根据索引使用情况大致估算找到记录所需要读取的行数
  
- ==extra==
- 

