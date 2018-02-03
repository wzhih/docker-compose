ARG version=7.1-fpm

FROM php:${version}

MAINTAINER winton wang < 1309603684@qq.com >

COPY sources.list /etc/apt/sources.list
COPY crontab /usr/cron/crontab

#安装一些可能会用到的工具
RUN apt-get install -y \
        vim \
        cron \
    && service cron start \
    && crontab /usr/cron/crontab

CMD [ "executable" ]