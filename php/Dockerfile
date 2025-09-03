# Base image with PHP and required extensions
FROM php:7.4-fpm

RUN apt-get update && apt-get install -y \
    zip unzip curl libzip-dev libonig-dev libxml2-dev\
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    nginx \
    supervisor openssl && \
    docker-php-ext-install pdo pdo_mysql zip mbstring fileinfo opcache && \
    apt-get clean
RUN curl -sL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs
WORKDIR /var/www/html
COPY --from=composer:2.7 /usr/bin/composer /usr/bin/composer

COPY . /var/www/html

RUN composer install --no-interaction --prefer-dist --optimize-autoloader

COPY nginx.conf /etc/nginx/nginx.conf
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN mkdir -p /var/cache/nginx /var/run /var/log/nginx && \
    chown -R www-data:www-data /var/www/html && \
    chmod -R 755 /var/www/html && \
    chown -R www-data:www-data /var/cache/nginx && \
    chown -R www-data:www-data /etc/nginx && \
    chown -R www-data:www-data /var/log/nginx && \
    touch /var/run/nginx.pid && chmod -R 755 /var/www/html/storage /var/www/html/bootstrap/cache && \
    chown -R www-data:www-data /var/run/nginx.pid


# Expose ports
EXPOSE 81 9000

# Start with supervisord
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
