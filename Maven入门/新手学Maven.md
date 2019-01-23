## 新手Maven 入门
<!-- toc -->

- [Maven 简介](#Maven-%E7%AE%80%E4%BB%8B)
- [一、下载安装环境配置](#%E4%B8%80%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE)
- [二、常用的Maven命令](#%E4%BA%8C%E5%B8%B8%E7%94%A8%E7%9A%84Maven%E5%91%BD%E4%BB%A4)
- [三、Maven 的仓库](#%E4%B8%89Maven-%E7%9A%84%E4%BB%93%E5%BA%93)
- [二、eclipse 上maven配置](#%E4%BA%8Ceclipse-%E4%B8%8Amaven%E9%85%8D%E7%BD%AE)
- [三、 eclipse 创建简单的Java maven 项目](#%E4%B8%89-eclipse-%E5%88%9B%E5%BB%BA%E7%AE%80%E5%8D%95%E7%9A%84Java-maven-%E9%A1%B9%E7%9B%AE)
- [四、eclipse 下maven 项目结构](#%E5%9B%9Beclipse-%E4%B8%8Bmaven-%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84)
- [五、pom.xml写依赖关系](#%E4%BA%94pomxml%E5%86%99%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB)

<!-- tocstop -->
### Maven 简介
Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具软件。 ---百度百科
>最简单的应用:不用到处找jar包,便于共享,不用分心找jar包,加油骚年!!!
### 一、下载安装环境配置
1.下载地址
```html
http://maven.apache.org/download.cgi
```
[Maven – Download Apache Maven](http://maven.apache.org/download.cgi)
![maven下载.png](attachments\04135da4.png)
2. 解压即可使用
3. 环境配置
在系统环境变量新建变量
`M2_HOME`
路径填maven 解压的位置
![mavaen环境配置1.png](attachments\d7ff65f4.png)
在path添加 `%M2_HOME%\bin`
![环境配置.png](attachments\b02fa9aa.png)
检查是否安装成功
`mvn -version`
![检查是否安装成功.png](attachments\73b78442.png)
### 二、常用的Maven命令
参见文章:
[Maven常用命令： - 艺意 - 博客园](http://www.cnblogs.com/wkrbky/p/6352188.html)

### 三、Maven 的仓库
1.中央仓库
Maven的国外中央仓库
[Maven Repository: Search/Browse/Explore](https://mvnrepository.com/)
2.镜像仓库
阿里云的镜像仓库
```
https://maven.aliyun.com/nexus/content/groups/public/
```
[阿里云Maven](https://maven.aliyun.com/nexus/content/groups/public/)
3.更改Maven中央仓库
在 apache-maven-3.6.0\conf下修改settings.xml
添加
```xml
<mirror>      
  <id>nexus-aliyun</id>    
  <name>nexus-aliyun</name>  
  <url>http://maven.aliyun.com/nexus/content/groups/public</url>    
  <mirrorOf>central</mirrorOf>      
</mirror> 
```
![maven设置镜像仓库.png](attachments\26e1e8e8.png)
4.更改本地仓库
```xml
<localRepository>D:/apache-maven-3.6.0-bin/repository</localRepository>
```
![Maven设置本地仓库.png](attachments\2e6c4535.png)


### 二、eclipse 上maven配置
一般都安装了maven插件
1. 查看maven插件
在window->preferences->搜索框输入maven
![maven的eclipse.png](attachments\8f2d05fe.png)
如果没有的话,我建议你更新一下eclipse版本吧,现在都是eclipse2018了
2. eclipse 配置maven
1.在window->preferences->搜索框输入maven,点installations
![eclipse中maven插件配置.png](attachments\2cfaec7b.png)
user Setting 设置全局设置gloabl Setting输入apache-maven-3.6.0\conf\settings.xml的路径
![eclipse中maven插件配置2.png](attachments\c704866d.png)
### 三、 eclipse 创建简单的Java maven 项目
1.new ->other->输入maven->maven project
![创建maven项目1.png](attachments\ad6dff4c.png)
2.next
![创建maven项目2.png](attachments\34200957.png)
3.next
 ![创建maven项目3.png](attachments\9f27ebd0.png)
 4.finish
 ### 四、eclipse 下maven 项目结构
 ![maven项目框架.png](attachments\3913be5e.png)
 - src/main/java 存放java代码
 - src/main/resourc 存放配置文件,比较规范
 - src/test/java 存放测试的java代码
 - src/test/resource 存放测试的配置文
 - JRE System Libary 存放jre的jar包
 - src 不需处理
 - target 存放编译后的文件
 - pom.xml 最重要的东西,项目依赖文件写在这里
 
 ### 五、pom.xml写依赖关系
 1.查看所需的jar包的坐标
maven 仓库
```html
https://mvnrepository.com/
```
https://mvnrepository.com/
![maven查找jar包坐标.png](attachments\d960a364.png)
点开,复制depency代码
![maven查找jar包坐标2.png](attachments\973a8feb.png)
2. 添加到pom.xml
 ![添加依赖pom.png](attachments\0418629a.png)
 3. 查看maven下载的jar包
 在Maven Dependencies依赖文件
 ![maven下载的依赖文件.png](attachments\1b463d05.png)
 jar 包下载在我们设置的仓库文件中
 >maven 真香,特别是找一些不常用的jar包
 >真的超级麻烦,用了Maven,再也不用担心找jar包了

----
 > 题外话,Junit功能还是很强大啊