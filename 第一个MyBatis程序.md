# Mybatis学习笔记

## MyBatis的第一个程序

思路：搭建环境--->导入MyBatis--->编写代码--->测试

### 1.搭建环境

搭建数据库

![image-20201001170819493](mybatisimage/image-20201001170819493.png)

> 使用了数据库已经存在的数据库

### 2.创建项目

新建一个普通的maven项目![image-20201001171317226](mybatisimage/image-20201001171317226.png)

删掉 **src** 文件夹当作父工程

![image-20201001172125032](mybatisimage/image-20201001172125032.png)

### 3.导入依赖

导入所有需要maven的依赖![image-20201001172346646](mybatisimage/image-20201001172346646.png)

### 4.编写配置文件

从官方文档获取**mybaits-config.xml**文件内容

![image-20201001172858463](mybatisimage/image-20201001172858463.png)

根据自己的配置的需求填写数据

![image-20201001174059347](mybatisimage/image-20201001174059347.png)

```url
&amp:代表&，为转义字符
useSSL=true:代表安全连接
useUnicode=true&ampcharaterEncoding=UTF-8：指定字符的编码·解码格式，避免乱码
存数据时：数据库在存放项目数据的时候会先用UTF-8格式将数据解码成字节码，然后再将解码后的字节码重新使用GBK编码存放到数据库中。
取数据时：在从数据库中取数据的时候，数据库会先将数据库中的数据按GBK格式解码成字节码，然后再将解码后的字节码重新按UTF-8格式编码数据，最后再将数据返回给客户端。
另还有时区的配置：serverTimezone=GMT%2B8：指定时区
例如：
在java代码里面插入的时间为：2018-06-24 17:29:56
但是在数据库里面显示的时间却为：2018-06-24 09:29:56
```

### 5.编写mybatis工具类

严格按照官方文档编写

**官方文档**

***

![image-20201001175141616](mybatisimage/image-20201001175141616.png)

SqlSession相当于JDBC的preparestatement![image-20201001175233278](mybatisimage/image-20201001175233278.png)

***

编写工具类

![](mybatisimage/image-20201001174758266.png)

### 6.编写代码

+ 实体类![image-20201001180155882](mybatisimage/image-20201001180155882.png)

+ Dao接口![image-20201001180305798](mybatisimage/image-20201001180305798.png)

+ 接口实现类

  **不需要实现接口类，只需要编写配置文件**

  _**官方文档**_

  ***

  ![image-20201001233654816](mybatisimage/image-20201001233654816.png)

  ***

  实现代码：

  ![image-20201001234408655](mybatisimage/image-20201001234408655.png)

  ```id
  注意：id为dao接口的方法的名字
  ```


### 7.编写测试代码

```注意
注意：测试代码所在的包要与被测试的代码包位置相同
```

![image-20201001235603214](mybatisimage/image-20201001235603214.png)

### 8.测试中遇到问题

``` 
配置文件没有注册 绑定接口错误 方法名不对 返回类型不对 maven导出资源问题
```

![image-20201002002228896](mybatisimage/image-20201002002228896.png)

```tiki wiki
注意：mybatis-config没有注册mapper
```

![image-20201002104004657](mybatisimage/image-20201002104004657.png)

```
资源过滤问题：找不到userMapper.xml文件 
```

**注意：**

![image-20201002104224043](mybatisimage/image-20201002104224043.png)

### 9.修改代码

修改pom文件，防止资源无法被找到（父工程和子工程都添加）

![image-20201002104356038](mybatisimage/image-20201002104356038.png)

注册mapper.xml

![image-20201002104503786](mybatisimage/image-20201002104503786.png)

```
本人遇到了问题：
java.security.cert.CertPathValidatorException: Path does not chain with any of the trust anchors
解决方案：把url中的useSSL=true改为false
```

### 10.拓展![image-20201002105745047](mybatisimage/image-20201002105745047.png)





## 增删改查实现（CRUD）

### 1.namespace

namespace中的报名要和Dao/Mapper接口的包名一致

### 2.select

选择，查询语句；

+ id：就是对应的namespace中的方法名；
+ resultType:Sql语句执行的返回值！
+ parameterType:参数类型！

#### 代码实现

​	dao层代码实现:

![image-20201002131713170](mybatisimage/image-20201002131713170.png)

配置文件编写： ![image-20201002131909944](mybatisimage/image-20201002131909944.png)

测试代码：![image-20201002132245980](mybatisimage/image-20201002132245980.png)



### 3.insert

#### 代码实现

+ dao层代码实现

![image-20201002132849634](mybatisimage/image-20201002132849634.png)

+ 配置文件编写![image-20201002132943922](mybatisimage/image-20201002132943922.png)

+ 测试代码编写

![image-20201002133236812](mybatisimage/image-20201002133236812.png)

```
注意：增删改需要提交事务
```

### 4.update

#### 代码实现

+ dao层
+ ![image-20201002133701499](mybatisimage/image-20201002133701499.png)
+ 配置文件编写![image-20201002133635097](mybatisimage/image-20201002133635097.png)
+ 测试代码编写![image-20201002133741037](mybatisimage/image-20201002133741037.png)

### 5.delete

#### 代码实现

+ dao层

  ![image-20201002133947815](mybatisimage/image-20201002133947815.png)

+ 配置文件编写

![image-20201002134040652](mybatisimage/image-20201002134040652.png)

+ 测试代码编写![image-20201002134104990](/mybatisimage/image-20201002134104990.png)



