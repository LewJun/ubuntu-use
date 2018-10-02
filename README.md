# ubuntu-use
ubuntu 18.04.1

[TOC]

## 安装Oracle 11gr2
[from](https://askubuntu.com/questions/566734/how-to-install-oracle-11gr2-on-ubuntu-14-04)
### Step 1: [Download](http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html) Oracle Database Express Edition.
### Step 2: Instructions before install Oracle
1. Copy the downloaded file and paste it in home directory.

2. Unzip using the command:

> unzip oracle-xe-11.2.0-1.0.x86_64.rpm.zip 
3. Install required packages using the command:

> sudo apt-get install alien libaio1 unixodbc
4. Enter into the Disk1 folder using command:

> cd Disk1/
5. Convert RPM package format to DEB package format (that is used by Ubuntu) using the command:

> sudo alien --scripts -d oracle-xe-11.2.0-1.0.x86_64.rpm
6. Create the required chkconfig script using the command:

> sudo pico /sbin/chkconfig
The pico text editor is started and the commands are shown at the bottom of the screen. Now copy and paste the following into the file and save:
``` bash
#!/bin/bash
# Oracle 11gR2 XE installer chkconfig hack for Ubuntu
file=/etc/init.d/oracle-xe
if [[ ! `tail -n1 $file | grep INIT` ]]; then
    echo >> $file
    echo '### BEGIN INIT INFO' >> $file
    echo '# Provides: OracleXE' >> $file
    echo '# Required-Start: $remote_fs $syslog' >> $file
    echo '# Required-Stop: $remote_fs $syslog' >> $file
    echo '# Default-Start: 2 3 4 5' >> $file
    echo '# Default-Stop: 0 1 6' >> $file
    echo '# Short-Description: Oracle 11g Express Edition' >> $file
    echo '### END INIT INFO' >> $file
fi
update-rc.d oracle-xe defaults 80 01
```

7. Change the permission of the chkconfig file using the command:

> sudo chmod 755 /sbin/chkconfig  
8. Set kernel parameters. Oracle 11gR2 XE requires additional kernel parameters which you need to set using the command:

> sudo pico /etc/sysctl.d/60-oracle.conf
9. Copy the following into the file and save:
``` bash
# Oracle 11g XE kernel parameters 
fs.file-max=6815744  
net.ipv4.ip_local_port_range=9000 65000  
kernel.sem=250 32000 100 128 
kernel.shmmax=536870912 
```
10. Verify the change using the command:

> sudo cat /etc/sysctl.d/60-oracle.conf 
11. You should see what you entered earlier. Now load the kernel parameters:

> sudo service procps start
12. Verify the new parameters are loaded using:

> sudo sysctl -q fs.file-max
You should see the file-max value that you entered earlier.

13. Set up /dev/shm mount point for Oracle. Create the following file using the command:

> sudo pico /etc/rc2.d/S01shm_load
14. Copy the following into the file and save.
``` bash
#!/bin/sh
case "$1" in
start)
    mkdir /var/lock/subsys 2>/dev/null
    touch /var/lock/subsys/listener
    rm /dev/shm 2>/dev/null
    mkdir /dev/shm 2>/dev/null
*)
    echo error
    exit 1
    ;;

esac 
```

15. Change the permissions of the file using the command:

> sudo chmod 755 /etc/rc2.d/S01shm_load

16. Now execute the following commands:
``` sh
sudo ln -s /usr/bin/awk /bin/awk 
sudo mkdir /var/lock/subsys 
sudo touch /var/lock/subsys/listener
```
Now, Reboot Your System

### Step 3: Install Oracle
1. Install the oracle DBMS using the command:

> sudo dpkg --install oracle-xe_11.2.0-2_amd64.deb
2. Configure Oracle using the command:

> sudo /etc/init.d/oracle-xe configure 
```
Oracle Database 11g Express Edition Configuration
-------------------------------------------------
This will configure on-boot properties of Oracle Database 11g Express 
Edition.  The following questions will determine whether the database should 
be starting upon system boot, the ports it will use, and the passwords that 
will be used for database accounts.  Press <Enter> to accept the defaults. 
Ctrl-C will abort.

Specify the HTTP port that will be used for Oracle Application Express [8080]:8080

/etc/init.d/oracle-xe: line 362: netstat: command not found
Specify a port that will be used for the database listener [1521]:1521

/etc/init.d/oracle-xe: line 405: netstat: command not found
Specify a password to be used for database accounts.  Note that the same
password will be used for SYS and SYSTEM.  Oracle recommends the use of 
different passwords for each database account.  This can be done after 
initial configuration:root123456
Confirm the password:root123456

Do you want Oracle Database 11g Express Edition to be started on boot (y/n) [y]:y

Starting Oracle Net Listener...Done
Configuring database...
Done
Starting Oracle Database 11g Express Edition instance...Done
Installation completed successfully.

```
3. Setup environment variables by editting your .bashrc file:

> pico ~/.bashrc
4. Add the following lines to the end of the file:
``` sh
export ORACLE_HOME=/u01/app/oracle/product/11.2.0/xe
export ORACLE_SID=XE
export NLS_LANG=`$ORACLE_HOME/bin/nls_lang.sh`
export ORACLE_BASE=/u01/app/oracle
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH
export PATH=$ORACLE_HOME/bin:$PATH
```

5. Load the changes by executing your profile:

> . ~/.bashrc
6. Start the Oracle 11gR2 XE:

> sudo service oracle-xe start
7. Add user YOURUSERNAME to group dba using the command:

> sudo usermod -a -G dba lewjun
### Step 4: Using the Oracle XE Command Shell
1. Start the Oracle XE 11gR2 server using the command:

> sudo service oracle-xe start
2. Start command line shell as the system admin using the command:

> sqlplus sys as sysdba
Enter the password that you gave while configuring Oracle earlier. You will now be placed in a SQL environment that only understands SQL commands.

3. Create a regular user account in Oracle using the SQL command:

> create user lewjun identified by lewjun;
Replace USERNAME and PASSWORD with the username and password of your choice. Please remember this username and password. If you had error executing the above with a message about resetlogs, then execute the following SQL command and try again:

> alter database open resetlogs;
4. Grant privileges to the user account using the SQL command:

> grant connect, resource to USERNAME;
Replace USERNAME and PASSWORD with the username and password of your choice. Please remember this username and password.

5. Exit the sys admin shell using the SQL command:

> exit;
6. Start the commandline shell as a regular user using the command:

> sqlplus
Now, you can run sql commands...


## react
### 使用 create-react-app 快速构建 React 开发环境
```
cnpm install -g create-react-app
create-react-app helloworld
cd helloworld
npm start
npm run build
sudo cnpm install -g serve
serve -s build
```

## npm
国内使用 npm 速度很慢，可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
> $ sudo npm install -g cnpm --registry=https://registry.npm.taobao.org

> $ sudo npm config set registry https://registry.npm.taobao.org

> $ sudo cnpm install -g [name]

## mariadb 安装
* 安装数据库环境
> sudo apt install -y mariadb-server mariadb-client

* 安装数据库实例
> sudo mysql_secure_installation
```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

Change the root password? [Y/n] n
lewjun@ubuntu:~$ sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

Change the root password? [Y/n] n
 ... skipping.

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] n
 ... skipping.

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

* 查看MariaDB数据库服务状态 （这里也说明了MariaDB与其他Mysql数据库不能共存在同一操作系统）
> sudo systemctl status mysql
```
● mariadb.service - MariaDB 10.1.34 database server
   Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: 
   Active: active (running) since Fri 2018-09-07 06:17:52 HKT; 5min ago
     Docs: man:mysqld(8)
           https://mariadb.com/kb/en/library/systemd/
 Main PID: 11942 (mysqld)
   Status: "Taking your SQL requests now..."
    Tasks: 27 (limit: 4915)
   CGroup: /system.slice/mariadb.service
           └─11942 /usr/sbin/mysqld

Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: Processing databases
Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: information_schema
Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: mysql
Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: performance_schema
Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: Phase 6/7: Checking and u
Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: Processing databases
Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: information_schema
Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: performance_schema
Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: Phase 7/7: Running 'FLUSH
Sep 07 06:17:56 ubuntu /etc/mysql/debian-start[11975]: OK
lines 1-21/21 (END)
```

* 启动MariaDB数据库服务
> sudo systemctl start mysql

* 设置mysql随系统服务启动
> sudo update-rc.d  mysql defaults

* 撤销随系统服务启动
> sudo update-rc.d  -f mysql remove

* 与之前版本mysql不同，需要获得操作系统管理员权限，才能登录MariaDB的root用户，普通操作系统用户不能登录MariaDB数据库root用户
> sudo mysql -u root -p
此时需要输入root的密码

* 备份mysql数据库 也需要获得操作系统管理员才能执行备份
> sudo mysqldump -uroot -p mysql >mysql.sql

*.创建普通数据库用户 （登录普通数据库用户则不需要获得操作系统管理员权限）
> create user 'lewjun'@'%' identified by '1';

* 登录远地数据库(需要lewjun@'%')
> mysql -h localhost -u lewjun -p1

* 修改MariaDB配置文件，监听外网访问
> sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf
```
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
# bind-address        = 127.0.0.1  #注释掉这一行
```
* 重启数据库服务，使配置生效
> sudo systemctl restart mysql

* MariaDB版本
> mysql -V


### mysql 完全卸载
* sudo apt purge mysql-*
* sudo rm -rf /etc/mysql/ /var/lib/mysql
* sudo apt autoremove
* sudo apt autoclean

### 创建数据库
使用root登录
> sudo mysql -u root -proot
创建库
> create database if not exists scott default character set utf8mb4 collate utf8mb4_unicode_ci;
查看已经创建的表
> show tables;

### 导入外部sql语句 source scott.sql
mysql> use scott;
mysql [scott]> source /home/lewjun/Documents/scott.sql;

### 忽略大小写
* 查看当前大小写情况
> show variables like '%case_table%';
``` 
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_table_names | 0     |
+------------------------+-------+
```
0表示不忽略大小写，1表示忽略大小写。
> sudo /etc/mysql/mariadb.conf.d/50-server.cnf
在[mysqlld]下添加
> lower_case_table_names=1
重启mysql
> sudo systemctl restart mysql
再次查看当前大小写情况
> show variables like '%case_table%';
``` 
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_table_names | 1     |
+------------------------+-------+
```
修改成功

### 解决mysql不用或任意密码也能登录
```
lewjun@ubuntu:~$ sudo mysql -u root -p
mysql> update user set authentication_string=password('root123456') where user='root';
mysql> update user set password=password('root123456') where user='root';
mysql> update user set plugin='mysql_native_password';
mysql> flush privileges;
mysql> quit;
重启 mysql
lewjun@ubuntu:~$ sudo systemctl mysql restart;
现在root使用任意密码登录，就会报错了。。。
lewjun@ubuntu:~$ sudo mysql -u root -panypwd
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
使用之前设置的密码root123456即可登录成功。
lewjun@ubuntu:~$ sudo mysql -u root -proot123456
Welcome to the MariaDB monitor.  Commands end with ; or \g.

```


## sdk
### 安装
```
* sudo curl -s https://get.sdkman.io | bash
* source "/home/lewjun/.sdkman/bin/sdkman-init.sh"
* sdk install gradle 4.10
```

## Apache Maven
```
* 创建j2se项目
> mvn archetype:generate -DgroupId=com.microandroid -DartifactId=hij2se -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false -X -DarchetypeCatalog=local

* 创建j2ee项目
> mvn archetype:generate -DgroupId=com.microandroid -DartifactId=hij2ee -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false -X -DarchetypeCatalog=local

```

## redis
```
$ sudo apt install make
$ sudo apt install gcc
$ sudo apt install tcl
$ wget http://download.redis.io/releases/redis-4.0.11.tar.gz
$ tar xzf redis-4.0.11.tar.gz
$ cd redis-4.0.11
$ sudo make
$ src/redis-server
# 另启一个窗口
$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"

将安装目录下的redis.conf
# requirepass foobared 取消注释，那么表示需要密码，密码默认为foobared
./redis-server ../redis.conf
./redis-cli 
auth foobared
./redis-cli --help
  -h <hostname>      Server hostname (default: 127.0.0.1).
  -p <port>          Server port (default: 6379).
  -s <socket>        Server socket (overrides hostname and port).
  -a <password>      Password to use when connecting to the server.
  -n <db>            Database number.

./redis-cli -h 127.0.0.1 -p 6379 -a foobared -n 1
```

## date
> date -s '2018-08-27 00:24:00'

## git
```
1. 安装
sudo apt install git
2. 配置
$ git config --global user.name 'lewjun072'
$ git config --global user.email 'lewjun072@sina.com'
```

## vim
在没有安装vim之前，直接使用vi编辑文件时候，输入i时，没有提示输入状态，并且还有其它问题
apt install vim

## 安装&配置jdk
```
1. 下载
到http://www.oracle.com官方网站下载合适的jdk
2. cd /usr/local/jdk
sudo cp /home/lewjun/Downloads/jdk-8u181-linux-x64.tar.gz ./
3. sudo tar -xzvf jdk-8u181-linux-x64.tar.gz 
4. sudo vim /etc/profile
export JAVA_HOME=/usr/local/jdk/jdk1.8.0_181
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
5. source /etc/profile
6. javac -version
```

## 压缩
```
tar –cvf jpg.tar *.jpg //将目录里所有jpg文件打包成tar.jpg 
tar –czf jpg.tar.gz *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz 
tar –cjf jpg.tar.bz2 *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2 
tar –cZf jpg.tar.Z *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z 
rar a jpg.rar *.jpg //rar格式的压缩，需要先下载rar for linux 
zip jpg.zip *.jpg //zip格式的压缩，需要先下载zip for linux
```

## 解压
```
tar –xvf file.tar //解压 tar包 
tar -xzvf file.tar.gz //解压tar.gz 
tar -xjvf file.tar.bz2 //解压 tar.bz2 
tar –xZvf file.tar.Z //解压tar.Z 
unrar e file.rar //解压rar 
unzip -o file.zip //解压zip -o 覆盖
```

## 安装sublime
```
1. wget https://download.sublimetext.com/sublime_text_3_build_3176_x64.tar.bz2
2. sudo cp Downloads/sublime_text_3_build_3176_x64.tar.bz2 /usr/local/
3. tar -xjf ./sublime_text_3_build_3176_x64.tar.bz2
4. cd sublime_text_3/
5. ./sublime_text
6. 安装 Package Control
> https://packagecontrol.io/installation
7. ctrl+shift+p > install package > prety json
8. remove package > prety json
```

## gpg: no valid OpenPGP data found.
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
gpg: no valid OpenPGP data found.
> curl -O https://download.sublimetext.com/sublimehq-pub.gpg

## curl
1. install
> sudo apt install curl

## apt install 过程出现失败，可以尝试如下命令
> sudo apt  --fix-broken install

## Ubuntu su认证失败解决方法
```
lewjun@ubuntu:~$ sudo passwd
Password: <--- 输入安装时那个用户的密码
Enter new UNIX password: <--- 新的 Root 用户密码
Retype new UNIX password: <--- 重复新的 Root 用户密码
passwd ：已成功更新密码
```

## 换国内源
编辑/etc/apt/sources.list，编辑之前备份，如果不能编辑，可以把这个文件内容拷贝到其它地方，添加如下数据后再覆盖。
```
# 中科大源
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# 阿里源
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
# 163
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
```
last 
> sudo apt-get update
> sudo apt-get upgrade

## Ubuntu18.04下安装搜狗输入法
[参考这里](https://blog.csdn.net/lupengCSDN/article/details/80279177)
```
1. sudo apt install fcitx
2. download sogoupinyin.deb
3. sudo dpkg -i sogoupinyin.deb
4. Settings > Region & Language > Manage Installed Languages > Keyboard input method system > fcitx
5. Reboot
6. 登陆后在右上角出现一个键盘标志，点击进入，选择Configure Current Input Method
7. 进入Input Method界面后，选择+号
8. 进入到Add input method界面，将下面的Only Show Current Language 点掉后，在搜索栏搜索搜狗拼音，选中之后进行添加。
9. 添加成功后，将搜狗拼音移到第一位。
10. 成功之后，打开浏览器随便输入，可以看到输入结果，同时成功后下方还会出现搜狗输入法的标志，这时候就可以通过shirt键切换中英文。
```
