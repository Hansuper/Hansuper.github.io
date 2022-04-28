## tiny tiny rss 服务搭建<!-- {docsify-ignore} -->

** 在搭建 tiny tiny rss服务时 遇到的问题在此记录下

按照官方文档 ```https://git.tt-rss.org/fox/ttrss-docker-compose.git/tree/README.md?h=static-dockerhub``` 采用docker-compose方式搭建 发现nginx报错 iptables failed

在搜索很多文章感觉都不对，最终发现一段这个描述
```
docker 服务启动的时候，docker服务会向iptables注册一个链，以便让docker服务管理的containner所暴露的端口之间进行通信

通过命令iptables -L可以查看iptables 链



在开发环境中，如果你删除了iptables中的docker链，或者iptables的规则被丢失了（例如重启firewalld），docker就会报iptables error例如：failed programming external connectivity … iptables: No chain/target/match by that name

要解决这个问题，只要重启docker服务，之后，正确的iptables规则就会被创建出来
```

真的是没有重启解决不了的 ```service docker restart ``` 完美解决

服务起来之后,发现用默认密码password无法登录，提示用户或密码不正确，这就出了鬼。在程序员的钻研精神驱使下必须要解决该问题

解决思路:
1,进入到rss web服务器 看到代码找线索
```docker exec -it ttrss-docker_web-nginx_1 /bin/sh```

通过docker-compose.yml文件 我们知道代码是在/var/www/html/tt-rss目录下; 进入到该目录果然是项目的代码，通过经验进入到sql/pgsql目录 打开 schema.sql 看到 是往tress_users表中添加了默认的admin用户

```
insert into ttrss_users (login,pwd_hash,access_level) values ('admin','SHA1:5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8', 10);
```

2,进入到rss pgsql服务
```docker exec -it ttrss-docker_db_1 /bin/sh```

pgsql是以用户身份进入数据库

```
sudo -i -u postgres //切换为postgres用户 同时相当于登录到postgres数据库

\l //查看都有哪些数据库 相当于mysql的 show databases

\c 库名 //进入到选择的数据库 相当于mysql的 use database

\di //查看都有哪些表 相当于mysql show tables

select * from table; //查看数据表中的内容

delete from ttrss_users where id = 1; //删除表中指定的记录

insert into ttrss_users (login,pwd_hash,access_level) values ('admin','SHA1:5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8', 10); //插入默认的用户
```

我们通过schema.sql文件中发现用户是在 tress_user表中

密码字段是经过加密的我们没办法恢复，索性我们删除掉原始记录重新执行一下insert语句

至此已可以成功登录。