FROM php:7.3-fpm

#Copy composer.lock and compoer.json

# COPY composer.lock composer.json /var/www/

#set working directory
WORKDIR /var/www

#install dependencies
RUN apt-get update && apt-get install -y \
    libmagickwand-dev --no-install-recommends \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    libzip-dev \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    rsync \
    ffmpeg \
    libicu-dev \
    icu-devtools \
    zlib1g-dev g++ \
    jpegoptim optipng pngquant gifsicle \
    npm \
    nodejs
RUN printf "\n" | npm install -g svgo
RUN printf "\n" | pecl install imagick
RUN yes | pecl install xdebug
# RUN yes | pecl install intl

# && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
# && echo "xdebug.default_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
# && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
# && echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
# && echo "xdebug.remote_host=10.254.254.254" >> /usr/local/etc/php/conf.d/xdebug.ini \
# && echo "xdebug.remote_log=/var/www/html/xdebug.log" >> /usr/local/etc/php/conf.d/xdebug.ini \
# && echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
# && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini \
# && echo "xdebug.idekey=VSCODE"  >> /usr/local/etc/php/conf.d/xdebug.ini
COPY ./php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
#clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
# Install extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl
RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
RUN docker-php-ext-install gd
RUN docker-php-ext-enable xdebug
RUN docker-php-ext-enable imagick
# RUN docker-php-ext-enable intl
# ENV EXT_REDIS_VERSION=4.3.0 EXT_IGBINARY_VERSION=3.0.1
# RUN docker-php-source extract \
#     # igbinary
#     && mkdir -p /usr/src/php/ext/igbinary \
#     &&  curl -fsSL https://github.com/igbinary/igbinary/archive/$EXT_IGBINARY_VERSION.tar.gz | tar xvz -C /usr/src/php/ext/igbinary --strip 1 \
#     && docker-php-ext-install igbinary \
#     # redis
#     && mkdir -p /usr/src/php/ext/redis \
#     && curl -fsSL https://github.com/phpredis/phpredis/archive/$EXT_REDIS_VERSION.tar.gz | tar xvz -C /usr/src/php/ext/redis --strip 1 \
#     && docker-php-ext-configure redis --enable-redis-igbinary \
#     && docker-php-ext-install redis \
#     # cleanup
#     && docker-php-source delete


RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# Install composer
# RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# COPY --from=composer /usr/bin/composer /usr/bin/composer

# RUN composer global require hirak/prestissimo
# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
# RUN composer install --prefer-dist --no-ansi --no-interaction
COPY . /var/www
# COPY .env /var/www/.env
# COPY app:./vendor ./

# RUN chmod +x artisan




# Copy existing application directory permissions
RUN chown=www:www . /var/www

# RUN php artisan key:generate
# RUN php artisan cache:clear



# RUN composer dump-autoload --optimize
# Change current user to www
USER www
# COPY docker-entrypoint.sh /
# ENTRYPOINT ["/docker-entrypoint.sh"]
# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
