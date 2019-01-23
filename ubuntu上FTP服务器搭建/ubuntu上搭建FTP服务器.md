## Ubuntu 18.04上搭建FTP服务器,建私人网盘,下载安装,文件上传下载，530 login incorrect，550 Permission denied.脱坑
## 一、FTP服务器的下载安装、用户设置、环境配置、使用说明
### 1.下载安装ftp
```shell
sudo apt-get update  #更新下载源
sudo apt-get install vsftpd   #安装ftp
```
### 2.设置ftp文件夹、添加用户名、密码
```shell
sudo useradd -d /home/ftp -M  userName  #用户名为userName,改成你的
sudo passwd  userName  #用户名和你上面输入的一致,后面会有两次密码输入确认
```
![FTP服务器搭建_添加用户.png](attachments\c9c175d1.png)
**小心坑** /home/ftp 是该用户ftp服务器的路径,所以要创建该路径,开放该文件夹的权限
```shell
sudo mkdir /home/ftp #创建文件夹
sudo chmod 777  /home/ftp #设置该文件夹权限为777,详细见下文
```
![FTP服务器搭建_设置文件夹权限.png](attachments\d45a6582.png)
### 3.修改vsftpd.conf配置文件
#### 3.1`/etc/vsftpd/vsftpd.conf`的简单介绍
`/etc/vsftpd/vsftpd.conf`是我们现在所用的vsftpd服务器的配置文件,这个非常重要,它可以决定是否允许匿名访问（默认是不允许匿名访问）、是否允许文件下载上传修改（默认是不允许上传文件和修改文件），是否允许本地登录（是指服务器本地登录）
#### 3.2修改/etc/vsftpd/vsftpd.conf配置文件
**打开vsftpd.conf 文件**
```shell
sudo vi /etc/vsftpd.conf 
```
![修改配置文件.png](attachments\97044ce1.png)
![修改配置文件2.png](attachments\e49b5fb0.png)
**/etc/vsftpd/vsftpd.conf配置文件的内容**
``` C
listen=NO
listen_ipv6=YES
anonymous_enable=NO  #是否允许匿名访问
local_enable=YES  #是否允许服务器本地登录
# write_enable=YES   #是否允许对ftp文件上传和修改,默认是被注释掉,如果你需要用户上传文件,就将#去掉即可,见下文
#local_umask=022
#anon_upload_enable=YES   #是否允许匿名用户上传文件,创建文件夹,默认被注释掉
#anon_mkdir_write_enable=YES  #是否允许匿名创建目录,默认是被注释掉
dirmessage_enable=YES  #目录信息
use_localtime=YES  #文件列表的上传时间
xferlog_enable=YES  #上传下载的日志
connect_from_port_20=YES  #ftp连接的端口,不要改
#chown_uploads=YES  #切换文件上传的目录,小心,这个操作可以会被用户误操作,建议别改
#chown_username=whoever
#xferlog_file=/var/log/vsftpd.log  #默认的上传下载文件的日志存放路径,不用改,要查看日志见本文最后面
#xferlog_std_format=YES #日志格式
#idle_session_timeout=600 #会话的超时时间,默认10分钟
#data_connection_timeout=120  #设定单次最大的连续传输时间，这里使用默认
#nopriv_user=ftpsecure
#设定支撑vsftpd 服务的宿主用户为手动建立的vsftpd用户。
#async_abor_enable=YES 
#设定支持异步传输功能

#ascii_upload_enable=YES
#ascii_download_enable=YES
# 设置ACII码文件上下传输

pam_service_name=ftp
#重点:pam_service_name=ftp,否则可能会报530错误,登录失败
```
**修改配置文件**
write_enable=YES  YES表示用户有权可以对ftp服务器进行文件上传和修改
将write_enable=YES前面的#删除掉
![修改配置文件3.png](attachments\d6159da1.png)

