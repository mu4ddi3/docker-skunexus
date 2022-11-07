FROM php:7.4.16-fpm-buster

RUN apt-get update \
    && apt-get upgrade -y
RUN apt-get install -y --no-install-recommends \
    vim \
    curl \
    wget \
    git \
    ca-certificates \
    apt-utils \
    zip \
    unzip \
    libicu-dev \
    libonig-dev \
    libzip-dev \
    libxml2-dev \
    libmcrypt-dev \
    gnupg

RUN docker-php-ext-install \
    pdo \
    pdo_mysql
RUN docker-php-ext-configure intl
RUN docker-php-ext-install \
    mbstring \
    intl \
    zip \
    soap \
    opcache \
    bcmath \
    sockets
RUN pecl install mcrypt-1.0.5 \
    && docker-php-ext-enable mcrypt
RUN pecl install apcu \
    && docker-php-ext-enable apcu

# Install xdebug if needed
ARG WITH_XDEBUG="false"

RUN if [ $WITH_XDEBUG = "true" ] ; then \
        pecl install xdebug; \
        docker-php-ext-enable xdebug; \
    fi ;

RUN export DEBIAN_FRONTEND=noninteractive
#COPY php-fpm-pool.conf /etc/php/8.0/pool.d/www.conf

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

## Node.js
RUN curl -sL https://deb.nodesource.com/setup_16.x -o nodesource_setup.sh
#RUN bash nodesource_setup.sh
#RUN apt-get install nodejs -y
#RUN npm install npm@7.11.1 -g
#RUN command -v node
#RUN command -v npm

RUN useradd -m developer -u 1000
RUN usermod -a -G developer www-data
##VOLUME ["/home/developer", "/application"]
WORKDIR /application/skunexus-core

#RUN rm -rf /var/lib/apt/lists/*
#RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
#    echo "pl_PL.UTF-8 UTF-8" >> /etc/locale.gen && \
#    locale-gen

RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" >> /etc/apt/sources.list.d/yarn.list && \
   wget -nv -O -  https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
   apt-get update && \
   apt-get -yq install yarn

RUN yarn install --frozen-lockfile --production && yarn cache clean

#RUN chmod -R 777 /dev/shm/.

EXPOSE 9000

CMD ["php-fpm"]

