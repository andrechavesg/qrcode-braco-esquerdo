FROM php:7.2.10-fpm

RUN apt-get update \
    && apt-get install -y --no-install-recommends vim curl debconf subversion git apt-transport-https apt-utils \
    build-essential locales acl mailutils wget zip unzip \
    gnupg gnupg1 gnupg2 \
    zlib1g-dev \
    libpng-dev \
    libxml2-dev \
    libxrender1 \
    libfontconfig1 \
    libx11-dev \
    libssl1.0-dev \
    libjpeg62 \
    libxtst6 \
    ffmpeg \
    sudo \
    wget

RUN apt-get install -y cron

RUN docker-php-ext-install pdo pdo_mysql zip xml gd
#RUN pecl install xdebug-2.6.0 \
#    && docker-php-ext-enable xdebug

RUN curl -sSk https://getcomposer.org/installer | php -- --disable-tls && \
	mv composer.phar /usr/local/bin/composer
#
#RUN wget --no-check-certificate https://phar.phpunit.de/phpunit-6.5.3.phar && \
#    mv phpunit*.phar phpunit.phar && \
#    chmod +x phpunit.phar && \
#    mv phpunit.phar /usr/local/bin/phpunit

RUN groupadd dev -g 999
RUN useradd dev -g dev -d /home/dev -m
RUN passwd -d dev

RUN rm -rf /var/lib/apt/lists/*
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    echo "pt_BR.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen

RUN echo "dev ALL=(ALL) ALL" > /etc/sudoers

COPY php.ini /usr/local/etc/php/php.ini
COPY php-fpm-pool.conf /usr/local/etc/php-fpm.d/www.conf
COPY root /etc/cron.d/cronjobs

WORKDIR /home/wwwroot/

RUN mkdir -p var/
RUN chmod 777 -R var/
RUN crontab /etc/cron.d/cronjobs

EXPOSE 9000

