FROM php:8.1-apache
RUN echo 'memory_limit = 2512M' > /usr/local/etc/php/conf.d/docker-php-memlimit.ini
RUN echo 'upload_max_filesize = 2512M' > /usr/local/etc/php/conf.d/docker-php-uploadlimit.ini
RUN echo 'post_max_size = 2512M' >> /usr/local/etc/php/conf.d/docker-php-uploadlimit.ini
RUN echo 'session.cookie_lifetime = 0' >> /usr/local/etc/php/conf.d/docker-php-uploadlimit.ini
RUN echo 'session.gc_probability = 0' >> /usr/local/etc/php/conf.d/docker-php-uploadlimit.ini

WORKDIR /code
RUN apt-get update && apt-get install -y \
    git zip unzip vim  xvfb libfontconfig


# Install extensions
RUN apt-get install -y \
        libfreetype6-dev \
        zlib1g-dev \
    && docker-php-ext-configure gd \
    && docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install pdo pdo_mysql

RUN apt-get -y install curl
RUN apt-get install -y \
    libpq-dev \
    wget \
    libmcrypt-dev \
    libzip-dev \
    openssl \
    libpq-dev \
    libpng-dev \
    && pecl install redis-6.0.0RC1 \
    && docker-php-ext-enable redis


RUN docker-php-ext-install zip
RUN docker-php-ext-install bcmath
RUN apt-get install -y libicu-dev
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl


ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions http


# Apache config
COPY docker/conf/vhost.conf /etc/apache2/sites-available/000-default.conf

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# RUN composer self-update --1

# prepare dirs
RUN chown www-data:www-data . /var/www
USER www-data:www-data
RUN mkdir vendor
COPY --chown=www-data:www-data . ./

# # Install php vendors
RUN COMPOSER_DISABLE_XDEBUG_WARN=1  composer install --verbose --prefer-dist --no-progress --no-interaction --optimize-autoloader


RUN php bin/console ca:cl
RUN rm -rf .git git* docker Dockerfile README.md

USER root


