FROM php:7.3-fpm

# Install recommended extensions for Symfony
RUN apt-get update && apt-get install -y \
        libicu-dev \
        git \
        zip \
        libzip-dev \
        imagemagick \
        libmagickwand-dev \
    && docker-php-ext-install \
        intl \
        opcache \
    && docker-php-ext-enable \
        intl \
        opcache \
    && apt-get install -y  \
    && pecl install imagick \
    && docker-php-ext-install mysqli pdo_mysql \
    && docker-php-ext-enable imagick

# Install others
RUN apt-get -qq update && apt-get -qq -y install  \
    automake \
    cmake \
    g++ \
    git \
    libicu-dev \
    libmagickwand-dev \
    libpng-dev \
    librabbitmq-dev \
    libreadline-dev \
    pkg-config \
    ssh-client \
    supervisor \
    zlib1g-dev \
    libzip-dev \
  && docker-php-ext-install \
    bcmath \
    gd \
    intl \
    opcache \
    pdo_mysql \
    zip \
  && git clone git://github.com/alanxz/rabbitmq-c.git \
    && cd rabbitmq-c \
    && mkdir build && cd build \
    && cmake -DENABLE_SSL_SUPPORT=OFF .. \
    && cmake --build . --target install  \
    && pecl install amqp imagick igbinary \
  && docker-php-ext-enable amqp imagick igbinary \
  && rm -rf /var/lib/apt/lists/*

# Midnight commander
RUN apt-get update && apt-get install mc -y

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

COPY supervisord.conf /etc/supervisor

# if you want some additional process
# COPY consumer.conf /etc/supervisor/conf.d

COPY fpm.conf /etc/supervisor/conf.d
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]

# if you want to install mongodb ext uncomment
# RUN pecl install mongodb && docker-php-ext-enable mongodb

# Remove file which is brake installing
RUN rm /usr/local/etc/php-fpm.d/www.conf.default
RUN rm /usr/local/etc/php-fpm.d/zz-docker.conf

RUN apt-get update

# Install Postgre PDO
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

# Permission fix
RUN usermod -u 1000 www-data

# This for case, when you won't use supervisord to start FPM
# uncoment this and remove fmp.conf then
# CMD ["php-fpm"]
# EXPOSE 9000