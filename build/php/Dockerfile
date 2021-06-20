FROM php:8-fpm-alpine

RUN apk add --update --no-cache \
        bash \
        icu-dev \
        curl \
        nodejs \
		nodejs-npm \
        git \
    && docker-php-ext-install pdo_mysql intl \
    && rm -rf /tmp/* /var/tmp/*

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER=1

RUN curl -sS https://get.symfony.com/cli/installer | bash && mv /root/.symfony/bin/symfony /usr/bin/symfony \
    && mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

COPY php.ini /usr/local/etc/php/conf.d/zzz-php.ini

ARG LINUX_USER
ARG LINUX_USER_ID
ARG LINUX_GROUP
ARG LINUX_GROUP_ID

RUN addgroup -S ${LINUX_GROUP} -g ${LINUX_GROUP_ID} && adduser -S ${LINUX_USER} -G ${LINUX_GROUP} --uid ${LINUX_USER_ID}

RUN mkdir /var/www/application
RUN chown -R ${LINUX_USER}:${LINUX_GROUP} /var/www/application

USER ${LINUX_USER}

WORKDIR /var/www/application
