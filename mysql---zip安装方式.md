# mysql---zip安装方式

## SET1 下载

[mysql下载官网直达链接]: https://dev.mysql.com/downloads/mysql/

![img](https://pic2.zhimg.com/80/v2-221d19c8c921321a9c5fbc84caae2e71_720w.jpg)

![img](https://pic2.zhimg.com/80/v2-868a0250730ae85ab40be2f3c1a21eb5_720w.jpg)



然后找个盘符建个文件夹解压这个mysql.zip包

E:\MYSQL我的文件安装位置

![image-20200331091336957](C:\Users\limfff\AppData\Roaming\Typora\typora-user-images\image-20200331091336957.png)

## SET2 创建my.ini

创建my.ini配置文件，不要创建data文件夹，不要创建data文件夹，不要创建data文件夹！！！重要的事情说三遍。

首先创建 my.ini配置文件

创建配置文件注意编码格式，注意编码格式！否则导致报错。这里我们的编码格式为utf-8，不是带bom的utf-8.如果还启动不成功，试试ANSI编码。

后期启动不了服务别又暴毙了。

### 步骤1 新建my.ini

新建文本文件

![image-20200331151815921](C:\Users\limfff\AppData\Roaming\Typora\typora-user-images\image-20200331151815921.png)

打开另存为

### 步骤二 改名my.ini 注意编码和保存类型

![image-20200331151922858](C:\Users\limfff\AppData\Roaming\Typora\typora-user-images\image-20200331151922858.png)

### 步骤三 my.ini写入配置

```text
[mysqld] 
# 设置 3306 端口 
port=3306 
# 设置 mysql 的安装目录 
basedir=E:\MYSQL\mysql-8.0.19-winx64
# 设置 mysql 数据库的数据的存放目录
datadir=E:\MYSQL\mysql-8.0.19-winx64\data
# 允许最大连接数 
max_connections=200 
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统 
max_connect_errors=10 
# 服务端使用的字符集默认为 UTF8 
character-set-server=utf8 
# 创建新表时将使用的默认存储引擎 
default-storage-engine=INNODB 
# 默认使用“mysql_native_password”插件认证 
default_authentication_plugin=mysql_native_password 
[mysql] 
# 设置 mysql 客户端默认字符集 
default-character-set=utf8 
[client] 
# 设置 mysql 客户端连接服务端时默认使用的端口 
port=3306 
default-character-set=utf8
```

**记住修改两处地址**

![image-20200331152218838](C:\Users\limfff\AppData\Roaming\Typora\typora-user-images\image-20200331152218838.png)

### 步骤四 配置环境变量

##### 配置MYSQL_HOME环境变量

```text
新建变量名：MYSQL_HOME   变量值：E:\MYSQL\mysql-8.0.19-winx64 点击确定
```

![image-20200331152459399](C:\Users\limfff\AppData\Roaming\Typora\typora-user-images\image-20200331152459399.png)

##### 配置path环境变量

```text
新建变量：%MYSQL_HOME%\bin 点击确定
之前一条就够了，现在保险起见，加入一条：E:\MYSQL\mysql-8.0.19-winx64
，也就是你安装位置下的bin文件夹。
```

![image-20200331152703156](C:\Users\limfff\AppData\Roaming\Typora\typora-user-images\image-20200331152703156.png)

## SET3 通过cmd 命令创建data文件夹

### 创建data文件夹：

（不要手动创建data文件夹，否则可能后面启动mysql服务启动不了！启动不了清删除data文件夹，重新执行以下代码。执行后出现data文件夹 ps：每执行一段代码都要点击回车键）

~~~text
mysqld --initialize-insecure
~~~

### 初始化语句：

（对照图c-命令窗口一条一条实行，执行后打开data文件夹出现初始化文夹）

~~~text
mysqld --defaults-file=E:\Mysql\mysql-8.0.19-winx64\my.ini --initialize –console
~~~

### 安装MySQL：

~~~text
mysqld install
~~~

### 进行MySQL初始化，执行后创建root用户：

~~~text
mysqld --initialize-insecure --user=mysql
~~~

### 启动MySQL服务：（显示启动成功为正确）

~~~text
net start mysql 
~~~

### 启动后你的root用户密码为空，设置密码的为123456：（密码改不改自己随意）

~~~text
mysqladmin -u root -p password 123456
~~~

### 回车出现 “Enter password” 不用输入直接点击回车下一步

### 登录用户：

~~~text
mysql -u root -p
（回车后，输入密码即可 ，之前设置的密码为123456）
~~~

成功登陆！安装完成

## 容易出现的问题：

~~~text
这是由于你计算机缺少这个文件，只需要下载下来丢到C:\Windows\System32下面即可，同样的群里面提供这个文件的下载。
~~~



![img](https://pic3.zhimg.com/80/v2-e87fa4d51a738f47af914a6a4213a75a_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-167523f9d8e787c114f4c9079e794d62_720w.jpg)



## **安装失败可能的原因：**

## **一：重装MySQL但是没有卸载干净**

~~~text
方式一
打开命令窗口，输入 net stop mysql 关闭MySQL服务 ，然后删除MySQL的服务输入：

sc delete MySQL
~~~

~~~text
方式二：打开任务管理器找到有关MySQL服务关闭，然后删除或卸载所有有关Mysql文件。
~~~

## **二：执行第二条初始化语句后出现乱码**

~~~text
安装目录中，文件夹避免出现中文字符，请使用英文命名文件夹。
~~~

## **三：MySQL服务启动不了**

~~~text
上面也说了，可能是你手动创建了data文件夹，删除data文件夹，然后重新执行上面所有的命令。

也可能是my.ini配置文件的编码问题，删除data文件夹和my.ini配置文件，重新来过。
~~~

-----------------------------------根据学习（知乎@一叶知秋）

链接：https://zhuanlan.zhihu.com/p/88271915