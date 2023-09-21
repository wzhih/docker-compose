FROM phpswoole/swoole:4.5.11-php7.4

# 复制本地配置文件
COPY php.ini /usr/local/etc/php/php.ini

RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list \
    && sed -i 's|security.debian.org/debian-security|mirrors.ustc.edu.cn/debian-security|g' /etc/apt/sources.list \
    && apt-get update \
    #安装yasd 用于debug
    && apt-get install -y git libboost-all-dev \
    && cd /tmp \
    && git clone https://gitee.com/sgyh/yasd.git \
    && cd yasd \
    && phpize --clean && \
        phpize && \
        ./configure && \
        make clean && \
        make && \
        make install \
    # 配置yasd
    && echo "zend_extension=yasd" >> /usr/local/etc/php/conf.d/docker-php-ext-yasd.ini \
    && echo "yasd.debug_mode=remote" >> /usr/local/etc/php/conf.d/docker-php-ext-yasd.ini \
    && echo "yasd.remote_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-yasd.ini \
    && echo "yasd.remote_port=9004" >> /usr/local/etc/php/conf.d/docker-php-ext-yasd.ini \
    && echo "swoole.use_shortname = off" >> /usr/local/etc/php/conf.d/docker-php-ext-swoole.ini \
    #安装扩展安装脚本，然后用工具安装composer
    && curl -sSLf \
        -o /usr/local/bin/install-php-extensions \
        https://gitee.com/sgyh/docker-php-extension-installer/raw/master/install-php-extensions \
    && chmod +x /usr/local/bin/install-php-extensions \
    && install-php-extensions mysqli pdo_mysql pcntl redis protobuf \
    && composer selfupdate

WORKDIR /workspace

EXPOSE 9501

CMD []
