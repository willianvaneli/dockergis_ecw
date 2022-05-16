ARG PHP_VERSION=8.0
FROM php:${PHP_VERSION}-fpm

# Adiciona pacotes
RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
    apt-utils \
    gnupg2 \
    libzip-dev zip unzip \
    curl \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libwebp-dev \
    libxpm-dev \
    libmcrypt-dev \
    libonig-dev; \
    rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure zip; \
    docker-php-ext-install zip; \
    docker-php-ext-install pdo_pgsql; \
    docker-php-ext-install pgsql; \
    docker-php-ext-configure gd \
    --prefix=/usr \
    --with-jpeg \
    --with-webp \
    --with-xpm \
    --with-freetype; \
    docker-php-ext-install gd; 

# Instalação do Postgres + Postgis
ARG INSTALL_PG_CLIENT=true
ARG INSTALL_POSTGIS=true
RUN if [ ${INSTALL_PG_CLIENT} = true ]; then \
    apt-get install -yqq gnupg \
    && . /etc/os-release \
    && echo "deb http://apt.postgresql.org/pub/repos/apt $VERSION_CODENAME-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
    && curl -sL https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
    && apt-get update -yqq \
    && apt-get install -yqq postgresql-client-12 postgis; \
    if [ ${INSTALL_POSTGIS} = true ]; then \
      apt-get install -yqq postgis; \
    fi \
    && apt-get purge -yqq gnupg \
;fi

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

# Define o workdir padrão
WORKDIR /var/www