FROM php:7.3-fpm

LABEL maintainer="winton wang"
LABEL email="wiwang@foxmail.com"

#换国内镜像源
COPY sources.list /etc/apt/sources.list

# 使用默认配置文件,开发是php.ini-development，生成环境是php.ini-production
RUN cp "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

#安装extension
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) mysqli \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) bcmath

#安装xdebug mcrypt
#xdebug配置 phpstorm要配置项目路径映射，在File | Settings | Languages & Frameworks | PHP | Debug 和 Servers里面配置
RUN pecl install xdebug-2.7.2 \
    && docker-php-ext-enable xdebug \
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

#安装一些会用到的工具
RUN apt-get install -y \
        vim \
        less


CMD ["php-fpm"]