## azkaban工作流调度器

### 简介 
* 基本了解：使用job配置文件建立任务之间依赖关系，提供web用户界面并且跟踪工作流
* 架构：Web Server;Executor Server;Mysql
### 配置 
* 三个压缩包：
  * azkaban-sql-script-2.5.0.tar.gz   存放了azkaban运行需要的sql（sql导入完文件就没用了）
  * azkaban-executor-server-2.5.0.tar.gz 存放执行器
  * azkaban-web-server-2.5.0.tar.gz 存放了web服务器

* 步骤：
* 1.解压: tar -zxf 
   * azkaban-2.5.0
   * azkaban-executor-2.5.0
   * azkaban-web-2.5.0
* 2.将三个文件放到一个文件夹下

 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-393430c49e1f3a09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 3.mysql下执行语句：
   * 3.1 `source /home/hadoop/azkaban/azkaban-2.5.0/create-all-sql-2.5.0.sql`(数据库为azkaban)

 ![1.png](https://upload-images.jianshu.io/upload_images/14465950-147874b28ef63e80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  
   * 3.2 数据库里生成表：
  
 ![2.png](https://upload-images.jianshu.io/upload_images/14465950-e858e16797a25f42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 4.ssl 配置:
`keytool -keystore keystore -alias jetty -genkey -keyalg RSA`
  * ssl的秘钥=jerry的密码（123456）
 
 ![1.png](https://upload-images.jianshu.io/upload_images/14465950-0f326339302f4333.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 5.配置 web下的文件:/azkaban-web-2.5.0/conf/
   * 5.1 
`vim azkaban.properties`
   * 时区：Asia/Shanghai
   * mysql:root-123-azkaban
   * jerry:123456
 
 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-a98c1a0c59bae5c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  * 5.2 vim azkaban-users.xml
`<user username="admin" password="admin" roles="admin,metrics" />`
 
 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-b868f85d39739c9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  
* 6.配置executor:azkaban-executor-2.5.0/conf
 * 6.1
`vim azkaban.properties`
   * 配置内容同5.1
 
 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-bd6a068b5b03814f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 7.启动服务（start/shutdown）
 * 注意：运行路径问题
      * azkaban/azkaban-executor-2.5.0路径下：
` bin/azkaban-executor-start.sh`
 
 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-a4819c13c27b1f52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  
 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-8c72e32fceda9bc8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  
 * azkaban/azkaban-web-2.5.0路径下：
` bin/azkaban-web-start.sh`
  
 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-104790922d686c78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  
 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-fdc4f5d7e81bc87b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


* 8.登录

 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-98edbbf1ab966998.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### azkaban页面操作
* 创建工程

 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-555361e2bd7892d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


* 上传job文件（如果原工程已经存在文件，会被替换掉）

 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-83b726b58c74d3ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


* 执行

 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-d3fc572548892098.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-7fcf0e33204d3032.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)+

 * 绿色代表成功，蓝色是运行，红色是失败。可以查看job运行时间，依赖和日志
 
![image.png](https://upload-images.jianshu.io/upload_images/14466577-e79a740c9d25d8d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


 * 点击details可以查看各个job运行情况（可以显示出文件错误信息）
 
![image.png](https://upload-images.jianshu.io/upload_images/14466577-fafec7f716c20b43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 使用案例（文件拓展名必须为.job,压缩包拓展名为.zip）
#### command命令
* 单个job
  * command.job
#### type=command
#### command=echo xxoo
  * 压缩
 
 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-1d4d49ea28a6519e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  * 执行结果

![image.png](https://upload-images.jianshu.io/upload_images/14466577-c9507b9cb5e885d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 多个job(dependcies=job的名称,来指定我当前要执行的job依赖于哪个job，在依赖的这个job执行完后再执行我)
  * foo.job
#### type=command
#### command=echo xxoo
  * bar.job 
#### type=command
#### dependencies=foo
#### command=echo bar 
 
  * 两个文件压缩成一个文件

  * 执行结果
 
  ![image.png](https://upload-images.jianshu.io/upload_images/14466577-aeb2957deef9339f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
  ![image.png](https://upload-images.jianshu.io/upload_images/14466577-c40fb1bcac305f45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 #### hdfs命令
 * hdfs
   * mk.job(azkaban执行有自己的默认路径，为了避免错误，写自己的路径或者绝对路径)
#### type=command
#### command=hadoop fs -mkdir /azkaban
   * 压缩
   * 执行结果
  
   ![image.png](https://upload-images.jianshu.io/upload_images/14466577-e039ada8fc585de7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
 #### linux命令
 
 #### hive命令行
 * hive -e 'mysql语句' 执行单条命令
 * hive -f '文件名' 执行文件（包含多个命令）
   * hivef.job
#### type=command
#### command=hive -f 'test.sql'
   * test.sql
#### use dsm;
#### create table aztest(id int,name string) row format delimited fields terminated by ',';
   * 两个文件一起压缩上传
   * 执行
 #### mapreduce:jar
 * wordcount(也可以自己写代码，打jar包)
#### type=command#### command=hadoop jar hadoop-mapreduce-examples-2.6.1.jar wordcount /akinput /akoutput
 
 * 命令中用到的要执行的jar包，在上传job时一并压缩打入
 
 ![image.png](https://upload-images.jianshu.io/upload_images/14466577-7c8a0f4facf283c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
 
 


  
  
 
 
 
 
 




 

 

 

 



 
 
