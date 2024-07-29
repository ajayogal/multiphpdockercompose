# Use the official PHP image with FPM
FROM php:8.3.9-fpm

# Set the working directory
WORKDIR /var/www

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install pdo pdo_mysql gd

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copy existing application directory contents
COPY . /var/www

# Set the correct permissions
RUN chown -R www-data:www-data /var/www \
    && chmod -R 755 /var/www

# Expose port 9000 and start PHP-FPM server
EXPOSE 9080

# Configure PHP-FPM to listen on the correct port
RUN echo "listen = 0.0.0.0:9080" >> /usr/local/etc/php-fpm.d/zz-docker.conf

CMD ["php-fpm"]
