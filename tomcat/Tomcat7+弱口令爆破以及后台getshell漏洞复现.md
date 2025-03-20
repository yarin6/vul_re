# Tomcat7+弱口令爆破以及后台getshell漏洞复现



1. ***弱口令&&密码爆破***

Tomcat 6.0及以后版本中，如果用户连续登录失败次数达到了5次，默认情况下会锁定该用户的登录账
户30分钟。在锁定期间，即使用户输入正确的用户名和密码，也无法登录管理页面（这里为了演示，
我手动取消了其对于爆破的限制）



具体取消限制方法：

> docker cp 352c637fc7a9:/usr/local/tomcat/conf/server.xml .    //将配置文件从容器复制一份出来，原因是容器内没有vim等命令，需要在外修改后传进去

![image-20250314194236505](C:\Users\21415\AppData\Roaming\Typora\typora-user-images\image-20250314194236505.png)

> docker cp server.xml 352c637fc7a9:/usr/local/tomcat/conf/

 注意重启容器



此时就可以尝试bp抓包爆破了

![image-20250314195209088](C:\Users\21415\AppData\Roaming\Typora\typora-user-images\image-20250314195209088.png)

爆破时注意base64编码，而且还要符合tomcat:tomcat该形式。成功爆破

![image-20250314201203286](C:\Users\21415\AppData\Roaming\Typora\typora-user-images\image-20250314201203286.png)

![image-20250314201243107](C:\Users\21415\AppData\Roaming\Typora\typora-user-images\image-20250314201243107.png)



2. ***后台getshell***

进入后台，上传war包，直接利用冰蝎中的木马shell.jsp压缩成war，多了一行shell

![image-20250314202140239](C:\Users\21415\AppData\Roaming\Typora\typora-user-images\image-20250314202140239.png)

访问ip:port/shell/shell.jsp， 用冰蝎链接，成功getshell



![image-20250314203042819](C:\Users\21415\AppData\Roaming\Typora\typora-user-images\image-20250314203042819.png)





**参考链接https://github.com/vulhub/vulhub/blob/master/tomcat/tomcat8/README.zh-cn.md**



