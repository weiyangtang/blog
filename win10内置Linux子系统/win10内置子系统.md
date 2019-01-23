## win10内置Linux 系统用户体验,和下载安装使用指南,window和Linux文件传输和远程登录wsl
>新手用完,感觉挺不错的。感觉和SSH连接远程服务器差不多
>最重要的下载安装简便,推荐不方便安装Linux系统的同学尝试一下
### 一、下载安装
1. 在设置->更新与安全->开发者选项->勾选开发人员模式
![开启win10子系统.png](attachments\51280b2e.png)
2. 在微软商店输入`wsl`  (window subsystem)
![下载安装win10内置子系统.png](attachments\1064bef8.png)
2.在下面的Linux系统中选择一款你喜欢的
3.![开启win10子系统5.png](attachments\dc1d9499.png)
4.设置用户名和密码,Linux输入密码是没有*代替,直接是空,网友们建议密码要复杂一点避免后期安装MySql软件密码强度不够等麻烦
![开启win10子系统6.png](attachments\fcd87523.png)
5.安装成功: 那就试试在Linux上安装软件是多么爽的一件事
```shell
sudo apt-get install cmatrix  #安装cmatrix
cmatrix                                #运行cmatrix           
```
![cmatrix 命令黑客界面.png](attachments\4885f2fc.png)
是不是很酷啊
更多好玩的Linux命令参考
```
https://www.cnblogs.com/1394htw/p/6358737.html?utm_source=itdadao&utm_medium=referral
```
[10个非常有趣的Linux命令【转载】 - 十点 - 博客园](https://www.cnblogs.com/1394htw/p/6358737.html?utm_source=itdadao&utm_medium=referral)
### 二、如何在window上打开win10内置子系统
#### 方法一: 
1. 以管理员身份打开命令提示符或者powershell
2. 输入`bash`,即可登录
适用于:本机登录
#### SSH登录
参考文章:
```
https://www.jianshu.com/p/9df97c22efc9
```
[Xshell完美连接win10 Linux子系统 - 简书](https://www.jianshu.com/p/9df97c22efc9)
1.在win10电脑下载xshell软件,或者是Putty
2.先用方法一登录内置子系统
3. SSH配置
输入
```shell
sudo apt-get remove --purge openssh-server   ## 先删ssh
sudo apt-get install openssh-server          ## 在安装ssh  
sudo rm /etc/ssh/ssh_config                  ## 删配置文件，让ssh服务自己想办法链接
sudo service ssh --full-restart
```
4. 查看ip
输入命令
```shell
ifconfig
```
![查看ip地址.png](attachments\dd471572.png)
5.putty登录或者xshell
![putty登录wsl.png](attachments\cc6d82ce.png)
![putty登录wsl2.png](attachments\10218668.png)
登录成功了
#### window和Linux文件传输
工具:FileZilla
1.下载FileZilla
2. 在FileZilla输入上面查看的ip,或者直接输入127.0.0.1(本机回环地址),账号密码
![ftp文件传输.png](attachments\5461ad27.png)