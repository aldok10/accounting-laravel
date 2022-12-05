FROM akarendra835/web-srv:8.0

USER root

# install application dependencies
WORKDIR /var/www/app

ARG TZ=Asia/Jakarta

RUN apk add --no-cache gnu-libiconv php8-iconv openssl curl bash

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone

# Install composer from the official image
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Create symlink so programs depending on `php` still function
# RUN ln -s /usr/bin/php81 /usr/bin/php

# Copy composer.* to container
COPY composer.* .

# Run composer install to install the dependencies
RUN composer install --no-scripts --no-autoloader --ansi --no-interaction

# set application environment variables
ENV APP_NAME="Laravel Apps" \
    APP_ENV=production \
    APP_DEBUG=false

# Copy Nginx configuration
COPY ./docker/nginx/ /etc/nginx/

RUN delgroup dialout

# copy application code
COPY . .

RUN composer dump-autoload -o \
    && chmod -R 777 storage bootstrap/cache /etc/nginx/ssl

# RUN composer update

USER nobody

EXPOSE 80
EXPOSE 443

# run supervisor
# CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
