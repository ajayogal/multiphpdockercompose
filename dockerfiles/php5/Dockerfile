#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#

FROM php:5.6-fpm
WORKDIR /var/www

#
#--------------------------------------------------------------------------
# Software's Installation
#--------------------------------------------------------------------------
#
# Installing tools and PHP extentions using "apt", "docker-php", "pecl",
#

# Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
#         "libpng12-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",
RUN set -eux; \
    sed -i 's/^deb /# deb /g' /etc/apt/sources.list; \
    echo 'deb http://archive.debian.org/debian/ stretch main contrib non-free' >> /etc/apt/sources.list; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
            curl \
            libmemcached-dev \
            libz-dev \
            libpq-dev \
            libjpeg-dev \
            libpng-dev \
            libfreetype6-dev \
            libssl-dev \
            # Install MySQL client
            mysql-client \
            libmcrypt-dev; \
    # cleanup
    rm -rf /var/lib/apt/lists/*


RUN set -eux; \
    # Install the PHP mcrypt extention
    docker-php-ext-install mcrypt; \
    # Install the PHP pdo_mysql extention
    docker-php-ext-install pdo_mysql mysqli; \
    # Install the PHP pdo_pgsql extention
    docker-php-ext-install pdo_pgsql; \
    # Install the PHP gd library
    docker-php-ext-configure gd \
            --enable-gd-native-ttf \
            --with-jpeg-dir=/usr/lib \
            --with-freetype-dir=/usr/include/freetype2; \
    docker-php-ext-install gd; \
    php -r 'var_dump(gd_info());'

RUN pwd
#
#--------------------------------------------------------------------------
# Set Permissions and Configuration
#--------------------------------------------------------------------------
RUN set -eux; \
    # Set permissions for storage folder (adjust as needed)
    # chmod -R 775 ./storage; \
    # Copy custom php.ini if available
    if [ -f "./php.ini" ]; then \
        cp ./php.ini /usr/local/etc/php/php.ini; \
    fi; \
    # Show installed PHP modules and configurations
    php -m; \
    php -i | grep 'php.ini'

#
#--------------------------------------------------------------------------
# Expose Ports and Define Command to Start PHP-FPM
#--------------------------------------------------------------------------
EXPOSE 9000
CMD ["php-fpm"]
