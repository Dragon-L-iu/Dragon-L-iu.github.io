# java连接Access数据库（详细）,你get到了吗？

## 建立数据库

1.首先下载office2016专业版，里面含有Access，具体下载可以百度。
2.打开Access，新建数据库



![新建数据库](https://img-blog.csdnimg.cn/20191209181645834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RyYWdvbmVkXzEyMw==,size_16,color_FFFFFF,t_70)

点击设计视图，保存表的名字为student

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191209181800485.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RyYWdvbmVkXzEyMw==,size_16,color_FFFFFF,t_70)



建立字段名称，定义主键（主键字段的数据不能出现重复的内容）



![在这里插入图片描述](https://img-blog.csdnimg.cn/2019120918214456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RyYWdvbmVkXzEyMw==,size_16,color_FFFFFF,t_70)

返回到数据表视图



![在这里插入图片描述](https://img-blog.csdnimg.cn/20191209182255958.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RyYWdvbmVkXzEyMw==,size_16,color_FFFFFF,t_70)

输入相应的数据



![在这里插入图片描述](https://img-blog.csdnimg.cn/20191209182509644.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RyYWdvbmVkXzEyMw==,size_16,color_FFFFFF,t_70)

## 连接数据库
我使用的是IDEA，以IDEA演示
==Access驱动文件在[点击这里](http://java.hhit.edu.cn/UserFiles/111004/files/20181212145939827.zip)==

首先导入Access驱动文件到idea里面
1.打开File选项，找到Project Structure



![在这里插入图片描述](https://img-blog.csdnimg.cn/20200103134721571.png)

2.找到Global Libraries



![在这里插入图片描述](https://img-blog.csdnimg.cn/20200103134831571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RyYWdvbmVkXzEyMw==,size_16,color_FFFFFF,t_70)

3.点击加号



![在这里插入图片描述](https://img-blog.csdnimg.cn/20200103135044547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RyYWdvbmVkXzEyMw==,size_16,color_FFFFFF,t_70)

4.找到Acess驱动包的路径



![在这里插入图片描述](https://img-blog.csdnimg.cn/20200103135135285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RyYWdvbmVkXzEyMw==,size_16,color_FFFFFF,t_70)



5.点击OK就导入好Access驱动了。

运行这段代码
```java
import java.sql.*;

/**
 * @ author Dragon
 * @ version
 */

public class TestConSQL {
    public static void main(String[] args) {

        try {
            Class.forName("com.hxtt.sql.access.AccessDriver");//导入Access驱动文件，本质是.class文件
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        try {
            Connection con = DriverManager.getConnection("jdbc:Access:///C:\\Users\\lenovo\\Desktop\\Datas.accdb","","");
            //与数据库建立连接，getConnection()方法第一个参数为jdbc:Access:///+文件总路径,第二个参数是用户名，第三个参数是密码（Access是没有用户名和密码此处为空字符串）
            Statement sta = con.createStatement();
            ResultSet res = sta.executeQuery("select * from student");
            //向数据库发送executeQuery()方法的数据库语句，对数据库返回的结果放到ResultSet里面
            //注意一定要保证数据库语句的正确性
            while(res.next()){//不断的移动光标到下一个数据
                System.out.println(res.getString(1)+res.getString(2)+res.getInt(3));
                //注意如果年龄为数值，需要getInt()
            }
            con.close();//关闭数据库连接
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```
运行结果
123456张三23
234567李四20
345678王二21
456789麻子23

==不同的功能对应不同executeQuery()方法的数据库语句==
 Access 2010：

驱动类：com.hxtt.sql.access.AccessDriver
连接串：jdbc:Access:///+DatabaseName

如果是其它数据库：
SQL SERVER:

驱动类：com.microsoft.sqlserver.jdbc.SQLServerDriver
连接串：jdbc:sqlserver://localhost:1433;DatabaseName=test

ORACLE:
驱动类：oracle.jdbc.driver.OracleDriver
连接串：jdbc:oracle:thin:@127.0.0.1:1521:test


MYSQL：

驱动类：com.mysql.jdbc.Driver
连接串：jdbc:mysql://localhost:3306/sample_db?user=root&password=your_password