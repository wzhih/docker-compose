FROM php:7.3-fpm

LABEL maintainer="winton wang"
LABEL email="wiwang@foxmail.com"

#换国内镜像源
#COPY sources.list /etc/apt/sources.list

# 复制本地配置文件
COPY php.ini /usr/local/etc/php/php.ini

#安装extension
RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list \
    && sed -i 's|security.debian.org/debian-security|mirrors.ustc.edu.cn/debian-security|g' /etc/apt/sources.list \
    && apt-get update && apt-get install -y \
        #安装gd扩展依赖
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        #安装intl扩展依赖
        libicu-dev \
        #安装imagick扩展依赖
        imagemagick \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install -j$(nproc) opcache \  
    && docker-php-ext-install -j$(nproc) mysqli \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) pcntl \
    && docker-php-ext-install -j$(nproc) bcmath

#安装xdebug redis
#xdebug配置 phpstorm要配置项目路径映射，在File | Settings | Languages & Frameworks | PHP | Debug 和 Servers里面配置
RUN pecl channel-update pecl.php.net \
    && pecl install xdebug-2.9.5 redis-5.2.2 \
    && docker-php-ext-enable xdebug redis \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_port=10000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.idekey=phpstorm" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

#安装composer
# RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
#     && php composer-setup.php \
#     && php -r "unlink('composer-setup.php');" \
#     && mv composer.phar /usr/local/bin/composer \
#     && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/



CMD ["php-fpm"]