### 4.登录测试
window平台上:win+R,输入cmd,在dos窗口上输入
```shell
ftp : ip地址
```
![windows命令行登录.png](attachments\5e029215.png)
### 5.文件上传和下载
**温馨提示：如果在文件上传下载过程中遇到问题见下文：脱坑记**
#### 5.1DOS命令行
常用的命令
1. open：与服务器相连接
2. send(put)：上传文件
3. get：下载文件
4. mget：下载多个文件
5. cd：切换目录 
6. dir：查看当前目录下的文件
7. del：删除文件
8. bye：中断与服务器的连接
举个栗子
```shell
ftp :129.211.19.198 #登录
dir   #查看文件列表
cd  pic 
mget  *.jpg  #下载pic文件下的所有.jp格式的图片
```
不太好用,特别是普通的用户,简直就是痛苦,用户体验感较差
#### 5.2 window资源管理器下载上传文件(强力推荐)
![window资源管理器两种方式.png](attachments\5c832b08.png)
>**下载**:此文件夹下的文件复制粘贴到本地
**上传**:将本地的文件粘贴到此文件夹下
**优点**:用户体验感较好,可以直接下载文件夹,不用压缩文件,对比QQ传输文件,非常方便,不用安装任何插件
**缺点**:下载较大的文件时可能会有无响应,没有进度条,
**适宜人群**:这个FTP服务器比较适合当代大学生,偶尔要交个报告啊,平时要打印东西啊,不用带U盘,可以说是大学生的神器
#### 5.3 浏览器下载
![浏览器下载文件.png](attachments\b4a12e26.png)
>**下载**:右键另存为即可
>**上传**:好像不可以
>**优点**:可以看到进度条
>**缺点**:无法下载文件夹,无法上传文件,操作稍微有些麻烦
>**适宜人群**:whoerver
#### 5.4 FTP专用工具:如FileZilla(功能十分强大)
![FileZilla使用介绍.png](attachments\837a1885.png)
>**下载**:右键有下载选择
>**上传**:好像在本地文件右键选择上传
>**优点**:功能十分强大,还可以直接传文件到云服务器,可视化好
>**缺点**:要下载这个软件,还得花两三分钟习惯一下
>**适宜人群**:开发人员、有一定电脑基础的人员
>
### 6.查看ftp日志(文件上传、下载、用户登录)

```shell
cd /var/log  #ubuntu系统日志路径
ls  #查看所有日志文件
sudo vim /var/log/ vsftpd.log   
#编辑查看vsftpd.log文件

```
## 二、530 Login incorrect. Login failed和550 Permission denied 错误解决方案
### 530 Login incorrect. Login failed
![FTP服务器搭建_503错误.png](attachments\70953ec9.png)
解决方案：
1.请务必检查你的ftp 地址、用户名、密码是否正确
极有可能是你密码设置没成功，或者是ip地址写错了,忘记密码可以重新设置`sudo passwd userName`,见上文
2.ubuntu专用的方法:重装vsftpd,然后重启ftp
```shell
sudo apt-get remove vsftpd
sudo rm /etc/pam.d/vsftpd
sudo apt-get install vsftpd 
sudo service vsftpd restart  #重启ftp服务器测试
```
3.注释掉/etc/pam.d/vsftpd中一行
```shell
 sudo vim  /etc/pam.d/vsftpd # vim编辑vsftpd
```
将`auth   required        pam_shells.so`注释掉
![修改vsftpd.png](attachments\0d7e2e3d.png)
还是要重启ftp
```shell
sudo service vsftpd restart  #重启ftp服务器测试
```
祝你早日脱坑,哈哈
![ubuntu上登录ftp服务器.png](attachments\702f989f.png)
在ubuntu上登录ftp
##  3. 登录文件下载
##### 3.1命令行登录测试
```shell
ftp 129.**.19.199/  
#输入你ftp服务器的ip地址
```

### 550 错误 Permission denied
![资源管理器无法上传文件.png](attachments\f83df93c.png)
文件上传无权限
响应:	550 Permission denied.
错误:	严重文件传输错误
修改文件权限参考文章:
[Ubuntu修改文件权限 - slwhy的博客 - CSDN博客](https://blog.csdn.net/slwhy/article/details/78876237)
```
https://blog.csdn.net/slwhy/article/details/78876237
```
设置文件权限777
- #### 用FileZilla客户端上传文件550 Permission denied,汉字乱码
![FTP文件上传550 Permission denied.png](attachments\a7335727.png)
![FTP服务器搭建_505错误解决方案.png](attachments\b9a30628.png)
解决方案
第一步: 设置ftp服务器的权限为777(每个用户都有权限读写修改)
```shell
 sudo chmod 777  /home/ftp  
 # /home/ftp表示ftp服务器的文件存放路径
```
![FTP服务器搭建_设置文件夹权限.png](attachments\458ebc3f.png)
第二步:将fileZilla的传输协议改成SFTP(Secure File Transfer Protocol)
![FTP服务器搭建_设置协议.png](attachments\a81ffce1.png)
