## 高版本MySQL（5.7,5.8）的JDBC连接新问题

在使用JDBC连接访问MySQL时一般要使用对应版本的驱动。 比如我使用了8.0.11版本的MySQL，对应驱动的Maven描述为：

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.11</version>
</dependency>12345
```

然后遇到了驱动问题、SSL安全访问的问题和时区问题。 
报错1：

```
Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdbc.Driver'. The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.1
```

高版本的驱动已经由

```
"com.mysql.jdbc.Driver"1
```

变更为

```
private static final String DB_DRIVER = "com.mysql.cj.jdbc.Driver";1
```

报错2：

```
Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
创建连接失败,查看控制台异常信息12
```

这个是安全连接的问题，在sql连接字符串中，添加关于不使用SSL访问数据数据库的说明：useSSL=false。

报错3：

```
java.sql.SQLException: The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.1
```

这个是时区问题，在访问字符串中添加关于时区的设置说明：serverTimezone=UTC。

完整的数据库访问字符串为：

```
private static final String DB_URL = "jdbc:mysql://localhost:3306/jdbc_example?characterEncoding=utf8&useSSL=false&serverTimezone=UTC";
```