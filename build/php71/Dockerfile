FROM php:7.1.33-fpm-stretch
MAINTAINER mofei <root@mofei.org>

# ARGS
ARG CHANGE_SOURCE=false

# Change Timezone
ARG TIME_ZONE=UTC
ENV TIME_ZONE ${TIME_ZONE}
RUN ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone

# Copy Extension Packages

#COPY redis-3.1.3.tgz  /tmp/redis.tgz
#COPY mcrypt-1.0.0.tgz /tmp/mcrypt.tgz

# Change China Sources
COPY sources.list /etc/apt/china.sources.list
RUN if [ ${CHANGE_SOURCE} = true ]; then \
	mv /etc/apt/sources.list /etc/apt/source.list.bak && mv /etc/apt/china.sources.list /etc/apt/sources.list \
;fi

RUN apt-get -o Acquire::Check-Valid-Until=false update && apt-get -y upgrade

# Install Base Components
RUN apt-get install -y --no-install-recommends \
		libcurl4-openssl-dev \
		libedit-dev \
		libsodium-dev \
		libsqlite3-dev \
		libssl-dev \
		libxml2-dev \
		zlib1g-dev \
		libpng-dev \
		libjpeg-dev \
		libfreetype6-dev \
		ntpdate \
		cron \
		vim \
		unzip \
		git \
		iproute2 \
		zip
####################################################################################
# Install Extension
####################################################################################

RUN docker-php-ext-configure gd --with-freetype-dir --with-jpeg-dir
RUN docker-php-ext-install gd pdo_mysql mysqli zip bcmath

# Redis
RUN pecl install redis && docker-php-ext-enable redis
# Swoole
RUN pecl install swoole && docker-php-ext-enable swoole
# Mcrypt
RUN apt-get install -y libmcrypt-dev libmhash-dev \
	&& pecl install mcrypt-1.0.0 && docker-php-ext-enable mcrypt
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
