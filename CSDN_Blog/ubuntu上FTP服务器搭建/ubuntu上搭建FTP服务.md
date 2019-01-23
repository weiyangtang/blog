## Ubuntu上搭建FTP服务器,文件上传下载，530 login incorrect，550 Permission denied.脱坑
### 1.下载安装ftp
```shell
sudo apt-get update  #更新下载源
sudo apt-get install vsftpd   #安装ftp
```
### 2.设置ftp文件夹、用户名、密码
```shell
sudo useradd -d /home/ftp -M  userName  #用户名为userName,改成你的
sudo passwd  userName  #用户名和你上面输入的一致,后面会有两次密码输入确认
```
![FTP服务器搭建_添加用户.png](attachments\c9c175d1.png)
#### 入坑：530 Login incorrect. Login failed
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
![命令行登录.png](attachments\e907f88c.png)
##### 3.2浏览器下载文件
![浏览器下载文件.png](attachments\a9eb651c.png)
##### 3.3 window 资源管理器(window用户推荐使用)
win+E ,打开资源管理器,输入ftp服务器的ip地址`ftp://129.**.19.199/` 
![资源管理器下载.png](attachments\1cd88f1e.png)
操作:复制粘贴,可能无法上传文件到服务器
### 4.文件上传ftp服务器, 550 错误 Permission denied
文件上传无权限
响应:	550 Permission denied.
错误:	严重文件传输错误
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
 sudo chmod 777  /home/ftp    #/home/ftp表示ftp服务器的文件存放路径
```
![FTP服务器搭建_设置文件夹权限.png](attachments\458ebc3f.png)
第二步:将fileZilla的传输协议改成SFTP(Secure File Transfer Protocol)
![FTP服务器搭建_设置协议.png](attachments\a81ffce1.png)


