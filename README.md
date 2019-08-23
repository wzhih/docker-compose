# docker-composer
**一个用于自用的，搭建php运行环境的docker-compose文件及目录结构**
  

### 每个分支对应一种运行环境与目录结构，如master分支就是以以下软件组成

- **Nginx 1.16**

- **PHP 7.3-fpm**

- **Mysql 8.0**


### 使用例子

第一次使用，在docker-compose.yml文件同级目录下执行以下命令

>docker-compose up -d

把项目放到www目录就可以访问此项目了，像安装了个LAMP一样

之后，关闭容器，使用

> docker-compose stop

开启容器，使用

> docker-compose start

关闭并删除这几个docker容器，使用

> docker-compose down

docker-compose的其他命令请查看其官方文档

***

### mysql8.0连接失败(1045错误)

MySQL8.0开始使用caching_sha2_passoword加密方式
使用以下命令，修改加密方式为mysql_native_password

```sql
use mysql;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
```

*其实大部分的配置文件以及容器产生的数据，我都不想放到容器外面来，可是由于修改配置文件太麻烦了，所以，我把可能会经常修改的软件的配置文件如nginx的，"提取"出来。至于数据库数据，还是放在外面吧，万一哪一天容器出问题了。。。。。。*