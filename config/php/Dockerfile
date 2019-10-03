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
  && version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;") \
    && curl -A "Docker" -o /tmp/blackfire-probe.tar.gz -D - -L -s https://blackfire.io/api/v1/releases/probe/php/linux/amd64/$version \
    && mkdir -p /tmp/blackfire \
    && tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp/blackfire \
    && mv /tmp/blackfire/blackfire-*.so $(php -r "echo ini_get('extension_dir');")/blackfire.so \
    && printf "extension=blackfire.so\nblackfire.agent_socket=tcp://blackfire:8707\n" > $PHP_INI_DIR/conf.d/blackfire.ini \
    && curl -A "Docker" -L https://blackfire.io/api/v1/releases/client/linux_static/amd64 | tar zxp -C /tmp/blackfire \
    && mv /tmp/blackfire/blackfire /usr/bin/blackfire \
    && rm -rf /tmp/blackfire /tmp/blackfire-probe.tar.gz \
  && rm -rf /var/lib/apt/lists/*

# Midnight commander
RUN apt-get update && apt-get install mc -y

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

COPY supervisord.conf /etc/supervisor
COPY consumer.conf /etc/supervisor/conf.d
COPY fpm.conf /etc/supervisor/conf.d
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]

# install mongodb ext
RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

# Permission fix
RUN usermod -u 1000 www-data

# This for case, when you won't use supervisord to start FPM
# uncoment this and remove fmp.conf then

#CMD ["php-fpm"]
#EXPOSE 9000