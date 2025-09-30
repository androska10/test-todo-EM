FROM php:8.2-fpm

# Установка системных пакетов
RUN apt-get update && apt-get install -y \
    git curl libpng-dev libonig-dev libxml2-dev libzip-dev \
    libfreetype6-dev libjpeg62-turbo-dev libwebp-dev zip unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip sockets \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Убираем лимит памяти
RUN echo "memory_limit=-1" > /usr/local/etc/php/conf.d/memory-limit.ini

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

# 1. Копируем зависимости
COPY composer.json composer.lock ./

# 2. Устанавливаем БЕЗ скриптов
RUN composer install --no-dev --optimize-autoloader --no-interaction --no-scripts

# 3. Копируем ВСЁ приложение (включая artisan)
COPY . .

# 4. Запускаем Laravel-скрипты
RUN php artisan package:discover && \
    php artisan config:cache && \
    php artisan route:cache

# 5. Права на запись
RUN chown -R www-data:www-data storage bootstrap/cache \
    && chmod -R 775 storage bootstrap/cache