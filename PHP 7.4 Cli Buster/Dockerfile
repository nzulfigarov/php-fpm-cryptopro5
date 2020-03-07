FROM php:7.4-cli-buster


RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
        curl \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update -yqq \
    && apt-get install -yqq \
        apt-utils \
        libzip-dev \
        zip \
        unzip \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip \
    && rm -rf /var/lib/apt/lists/*


# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

RUN usermod -u 1000 www-data

RUN mkdir -p /home/cryptopro/downloads


COPY linux-amd64_deb.tgz /home/cryptopro/downloads/linux-amd64_deb.tgz
COPY php.ini /usr/local/etc/php/php.ini

RUN mkdir -p /home/cryptopro/installer

WORKDIR /home/cryptopro/installer

RUN tar -xvf /home/cryptopro/downloads/linux-amd64_deb.tgz \
    && bash linux-amd64_deb/install.sh \
    && cd /bin \
    && ln -s /opt/cprocsp/bin/amd64/certmgr \
    && ln -s /opt/cprocsp/bin/amd64/cpverify \
    && ln -s /opt/cprocsp/bin/amd64/cryptcp \
    && ln -s /opt/cprocsp/bin/amd64/csptest \
    && ln -s /opt/cprocsp/bin/amd64/csptestf \
    && ln -s /opt/cprocsp/bin/amd64/der2xer \
    && ln -s /opt/cprocsp/bin/amd64/inittst \
    && ln -s /opt/cprocsp/bin/amd64/wipefile \
    && ln -s /opt/cprocsp/sbin/amd64/cpconfig \
    && rm -rf /home/cryptopro


WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
