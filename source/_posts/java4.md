---
title: MySQL与Java的连接
date: 2018-06-05 12:50:00
tags:
    - Java
    - MySQL
categories: Java
catalog: true
---

通过实现[菜鸟教程上的一个例程](http://www.runoob.com/java/java-mysql-connect.html)来记录一下这个操作。

Java与MySQL的连接主要依赖一个JDBC驱动来进行。
## 建立一个测试的新库

通过下面一行命令

```
mysqladmin -u root -p create jdbctest
```

即建立了一个名为jdbctest的数据库。此时这个数据库中还没有tables。我们进入到数据库中并创建一个新的table

```sql
sql> CREATE TABLE `websites` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` char(20) NOT NULL DEFAULT '' COMMENT '站点名称',
  `url` varchar(255) NOT NULL DEFAULT '',
  `alexa` int(11) NOT NULL DEFAULT '0' COMMENT 'Alexa 排名',
  `country` char(10) NOT NULL DEFAULT '' COMMENT '国家',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;
```

即创建了一个新table。然后给这个表中插入数据：

```sql
sql> INSERT INTO `websites` VALUES ('1', 'Google', 'https://www.google.cm/', '1', 'USA'), ('2', '淘宝', 'https://www.taobao.com/', '13', 'CN'), ('3', '菜鸟教程', 'http://www.runoob.com', '5892', ''), ('4', '微博', 'http://weibo.com/', '20', 'CN'), ('5', 'Facebook', 'https://www.facebook.com/', '3', 'USA');

```
一个测试的数据库就建好了。
如果需要导入数据库结构文件的话，通过这个方式：
导入方法：在已经use 这个数据库之后，通过下面命令


```
mysql> source path-to-'filename.sql'
```

即可快速方便的导入数据建立索引。数据这时就已经导入了。

## 与Java进行连接
### JDBC配置
连接数据库需要一个驱动包Java 连接 MySQL 需要驱动包，[最新版下载地址](http://dev.mysql.com/downloads/connector/j/) ，解压后得到jar库文件，然后在对应的项目中导入该库文件。

## 查看端口号
通过命令

```
mysql> show global variables like 'port';
```
即可查看端口，一般情况下默认端口号是3306。所以如果没有自己去修改端口号的话，这一步可以省略。

## Demo代码

```java

package jdbctestpack;
import java.sql.*;

public class jdbctest {
    //JDBC 驱动名及数据库URL
    static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
    static final String DB_URL = "jdbc:mysql://localhost:3306/jdbctest";

    //数据库的用户名与密码，需要根据自己的设置
    static final String USER = "root";
    static final String PASS = "xxxxxx";
    //此处放自己的数据库密码

    public static void main(String[] args){
        Connection conn = null;
        Statement stmt = null;
        try{
            //注册JDBC驱动
            Class.forName(JDBC_DRIVER);

            //打开链接
            System.out.println("链接数据库...");
            conn = DriverManager.getConnection(DB_URL,USER,PASS);

            //执行查询
            System.out.println("实例化Statement对象...");
            stmt = conn.createStatement();
            String sql;
            sql = "SELECT id, name, url FROM websites";
            ResultSet rs = stmt.executeQuery(sql);

            //展开结果集数据库
            while(rs.next()){
                //通过字段检索
                int id = rs.getInt("id");
                String name = rs.getString("name");
                String url = rs.getString("url");
                System.out.print("ID: " + id);
                System.out.print(", 站点名称" + name);
                System.out.print(", 站点 URL: " + url);
                System.out.print("\n");
            }
            //完成后关闭
            rs.close();
            stmt.close();
            conn.close();
        }catch (SQLException se){
            //处理JDBC错误
            se.printStackTrace();
        }catch(Exception e){
            //处理Class.forName错误
            e.printStackTrace();
        }finally {
            {
                //关闭资源
                try{
                    if(stmt!=null) stmt.close();
                }catch(SQLException se2){
                 //什么都不做
                }
                try{
                    if(conn!=null) conn.close();
                }catch (SQLException se){
                    se.printStackTrace();
                }
            }
            System.out.println("Goodbye!");
        }
    }
}

```

执行之后的输出结果如下：

![success](http://upload-images.jianshu.io/upload_images/11400909-80fd63263e2f2843.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

即连接成功。

***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
