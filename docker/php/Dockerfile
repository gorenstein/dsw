FROM php:8.3-apache-bookworm

# common install
RUN apt update && \
    apt install -y --no-install-recommends \
        apt-transport-https bash-completion git-all gnupg libzip-dev mc unzip zip && \
    a2enmod rewrite && \
    docker-php-ext-install opcache zip && \
    rm -rf /var/lib/apt/lists/* &&  apt purge -y --auto-remove

# apcu - php ext
RUN pecl install apcu && \
    docker-php-ext-enable apcu

# xdebug - php ext
RUN pecl install xdebug && \
    docker-php-ext-enable xdebug

# intl - php ext
RUN apt -y update && \
    apt install -y libicu-dev && \
    docker-php-ext-configure intl && \
    docker-php-ext-install intl && \
    rm -rf /var/lib/apt/lists/* && apt purge -y --auto-remove

# mysql - php ext
RUN docker-php-ext-install pdo_mysql

# pgsql - php ext
RUN apt update && \
    apt install -y libpq-dev && \
    docker-php-ext-install pdo_pgsql && \
    rm -rf /var/lib/apt/lists/* && apt purge -y --auto-remove

# redis - php ext
RUN pecl install redis && \
    docker-php-ext-enable redis

# AMQP - php ext
RUN apt update && \
    apt install -y librabbitmq-dev && \
    pecl install amqp && \
    docker-php-ext-enable amqp && \
    rm -rf /var/lib/apt/lists/* && apt purge -y --auto-remove

# xls - php ext
RUN apt update && \
    apt install -y libxslt-dev && \
    docker-php-ext-install xsl && \
    rm -rf /var/lib/apt/lists/* && apt purge -y --auto-remove

# gb - php ext
RUN apt update && \
    apt install -y libpng-dev libjpeg-dev && \
    docker-php-ext-configure gd && \
    docker-php-ext-install gd && \
    rm -rf /var/lib/apt/lists/* && apt purge -y --auto-remove


# config
COPY files/ /

# nodejs
RUN apt update && \
    apt install -y nodejs npm && \
    rm -rf /var/lib/apt/lists/* && apt purge -y --auto-remove

# https://hub.docker.com/_/composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Install Symfony CLI
RUN mkdir -p /tmp/symfony-cli && \
    curl -1sLf 'https://github.com/symfony-cli/symfony-cli/releases/latest/download/symfony-cli_linux_amd64.tar.gz' | tar xz -C /tmp/symfony-cli && \
    cp /tmp/symfony-cli/symfony /usr/local/bin/symfony && \
    chmod a+x /usr/local/bin/symfony && \
    rm -rf /tmp/symfony-cli


# Set not-root user and group
ARG user=appuser
ARG group=${user}
ARG uid=1000
ARG gid=1000
RUN groupadd -g ${gid} ${group} && \
    useradd -u ${uid} -g ${group} -s /bin/sh -m ${user}

# Switch to user
USER ${uid}:${gid}
COPY files/root/ /home/${user}/

WORKDIR /var/www/html/
