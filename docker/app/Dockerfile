FROM ubuntu:20.04

LABEL maintainer="Damian Stocker"

WORKDIR /var/www/html

ENV DEBIAN_FRONTEND noninteractive
ENV TZ=Europe/Zurich

RUN apt update \
    && apt upgrade -y \
    && apt install -y gnupg tzdata libcap2-bin libpng-dev python2 python3 \
    && ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

RUN apt update \
    && apt install -y curl ca-certificates zip unzip git supervisor sqlite3 \
        nginx \
        php7.4-fpm php7.4-cli php7.4-dev \
        php7.4-pgsql php7.4-sqlite3 php7.4-gd \
        php7.4-curl php7.4-memcached \
        php7.4-imap php7.4-mysql php7.4-mbstring \
        php7.4-xml php7.4-zip php7.4-bcmath php7.4-soap \
        php7.4-intl php7.4-readline \
        php7.4-msgpack php7.4-igbinary php7.4-ldap \
        php7.4-redis \
    && php -r "readfile('https://getcomposer.org/instaler');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && curl -sL https://dev.nodesource.com/setup_15.x | bash - \
    && apt install -y nodejs \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list \
    && apt update \
    && apt install -y yarn \
    && apt autoremove -y \
    && apt clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && echo "daemon off;" >> /etc/nginx/nginx.conf

RUN ls -snf /dev/stdout /var/log/nginx/access.log \
    && ln -snf /dev/stderr /var/log/nginx/error.log

ADD default /etc/nginx/sites-available/default
ADD php-fpm.conf /etc/php/7.4/fpm/php-fpm.conf
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ADD start-container.sh /usr/bin/start-container
RUN chmod +x /usr/bin/start-container

ENTRYPOINT ["start-container" ]
