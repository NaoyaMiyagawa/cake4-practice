FROM php:8.2-rc-apache-buster

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# node
RUN curl -SL https://deb.nodesource.com/setup_16.x | bash \
    && apt-get update \
    && apt-get install -y --no-install-recommends nodejs \
    && npm install -g npm@latest \
    && npm install -g yarn

# install modules
RUN apt-get update \
    # apt-get install
    && apt-get install -y --no-install-recommends \
    unzip git vim zsh \
    libzip-dev libicu-dev libpq-dev \
    libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    # docker-php-ext-install
    && docker-php-ext-install \
    gd intl zip \
    pdo_pgsql pgsql \
    # others
    && a2enmod rewrite \
    # clear cache
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# configure modules
RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-configure gd --with-freetype --with-jpeg

# composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 80 443
