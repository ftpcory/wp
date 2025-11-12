FROM php:8.4-cli
LABEL org.opencontainers.image.authors="corycollier@corycollier.com"

RUN apt -y update && \
    apt -y upgrade && \
    apt -y install \
        git \
        vim \
        zip \
        bash \
        cron \
        curl \
        less \
        sudo \
        wget \
        rsync \
        unzip \
        groff \
        gnupg2 \
        python3 \
        sqlite3 \
        binutils \
        logrotate \
        colordiff \
        net-tools \
        libpng-dev \
        libgmp-dev \
        libzip-dev \
        zlib1g-dev \
        libxslt-dev \
        libjpeg-dev \
        lsb-release \
        libmcrypt-dev \
        libsqlite3-dev \
        mariadb-backup \
        ca-certificates \
        bash-completion \
        libfreetype6-dev \
        python3-setuptools \
		libjpeg62-turbo-dev \
        apt-transport-https \
        default-mysql-client \
        libperl4-corelibs-perl

# Add all of the php specific packages
RUN docker-php-source extract \
    && docker-php-ext-configure gd \
        --enable-gd-jis-conv \
        --with-jpeg=/usr/include/ \
        --with-freetype=/usr/include/ \
    && docker-php-ext-install \
        gd \
        gmp \
        pdo \
        xsl \
        zip \
        exif \
        pcntl \
        bcmath \
        mysqli \
        opcache \
        pdo_mysql \
        pdo_sqlite

# Install modules not able to be installed any other way
RUN pecl install xdebug redis \
    && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini \
    && rm -rf /tmp/pear

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && rm awscliv2.zip \
    && rm -rf /aws

COPY config/custom.php.ini /usr/local/etc/php/conf.d/custom.php.ini
COPY config/cron-wordpress /app/cron-wordpress
COPY dotfiles/.* /root/
COPY scripts/* /usr/local/bin

USER www-data

WORKDIR /app

USER root
CMD cron -f
