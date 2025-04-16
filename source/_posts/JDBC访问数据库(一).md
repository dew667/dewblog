---
title: JDBC访问数据库(一)
---

JDBC(Java Data Base Connection)即通过Java访问数据库

#### 1.导入mysql-jdbc的jar包

访问MySql官网，找到Connector/J选择合适的版本下载。这里我用的Java8和mysql5.5能够和官网提供的Connector/J5.1.47版本很好地兼容。[下载地址](https://dev.mysql.com/downloads/connector/j)  
通常将项目用到的jar包放在项目的lib目录下，然后在eclipse中导入该jar包  
导包步骤： 右键project->property->java build path->libaries->add external jars
 <!--more-->
#### 2.初始化驱动

以下代码是用来初始化驱动的：  
`Class.forName("com.mysql.jdbc.Driver");`  
初始化驱动类com.mysql.jdbc.Driver就在之前导入的jar包中  
初始化b抛出的异常类名为ClassNotFoundException

示例代码：

    package jdbc;
       
    public class TestJDBC {
        public static void main(String[] args) {
               
            //初始化驱动
            try {
                //驱动类com.mysql.jdbc.Driver
                //就在 mysql-connector-java-5.1.47-bin.jar中
                //如果忘记了第一个步骤的导包，就会抛出ClassNotFoundException
                Class.forName("com.mysql.jdbc.Driver");
                  
                System.out.println("数据库驱动加载成功 ！");
       
            } catch (ClassNotFoundException e) {
                //出错
                e.printStackTrace();
            }
               
        }
    }

#### 3.建立与数据库的连接

需要mysql中已经创建了相应的数据库

    package jdbc;
      
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.SQLException;
      
    public class TestJDBC {
        public static void main(String[] args) {
      
            try {
                Class.forName("com.mysql.jdbc.Driver");
      
                // 建立与数据库的Connection连接
                // 这里需要提供：
                // 数据库所处于的ip:localhost:3306 (本机)
                // 数据库的端口号： 3306 （mysql专用端口号）
                // 数据库名称 jdbc_study
                // 账号 root
                // 密码 admin
      
                Connection c = DriverManager
                        .getConnection(
                                "jdbc:mysql://localhost:3306/jdbc_study",
                                "root", "admin");
      
                System.out.println("连接成功，获取连接对象： " + c);
      
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            } catch (SQLException e) {
                e.printStackTrace();
            }
      
        }
    }

#### 4.创建Statement

Statement是用于执行SQL语句的，比如增加，删除

    package jdbc;
      
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.SQLException;
    import java.sql.Statement;
      
    public class TestJDBC {
        public static void main(String[] args) {
      
            try {
                Class.forName("com.mysql.jdbc.Driver");
      
                Connection c = DriverManager
                        .getConnection(
                                "jdbc:mysql://localhost:3306/jdbc_study",
                                "root", "admin");
      
                // 注意：使用的是 java.sql.Statement
                // 不要不小心使用到： com.mysql.jdbc.Statement;
                Statement s = c.createStatement();
      
                System.out.println("获取 Statement对象： " + s);
      
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            } catch (SQLException e) {
                e.printStackTrace();
            }
      
        }
    }

#### 5.执行SQL语句

s.execute执行sql语句

    package jdbc;
      
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.SQLException;
    import java.sql.Statement;
      
    public class TestJDBC {
        public static void main(String[] args) {
      
            try {
                Class.forName("com.mysql.jdbc.Driver");
      
                Connection c = DriverManager
                        .getConnection(
                                "jdbc:mysql://localhost:3306/jdbc_study",
                                "root", "admin");
      
                Statement s = c.createStatement();
      
                // 准备sql语句
                // 注意： 字符串要用单引号'
    	    //Java变量两侧j用+号，再套上双引号""
                String sql = "insert into hero values(null,'提莫',"+313.0f+","+50+")"; 
                s.execute(sql);
      
                System.out.println("执行插入语句成功");
      
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            } catch (SQLException e) {
                e.printStackTrace();
            }
      
        }
    }

#### 6.关闭连接

关闭连接的顺序：  
先关闭Statement，后关闭Connection

    package jdbc;
     
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.SQLException;
    import java.sql.Statement;
     
    public class TestJDBC {
        public static void main(String[] args) {
     
            Connection c = null;
            Statement s = null;
            try {
                Class.forName("com.mysql.jdbc.Driver");
     
                c = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc_study", "root",
                        "admin");
     
                s = c.createStatement();
     
                String sql = "insert into hero values(null,'提莫'," + 313.0f + "," + 50 + ")";
     
                s.execute(sql);
     
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            } catch (SQLException e) {
                e.printStackTrace();
            } finally {
                // 数据库的连接时有限资源，相关操作结束后，养成关闭数据库的好习惯
                // 先关闭Statement
                if (s != null)
                    try {
                        s.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                // 后关闭Connection
                if (c != null)
                    try {
                        c.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
     
            }
     
        }
    }

#### 7.查询语句

executeQuery 执行SQL查询语句

    package jdbc;
     
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.ResultSet;
    import java.sql.SQLException;
    import java.sql.Statement;
     
    public class TestJDBC {
        public static void main(String[] args) {
            try {
                Class.forName("com.mysql.jdbc.Driver");
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
     
            try (Connection c = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc_study",
                    "root", "admin"); Statement s = c.createStatement();) {
     
                String sql = "select * from hero";
     
                // 执行查询语句，并把结果集返回给ResultSet
                ResultSet rs = s.executeQuery(sql);
                while (rs.next()) {
                    int id = rs.getInt("id");// 可以使用字段名
                    String name = rs.getString(2);// 也可以使用字段的顺序
                    float hp = rs.getFloat("hp");
                    int damage = rs.getInt(4);
                    System.out.printf("%d\t%s\t%f\t%d%n", id, name, hp, damage);
                }
                // 不一定要在这里关闭ReultSet，因为Statement关闭的时候，会自动关闭ResultSet
                // rs.close();
     
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }