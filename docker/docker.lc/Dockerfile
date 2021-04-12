FROM php:7.3-fpm

# Copy composer.lock and composer.json
COPY composer.lock composer.json /var/www/html/laravel-docker/

# Set working directory
WORKDIR /var/www/html/laravel-docker

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    nano \
    unzip \
    git \
    curl \
    libzip-dev

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl
RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
RUN docker-php-ext-install gd

# Install composer, run composer with composer image so production do not need composer dependency
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
#RUN groupadd -g 1000 www-data; exit 0
#RUN useradd -u 1000 -ms /bin/bash -g www-data www-data; exit 0

# Copy existing application directory contents
COPY . /var/www/html/laravel-docker

# Copy existing application directory permissions
# COPY --chown=root:root . /var/www/html/bemor
# Ensure all future content in the folder will inherit group ownership
#RUN chmod -R ug+rwx storage bootstrap/cache
RUN chmod -R 777 storage bootstrap/cache

# Change current user to www-data
# USER www-data

# Expose port 9000 and start php-fpm server
EXPOSE 9000

CMD ["php-fpm"]
