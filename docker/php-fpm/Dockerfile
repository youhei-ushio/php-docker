FROM php:fpm

ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update
RUN apt-get -y install libzip-dev unzip zlib1g-dev git libpng-dev libgif-dev \
    libjpeg-dev libfreetype6-dev libssl-dev iputils-ping net-tools
RUN docker-php-ext-install pdo_mysql mysqli zip bcmath opcache \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd
RUN printf "\n" | pecl install xdebug

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer
RUN composer config -g repos.packagist composer https://packagist.jp
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

ENV PHP_IDE_CONFIG "serverName=docker"

ENV PATH $PATH:/var/app/vendor/bin