### 6.总结

编写接口--->编写配置文件--->编写测试代码

（注意：增删改需要提交事务：SqlSession.commit()）

###  7.分析错误

```
注意：读错要从下往上读
```

+  标签不要匹配错

+ resource绑定mapper,需要使用路径

+ 程序配置文件必须符合规范

+ NullPointerExceptiion,没有注册到资源

+ 输出的xml文件中存在中文乱码问题

+ ###### maven资源没有导出问题

### 8.万能Map

```
假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们应当考虑使用Map!
```

#### 插入案例

+ dao层代码编写

![image-20201002191223962](mybatisimage/image-20201002191223962.png)

+ 配置文件编写

![image-20201002191641408](mybatisimage/image-20201002191641408.png)

+ 测试代码编写

![image-20201002192028418](mybatisimage/image-20201002192028418.png)

#### 获取案例

+ dao层代码

![image-20201002220454736](mybatisimage/image-20201002220454736.png)

+ 配置文件编写

![image-20201002220633334](mybatisimage/image-20201002220633334.png)

+ 测试代码编写

![image-20201002224142412](mybatisimage/image-20201002224142412.png)

```
Map传递参数，直接在sql中取出key即可！
对象传递参数，直接在sql中取都西昂的属性即可！
只有一个基本类型参数的情况下，可以直接在sql中取到
多个参数用Map,或者注解
```

### 9.模糊查询

```
关键词：like
```

+ dao层代码编写

![image-20201002224704375](mybatisimage/image-20201002224704375.png)

+ 配置文件编写

  （1）第一个方案：执行代码的时候，传递通配符%%

![image-20201002225100248](mybatisimage/image-20201002225100248.png)

​		（2）第二个方案：在sql拼接中使用通配符

![image-20201002225536076](mybatisimage/image-20201002225536076.png)

+ 测试代码编写

![image-20201002225127525](mybatisimage/image-20201002225127525.png)



## 配置解析

### 1.核心配置文件

+ mybatis-config.xml
+ MyBatis的配置文件包含了会深深影响MyBatis行为的设置和属性信息 

![image-20201002230033981](mybatisimage/image-20201002230033981.png)

### 2.创建一个新的项目

![image-20201002230418422](mybatisimage/image-20201002230418422.png)

### 3.环境设置（environments）

Mybatis可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

```
注意，要选择另一个环境只需把<environments default="新的环境的id">(下图中原来的值为development)
```

![image-20201002230856747](mybatisimage/image-20201002230856747.png)

### 4.属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性都是可外部配置且可动态替换的，既可以在典型的java属性文件中配置，亦可通过properties元素的子元素来传递。

编写一个配置文件

![image-20201002234103146](mybatisimage/image-20201002234103146.png)

注意，一定要按照顺序配置properties

![](mybatisimage/image-20201002234324821.png)

第一种方式：

![image-20201002234637195](mybatisimage/image-20201002234637195.png)

第二种方式

![image-20201002234738319](mybatisimage/image-20201002234738319.png)

+ 可以直接引入外部文件
+ 可以在其中增加一些属性配置
+ 如果两个文件有同一个字段，优先使用外部配置文件的！

### 5.类型别名（typeAliases ）

+ 类型别名可为 Java 类型设置一个缩写名字
+ 存在的意义仅在于用来减少冗余的全限定类名书写
+ 实例：

![image-20201002235544606](mybatisimage/image-20201002235544606.png)

也可以指定一个包名，MyBatis会在包名下面搜索需要的javabean,比如：

扫描实体类的包，它的默认别名就为这个类的别名，首字母小写

![image-20201003000006556](mybatisimage/image-20201003000006556.png)

在实体类比较少的时候，使用第一种方式

如果实体类十分多，建议使用第二种。

第一种可以DIY别名，第二种则不行，如果非要改，需要在实体类上增加注解

![image-20201003000311748](mybatisimage/image-20201003000311748.png)

### 6.设置

这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为

![image-20201003000937479](mybatisimage/image-20201003000937479.png)

![image-20201003000958537](mybatisimage/image-20201003000958537.png)

### 7.映射器（Mappers）

MapperRegistry:注册绑定我们的Mapper文件;

![image-20201003001747690](mybatisimage/image-20201003001747690.png) 

```
注意点：
使用class:
1:接口和他的Mapper配置文件必须同名 
2:接口和他的Mapper配置文件必须在同一个包下

使用扫描包进行注入绑定
1:接口和他的Mapper配置文件必须同名 
2:接口和他的Mapper配置文件必须在同一个包下
```

### 8.生命周期和作用域

理解我们之前讨论过的不同作用域和生命周期类别是至关重要的，因为错误的使用会导致非常严重的**并发问题**

流程：

![image-20201003003034016](mybatisimage/image-20201003003034016.png)

![image-20201003003638864](mybatisimage/image-20201003003638864.png)

```
这里的每一个Mapper,就代表一个具体的业务
```

**SqlSessionFactoryBuilder**

+ 一旦创建了，就不再需要它
+ 局部变量

**SqlSessionFactory**

+ 可以想象为：数据库连接池
+ 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例
+ SqlSessionFactory 的最佳作用域是应用作用域
+ 使用单例模式或者静态单例模式

**SqlSession**

+ 连接到连接池的一个请求！
+ SqlSession 的实例不是线程安全的，因此是不能被共享的
+ 用完之后需要赶紧关闭，否则占用资源