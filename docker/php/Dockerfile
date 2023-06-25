FROM php:8.2-apache-bookworm

# common install
RUN apt update \
    && apt install -y --no-install-recommends \
      apt-transport-https git-all gnupg libicu-dev libpq-dev libzip-dev mc unzip zip \
    && rm -rf /var/lib/apt/lists/* \
    && a2enmod rewrite \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl pdo_mysql pdo_pgsql opcache zip \
    && apt-get purge -y --auto-remove

# config
COPY files/ /

# https://hub.docker.com/_/composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# install symfony
RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash \
    && apt install -y symfony-cli

# Set not-root user and group
ARG user=appuser
ARG group=${user}
ARG uid=1000
ARG gid=1000
RUN groupadd -g ${gid} ${group} \
    && useradd -u ${uid} -g ${group} -s /bin/sh -m ${user}

# Switch to user
USER ${uid}:${gid}
COPY files/root/ /home/${user}/

WORKDIR /var/www/html/
