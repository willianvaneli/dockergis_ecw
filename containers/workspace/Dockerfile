FROM phusion/baseimage:focal-1.0.0

# Define a linguagem padrão para português
ARG LOCALE=en_US.UTF-8
RUN locale-gen $LOCALE \
    && update-locale LANG=$LOCALE

# Define a timezone
ARG TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Adiciona pacotes obrigatórios
ARG PHP_VERSION=8.0
RUN set -eux \
    && apt-get install -y software-properties-common \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --allow-downgrades --allow-remove-essential \
      --allow-change-held-packages \
      php${PHP_VERSION}-cli \
      php${PHP_VERSION}-common \
      php${PHP_VERSION}-curl \
      php${PHP_VERSION}-intl \
      php${PHP_VERSION}-xml \
      php${PHP_VERSION}-mbstring \
      php${PHP_VERSION}-mysql \
      php${PHP_VERSION}-pgsql \
      php${PHP_VERSION}-sqlite \
      php${PHP_VERSION}-sqlite3 \
      php${PHP_VERSION}-zip \
      php${PHP_VERSION}-bcmath \
      php${PHP_VERSION}-memcached \
      php${PHP_VERSION}-gd \
      php${PHP_VERSION}-dev \
      pkg-config \
      libcurl4-openssl-dev \
      libedit-dev \
      libssl-dev \
      libxml2-dev \
      libzip-dev zip unzip \
      xz-utils \
      libsqlite3-dev \
      rsync \
      sqlite3 \
      git \
      curl \
      vim \
      nano \
      tree \
      postgresql-client \
      apt-utils \
    && apt-get clean \
    && curl -s http://getcomposer.org/installer | php \
    && echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc \
    && mv composer.phar /usr/local/bin/composer

# Adiciona um usuário não-root
ARG PUNAME=workspace
ARG PUID=1000
ARG PGNAME=workspace
ARG PGID=1000

RUN groupadd -g ${PGID} ${PGNAME} \
    && useradd -l -u ${PUID} -g ${PGID} -m ${PUNAME} \
    && usermod -p "*" ${PUNAME} -s /bin/bash

# Atualiza o composer
RUN set -eux; \
    composer self-update --2; 

# Exporta o path do vendor
RUN echo "" >> ~/.bashrc && \
    echo 'export PATH="$HOME/.composer/vendor/bin:$PATH"' >> ~/.bashrc

RUN echo "" >> ~/.bashrc && \
    echo 'export PATH="/var/www/vendor/bin:$PATH"' >> ~/.bashrc

# Instala o Node
RUN curl -fsSL https://deb.nodesource.com/setup_17.x | bash -
RUN apt-get install -y nodejs

# Limpa arquivos temporários do apt-get
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

# Define o workdir padrão
WORKDIR /var/www