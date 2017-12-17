FROM ubuntu:16.04

LABEL maintainer='ksungcaya'

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt-get update && apt-get install -y curl zip unzip git software-properties-common locales \
    && locale-gen en_US.UTF-8 \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y php7.1-fpm php7.1-cli \
       php7.1-mcrypt php7.1-gd php7.1-sqlite3 php7.1-mysql \
       php7.1-pgsql php7.1-imap php7.1-mbstring php7.1-xml php7.1-curl \
       php7.1-bcmath php7.1-mongo php7.1-intl php7.1-soap \
       php-pear php-memcached php-memcache php-redis

# Install apcu
RUN apt-get install -y php-apcu --no-install-recommends

# Install composer and cleanup
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir /run/php \
    && apt-get remove -y --purge software-properties-common \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD php-fpm.conf /etc/php/7.1/fpm/php-fpm.conf
ADD www.conf /etc/php/7.1/fpm/pool.d/www.conf

# create an ssh directory so we can mount the host ssh for private repo fetching
RUN mkdir /root/.ssh && echo "StrictHostKeyChecking no " > /root/.ssh/config

EXPOSE 9000

CMD ["php-fpm7.1"]
