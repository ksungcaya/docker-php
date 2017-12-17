FROM ubuntu:16.04

LABEL maintainer='ksungcaya'

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt-get update && apt-get install -y curl zip unzip git software-properties-common locales \
    && locale-gen en_US.UTF-8 \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y php5.6-fpm php5.6-cli \
       php5.6-mcrypt php5.6-gd php5.6-sqlite3 php5.6-mysql \
       php5.6-pgsql php5.6-imap php5.6-mbstring php5.6-xml php5.6-curl \
       php5.6-bcmath php5.6-mongo php5.6-intl php5.6-soap \
       php-pear php-memcached php-memcache php-redis

# Install apcu
RUN apt-get install -y php-apcu --no-install-recommends

# Install composer and cleanup
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir /run/php \
    && apt-get remove -y --purge software-properties-common \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD php-fpm.conf /etc/php/5.6/fpm/php-fpm.conf
ADD www.conf /etc/php/5.6/fpm/pool.d/www.conf

# create an ssh directory so we can mount the host ssh for private repo fetching
RUN mkdir /root/.ssh && echo "StrictHostKeyChecking no " > /root/.ssh/config

EXPOSE 9000

CMD ["php-fpm5.6"]
