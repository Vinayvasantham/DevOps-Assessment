FROM php:8.1-fpm

# Install system dependencies
RUN apt update && apt install -y \
    git \
    unzip \
    libzip-dev \
    zip \
    curl \
    && docker-php-ext-install pdo pdo_mysql zip

# Install Composer globally
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

# Copy app files into container
COPY . .

# Install PHP dependencies (production mode)
RUN composer install

# Fix permissions for Yii2 runtime and assets directories
RUN mkdir -p /var/www/html/runtime /var/www/html/web/assets \
    && chown -R www-data:www-data /var/www/html/runtime /var/www/html/web/assets

EXPOSE 9000

CMD ["php-fpm"]

