# 致远OA-ajax.do任意文件上传漏洞

<br/><br/>

渗透测试网站前必须签订渗透测试授权书，若私自渗透网站，后果自负

具体处罚见《中华人民共和国网络安全法》

本文经对某公司签过渗透测试授权书下进行。

<br/><br/>

**致远OA-ajax.do任意文件上传漏洞复现:**

<br/><br/>

**漏洞详情：**

<br/><br/>

致远OA ajax.do登录绕过 任意文件上传

<br/><br/>

漏洞影响范围:

致远OA V8.0;

致远OA V7.1、V7.1SP1;

致远OA V7.0、V7.0SP1、V7.0SP2、V7.0SP3;

致远OA V6.0、V6.1SP1、V6.1SP2;

致远OA V5.x;

致远OA G6

<br/><br/>

**漏洞复现：**

<br/><br/>

1.致远OA V8 前台

<img src="https://github.com/rmrfstop/rmrfstop.github.io/blob/%E8%87%B4%E8%BF%9COA/1.png">

<br/><br/>

2.可能存在漏洞路径：

```
http://ip:port/seeyon/thirdpartyController.do.css/..;/ajax.do
```

<img src="https://github.com/rmrfstop/rmrfstop.github.io/blob/%E8%87%B4%E8%BF%9COA/2.png">

<br/><br/>

3.使用poc脚本进行测试，验证漏洞是否存在：

poc脚本如下：

<img src="https://github.com/rmrfstop/rmrfstop.github.io/blob/%E8%87%B4%E8%BF%9COA/3.png">

<br/><br/>

4.返回响应200，成功

<img src="https://github.com/rmrfstop/rmrfstop.github.io/blob/%E8%87%B4%E8%BF%9COA/4.png">

<br/><br/>

5.出现以下异常，则可能存在漏洞。

<img src="https://github.com/rmrfstop/rmrfstop.github.io/blob/%E8%87%B4%E8%BF%9COA/5.png">

<br/><br/>

6.冰蝎连接shell

路径： http://ip:port/seeyon/SeeyonUpdate1.jspx

密码： rebeyond

成功拿到管理员权限：

<img src="https://github.com/rmrfstop/rmrfstop.github.io/blob/%E8%87%B4%E8%BF%9COA/6.png">
