FROM ubuntu:18.04

LABEL maintainer='ksungcaya'

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt-get update \
    && apt-get install -y gnupg tzdata software-properties-common locales \
    && echo "UTC" > /etc/timezone \
    && dpkg-reconfigure -f noninteractive tzdata \
    && locale-gen en_US.UTF-8

RUN apt-get update && apt-get install -y curl zip unzip git supervisor sqlite3 nginx \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y php7.3-fpm php7.3-cli \
    php7.3-gd php7.3-sqlite3 php7.3-mysql \
    php7.3-pgsql php7.3-imap php7.3-mbstring php7.3-xml php7.3-curl \
    php7.3-bcmath php7.3-mongo php7.3-intl php7.3-soap php7.3-readline \
    php-pear php-memcached php-memcache php-redis

# Install apcu
RUN apt-get install -y php-apcu --no-install-recommends

# Install composer and cleanup
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir /run/php \
    && apt-get remove -y --purge software-properties-common \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && echo "daemon off;" >> /etc/nginx/nginx.conf


RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log


ADD default /etc/nginx/sites-available/default
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ADD php-fpm.conf /etc/php/7.3/fpm/php-fpm.conf
ADD docker-entrypoint /usr/bin/docker-entrypoint
RUN chmod +x /usr/bin/docker-entrypoint

# create an ssh directory so we can mount the host ssh for private repo fetching
RUN mkdir /root/.ssh && echo "StrictHostKeyChecking no " > /root/.ssh/config

EXPOSE 80 443

ENTRYPOINT ["docker-entrypoint"]
