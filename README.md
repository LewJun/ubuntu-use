# ubuntu-use
ubuntu 18.04.1

[TOC]

## Apache Maven
```
创建j2se项目
mvn archetype:generate -DgroupId=com.microandroid -DartifactId=hij2se -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false -X -DarchetypeCatalog=local

创建j2ee项目
mvn archetype:generate -DgroupId=com.microandroid -DartifactId=hij2ee -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false -X -DarchetypeCatalog=local

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
