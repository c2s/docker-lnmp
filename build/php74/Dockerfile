FROM php:7.4.12-fpm-buster
MAINTAINER mofei <root@mofei.org>

# ARGS
ARG CHANGE_SOURCE=false

# Change Timezone
ARG TIME_ZONE=Asia/Shanghai
ENV TIME_ZONE=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone

# Copy Extension Packages

#COPY redis-3.1.3.tgz  /tmp/redis.tgz
#COPY mcrypt-1.0.0.tgz /tmp/mcrypt.tgz
#COPY swoole-2.1.3.tgz /tmp/swoole.tar.gz

# Change China Sources
#COPY sources.list /etc/apt/china.sources.list
#RUN if [ ${CHANGE_SOURCE} = true ]; then \
#	mv /etc/apt/sources.list /etc/apt/source.list.bak && mv /etc/apt/china.sources.list /etc/apt/sources.list \
#;fi

RUN apt-get -o Acquire::Check-Valid-Until=false update && apt-get -y upgrade

# Install Base Components
RUN apt-get install -y --no-install-recommends vim \
 wget \
 lrzsz \
 curl \
 telnet \
 iputils-ping \
 net-tools \
 tcpdump \
 strace \
 smem \
 gdb \
 lsof \
 libonig-dev \
 libjpeg-dev \
 libpng-dev \
 zlib1g-dev \
 libzip-dev \
 libfreetype6-dev \
 libjpeg62-turbo-dev \
 libmcrypt-dev \
 libsasl2-dev \
 libmemcached-dev \
 libicu-dev \
 libgmp3-dev \
 libzip4 \
 procps \
 zip \
 unzip \
 iproute2 \
 git
####################################################################################
# Install Extension
####################################################################################

RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gmp iconv intl opcache mbstring gd pdo_mysql mysqli pcntl bcmath zip && pecl update-channels


# Redis
RUN pecl install redis && docker-php-ext-enable redis
# Swoole
RUN pecl install swoole && docker-php-ext-enable swoole
# Mcrypt
RUN apt-get install -y libmcrypt-dev libmhash-dev \
	&& pecl install mcrypt-1.0.3  && docker-php-ext-enable mcrypt
# Inotify
RUN pecl install inotify && docker-php-ext-enable inotify

# php composer
ADD composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer

# Setup Composer Source
RUN if [ $CHANGE_SOURCE = true ]; then \
	composer config -g repo.packagist composer https://packagist.phpcomposer.com \
;fi

RUN /sbin/ip route|awk '/default/ { print  $3,"\tdockerhost" }' >> /etc/hosts