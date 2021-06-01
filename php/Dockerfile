FROM php:8.0.3-fpm-alpine3.13
#FROM php:7.4.16-fpm-alpine3.13

WORKDIR /var/www

COPY dependencias.sh /var/www/dependencias.sh

RUN apk update && apk upgrade 
RUN apk add --no-cache --update   \
# App 
                git \
                redis \
                bash \
                wget \
                curl \
                zip \
                yarn \
# Instalar este paquete para poder conectarse desde este servidor al serveDB
                postgresql-client \
# App necesaria para instalacion de 
# extensiones php
                libpq \
                libzip-dev \
                curl-dev \
        && docker-php-ext-install -j "$(nproc)" \
                # openssl \
                # sodium \
                curl \
                zip \
                pdo_mysql \
                pdo 

############################
#       PDO_PGSQL          #     
############################
# Instalacion de paquete para postgresql
RUN apk add --no-cache --update postgresql-dev \
# Configuracion de la extension php a instalar (Si es necesario)
        && docker-php-ext-configure pgsql -with-pgsql=/usr/include/postgresql/ 
# Instalacion de extension php
RUN docker-php-ext-install -j "$(nproc)" pdo_pgsql

############################
#          GD              #     
############################
# Instalacion de paquete para gd
RUN apk add --no-cache --update \             
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libpng \
        zlib-dev \
# Configuracion de la extension php a instalar (Si es necesario)
        && docker-php-ext-configure gd \
        --with-freetype=/usr/local/ \
        --with-jpeg=/usr/local/ \
# Instalacion de extension php
        && docker-php-ext-install -j "$(nproc)" gd


############################
#            INTL          #     
############################
# Instalacion de paquete para intl
RUN apk add --no-cache --update icu-dev \
# Instalacion de extension php
        && docker-php-ext-install -j "$(nproc)" intl

############################
#            XSL          #     
############################
# Instalacion de paquete para xsl
RUN apk add --no-cache --update libxslt-dev \
# Instalacion de extension php
        && docker-php-ext-install -j "$(nproc)" xsl 

RUN apk del --no-cache libpng-dev libjpeg-turbo-dev freetype-dev 


RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

RUN  wget https://get.symfony.com/cli/installer -O - | bash \
        && mv /root/.symfony/bin/symfony /usr/local/bin/symfony


EXPOSE 9000

