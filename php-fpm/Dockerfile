FROM composer:2 as composer_stage

RUN rm -rf /var/www && mkdir -p /var/www/html
WORKDIR /var/www/html

COPY composer.json composer.lock ./

# This are production settings, I'm running with 'no-dev', adjust accordingly 
# if you need it
RUN composer install --ignore-platform-reqs --prefer-dist --no-scripts --no-progress --no-interaction --no-dev --no-autoloader
RUN composer dump-autoload --optimize --apcu --no-dev

FROM php:8.0.2-fpm-alpine
WORKDIR /var/www/html

RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install mysqli

COPY php-fpm/config/php.ini /usr/local/etc/php/

COPY --from=composer_stage /var/www/html /var/www/html

COPY src src/

CMD ["php-fpm"]

EXPOSE 9000
