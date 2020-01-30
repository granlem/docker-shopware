# File forked from https://github.com/sprintcube/docker-compose-lamp/tree/7.4.x - Thanks!
FROM php:7.4.1-apache-buster

# Surpresses debconf complaints of trying to install apt packages interactively
# https://github.com/moby/moby/issues/4032#issuecomment-192327844
 
ARG DEBIAN_FRONTEND=noninteractive

# Update
RUN apt-get -y update --fix-missing && \
    apt-get upgrade -y && \
    apt-get --no-install-recommends install -y apt-utils && \
    rm -rf /var/lib/apt/lists/*


# Install useful tools and install important libaries
RUN apt-get -y update && \
    apt-get -y --no-install-recommends install nano wget \
dialog \
libsqlite3-dev \
libsqlite3-0 && \
    apt-get -y --no-install-recommends install default-mysql-client \
zlib1g-dev \
libzip-dev \
unzip \
libxml2 \
libssl-dev \
libxml2-dev \
libicu-dev && \
    apt-get -y --no-install-recommends install --fix-missing apt-utils \
build-essential \
git \
libpng-dev \
zlib1g-dev \
curl \
libonig-dev && \ 
    apt-get -y --no-install-recommends install --fix-missing libcurl4 \
libcurl4-openssl-dev \
zip \
libxml2 \
openssl && \
    rm -rf /var/lib/apt/lists/* && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install xdebug
RUN pecl install xdebug-2.8.0 && \
    docker-php-ext-enable xdebug

# Install redis
RUN pecl install redis-5.1.1 && \
    docker-php-ext-enable redis

# Other PHP7 Extensions
RUN docker-php-ext-install pdo_mysql && \
    docker-php-ext-install pdo_sqlite && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install curl && \
    docker-php-ext-install tokenizer && \
    docker-php-ext-install json && \
    docker-php-ext-install zip && \
    docker-php-ext-install -j$(nproc) intl && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install dom && \
    docker-php-ext-install pdo && \
    docker-php-ext-install simplexml && \
    docker-php-ext-install fileinfo && \
    docker-php-ext-install iconv && \
    docker-php-ext-install xml && \
    docker-php-ext-install gd && \
    docker-php-ext-install gettext

# Install Freetype 
RUN apt-get -y update && \
    apt-get --no-install-recommends install -y libfreetype6-dev \
libjpeg62-turbo-dev \
libpng-dev && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure gd

# Enable apache modules
RUN a2enmod rewrite headers

# Download shopware
ADD https://releases.shopware.com/sw6/install_6.1.1_1580132211.zip /usr/src/
RUN unzip -q /usr/src/install_6.1.1_1580132211.zip -d /var/www/html/
RUN chown -R www-data:www-data /var/www/html

# Adjust apache config
ADD https://raw.githubusercontent.com/granlem/docker-shopware/master/image/000-default.conf /etc/apache2/sites-enabled/

# Cleanup
RUN rm -rf /usr/src/*

