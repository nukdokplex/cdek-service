FROM php:7.4-apache

ARG ALLOW_ORIGIN="*"

RUN apt-get update && apt-get install -y \
    libcurl4 \
    libcurl4-openssl-dev \
    curl \
    wget \
    && docker-php-ext-configure curl \
    && docker-php-ext-install curl \
    && rm -rf /var/lib/apt/lists/*

RUN echo "Header always set Access-Control-Allow-Origin \"$ALLOW_ORIGIN\"" >/etc/apache2/conf-available/cors.conf \
    && a2enconf cors \
    && a2enmod headers

RUN --mount=type=secret,id=cdek_login \
    --mount=type=secret,id=cdek_pass \
    export CDEK_LOGIN=$(cat /run/secrets/cdek_login) \
    && export CDEK_PASS=$(cat /run/secrets/cdek_pass) \
    && wget https://raw.githubusercontent.com/cdek-it/widget/2e2f9a9233e47521f60f482ce1efb0ce2436ada0/dist/service.php -O /var/www/html/service.php \
    && sed -i "s/'cdek-login'/'${CDEK_LOGIN}'/" /var/www/html/service.php \
    && sed -i "s/'cdek-pass'/'${CDEK_PASS}'/" /var/www/html/service.php
