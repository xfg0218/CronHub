# CronHub 学习总结
 ## github介绍
https://github.com/sharpstill/CronHub

## 使用教程参考
http://www.mamicode.com/info-detail-1755577.html

## 下载cronhub源代码
  * 下载源码
   https://github.com/sharpstill/CronHub 下载master分支的源代码

## 导入到开发工具中

## 下载tomcat8.0
下载地址:http://tomcat.apache.org/

## 给项目配置JDK与添加tomcat1.8的JAR
 * 把项目中的JDK替换成JDK1.8的即可
 * 如下所示添加tomcat1.8的JAR的包
在CronHub-master/WebRoot/WEB-INF下创建tomcat1.8文件夹，并把第5步下载的tomcat1.8的lib下的所有的JAR
复制到刚才创建的目录下并添加到环境变量中即可


## 安装mysql5.6数据库
  * 在线安装mysql5.6数据库
   
   service mysqld stop

   yum remove mysql mysql-*
   
   yum list installed | grep mysql

   rpm -e --nodeps `rpm -qa | grep mysql`
   
   rpm -Uvh http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
   
   yum install mysql-community-server
   
   mysql -V
   
   service mysqld start


 * 修改root密码开启远程链接
 
  * 修改密码
  
    注意:第一次进入时没有密码，直接点击回车即可

    mysql -uroot -p
    
    mysql> use mysql;
    
    Reading table information for completion of table and column names
    
    You can turn off this feature to get a quicker startup with -A
    
    Database changed;
   
    mysql> update user set password=password("1234") where user='root';
    
    Query OK, 5 rows affected (0.01 sec)
    
    Rows matched: 5  Changed: 5  Warnings: 0

    mysql> flush privileges;
    Query OK, 0 rows affected (0.00 sec)


 * 开启远程链接
  * 开启mysql的远程登录权限
  
    mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '1234' WITH GRANT OPTION;
    Query OK, 0 rows affected (0.00 sec)

  * 刷新使之立刻生效
  
    mysql> flush privileges;

    Query OK, 0 rows affected (0.00 sec)



 * 创建cronhub需要的数据库
 
  * 下载mysql脚本
  
    https://download.csdn.net/download/xfg0218/10356913

   * 把SQL导入到mysql中
   
    mysql> source  /home/xiaoxu/cronhub_manage_system.sql


## 修改配置
 
  * 修改application.properties
   * 主要修改CronHub-master/config/application.properties 中的数据库链接信息


  * 修改192.168.*.*.properties 配置文件
   * 主要修改 /CronHub-master/ant/192.168.*.**.properties 修改成自己需要部署的IP地址
   * 修改 CronHub-master/ant/build.xml 所有的IP地址，并修改自己需要部署的IP地址

  * 修改install_start.sh脚本
   * 修改 CronHub-master/WebRoot/download/install_start.sh 的55行处加入base_java_home=/opt/jdk1.8.0;
   * 在128行处修改成 ... -home  ${base_java_home}  -Xmx2000m -pidfile ...


## 对项目进行打包
 * 对CronHub-master 打包
 * 找到CronHub-master/ant/build.xml在build.xml上右击, 找到 run as >> Ant build  ，出现以下信息表示以成功打包
   
    ×××××××××

    BUILD SUCCESSFUL

    Total time: 9 seconds

 * 查看打包完的war包
 
   如下所示会出现一个war文件夹

## 把项目部署到tomcat1.8中
 * 删除apache-tomcat-8.5.30/webapps/ROOT下的所有文件
  * cd  /opt/apache-tomcat-8.5.30/webapps/ROOT
  * rm -rf *

 * 上传项目到服务器的tomcat8.0的webapps/ROOT下
 
  * cp CronhubManageSystem_101.9.war root@192.168.0.99:/opt/apache-tomcat-8.5.30/webapps/ROOT

* CronhubManageSystem_101.9.war 
 * jar  -xvf CronhubManageSystem_101.9.war 

* 启动tomcat8.0

  * cd  /opt/apache-tomcat-8.5.30/bin

  * ./startup.sh;tail -F ../logs/catalina.2018-04-17.log

   当出现以下日志表示启动成功
    
   ×××××××××

   17-Apr-2018 07:45:06.459 INFO [main] org.apache.coyote.AbstractProtocol.pause Pausing ProtocolHandler ["http-nio-8080"]
   
   17-Apr-2018 07:45:06.513 INFO [main] org.apache.coyote.AbstractProtocol.pause Pausing ProtocolHandler ["ajp-nio-8009"]
   
   17-Apr-2018 07:45:06.566 INFO [main] org.apache.catalina.core.StandardService.stopInternal Stopping service [Catalina]



## 在crontab的机器上安装daemon

 * 安装daemon
 
   /opt/apache-tomcat-8.5.30/webapps/ROOT/download

   ./install_start.sh -d /opt/modules/daemon -s 2012 -i 192.168.0.1 -p 8080

* 参数的含义

  -d install directory path
  
  -s daemon boot start port
  
  -i cronhub center server's ip used for download daemon's jar and jdk and jsvc and so on

  -p cronhub center server's port used for download daemon's jar and jdk and jsvc and so on



## 查看cronhub的web界面
 * 记录查询界面
 * 未完成列表界面
 * 任务调度界面
 * 添加daemon界面
 × daemon 检查和管理界面CronHub
