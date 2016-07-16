FROM php:7.0.8-fpm

# Setting PHP
COPY ./php.ini /usr/local/etc/php/php.ini
RUN apt-get update \
 && apt-get install -y \
    libicu-dev \
    libmcrypt-dev \
    libxml2-dev \
    unzip \
 && docker-php-ext-install pdo_mysql intl mcrypt mysqli


# Install composer
RUN /bin/bash -c "debconf-set-selections <<< \"postfix postfix/mailname string your.hostname.com\"" \
 && /bin/bash -c "debconf-set-selections <<< \"postfix postfix/main_mailer_type string 'Internet Site'\"" \
 && apt-get update \
    && apt-get install -y --no-install-recommends \
        git \
    && rm -rf /var/lib/apt/lists/*
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/bin/composer


# Install & Setting Postfix
RUN apt-get update \
    && apt-get install -y \
    postfix
RUN echo 'smtp_tls_CAfile = /etc/ssl/certs/ssl-cert-snakeoil.pem' >> /etc/postfix/main.cf \
 && echo 'smtp_tls_security_level = may' >> /etc/postfix/main.cf \
 && echo 'smtp_tls_loglevel = 1' >> /etc/postfix/main.cf


COPY ./services.sh /etc/services.sh
RUN chmod a+x /etc/services.sh
CMD ["/etc/services.sh"]