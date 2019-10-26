# docker-composer
**一个用于自用的，搭建php运行环境的docker-compose文件及目录结构**

## 容器如何访问主机

- window/mac下，使用docker for window/mac，可以在容器中直接使用`host.docker.internal`，通过这个host即可访问主机
- linux下，暂时还没有`host.docker.internal`，但是通过`docker-compose.yml`创建的容器之间，可以通过容器名称进行连接。
就像此项目中，nginx的默认配置文件`nginx\conf.d\server.conf`中就是通过`php:9000`来访问php容器的。同理，php代码在php容器解析运行时，可以通过`mysql`来访问mysql容器。

**容器名称可自定义，由docker-composer.yml文件中的`container_name`定义**

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

或者，直接在配置文件里面加入以下内容，容器初始化的时候，就会使用老的加密方式了
```
default-authentication-plugin=mysql_native_password
```

*其实大部分的配置文件以及容器产生的数据，我都不想放到容器外面来，可是由于修改配置文件太麻烦了，所以，我把可能会经常修改的软件的配置文件如nginx的，"提取"出来。至于数据库数据，还是放在外面吧，万一哪一天容器出问题了。。。。。。*