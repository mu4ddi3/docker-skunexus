FROM php:7.4.16-fpm-buster

RUN apt-get update \
    && apt-get upgrade -y \

# Install persistent build dependencies
RUN set -eux; apt-get install -y --no-install-recommends \
    apt-utils \
    ca-certificates \
    curl \
    file \
    gettext \
    ghostscript \
    git \
    gnupg \
    libc6 \
    libcurl4 \
    libgmp10 \
    libsqlite3-0 \
    libzstd1 \
    vim \
    wget \
    zip \
    unzip \
    zsh \
    expect \
    locales

# Install ephemeral build dependencies
RUN set -ex; apt-get install -y --no-install-recommends \
    libcurl4-openssl-dev \
    libfreetype6-dev \
    libffi-dev \
    libicu-dev \
    libjpeg-dev \
    libmcrypt-dev \
    libmhash-dev \
    libonig-dev \
    libpng-dev \
    libxml2-dev \
    libxslt1-dev \
    libyaml-0-2 \
    libzip-dev

# Configure php extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg;
RUN docker-php-ext-configure bcmath --enable-bcmath;
RUN docker-php-ext-configure calendar --enable-calendar;
RUN docker-php-ext-configure curl --with-curl;
RUN docker-php-ext-configure ffi --with-ffi;
RUN docker-php-ext-configure ftp --enable-ftp;
RUN docker-php-ext-configure gettext --with-gettext;
RUN docker-php-ext-configure intl --enable-intl;
RUN docker-php-ext-configure pdo_mysql --with-pdo-mysql;
RUN docker-php-ext-configure mbstring --enable-mbstring;
RUN docker-php-ext-configure shmop --enable-shmop;
RUN docker-php-ext-configure soap --enable-soap;
RUN docker-php-ext-configure sockets --enable-sockets;
RUN docker-php-ext-configure sysvmsg --enable-sysvmsg;
RUN docker-php-ext-configure sysvsem --enable-sysvsem;
RUN docker-php-ext-configure sysvshm --enable-sysvshm;
RUN docker-php-ext-configure xsl --with-xsl;

# Install php extensions
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install calendar
RUN docker-php-ext-install ctype
RUN docker-php-ext-install curl
RUN docker-php-ext-install dom
RUN docker-php-ext-install exif
RUN docker-php-ext-install ffi
RUN docker-php-ext-install fileinfo
RUN docker-php-ext-install filter
RUN docker-php-ext-install gd
RUN docker-php-ext-install gettext
RUN docker-php-ext-install intl
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install opcache
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install shmop
RUN docker-php-ext-install soap
RUN docker-php-ext-install sockets
RUN docker-php-ext-install sysvmsg
RUN docker-php-ext-install sysvsem
RUN docker-php-ext-install sysvshm
RUN docker-php-ext-install xsl
RUN docker-php-ext-install zip

# Install and enable redis dependency via pecl
RUN pecl install igbinary \
    && docker-php-ext-enable igbinary

# Install and enable other php extensions via pecl
RUN pecl install apcu \
    && docker-php-ext-enable apcu
RUN pecl install mcrypt-1.0.5 \
    && docker-php-ext-enable mcrypt
RUN pecl install redis \
    && docker-php-ext-enable redis

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version
RUN composer global require "liuggio/fastest=^1.10"

# Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x -o nodesource_setup.sh
RUN bash nodesource_setup.sh
RUN apt-get install -y nodejs

# Yarn
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" >> /etc/apt/sources.list.d/yarn.list && \
   wget -nv -O -  https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
   apt-get update && \
   apt-get -yq install yarn
RUN yarn install --frozen-lockfile --production && yarn cache clean

# Install xdebug if needed -> use `--build-arg WITH_XDEBUG="true"` if needed or use value from override file
ARG WITH_XDEBUG="false"
RUN if [ $WITH_XDEBUG = "true" ] ; then \
        pecl install xdebug-3.1.6; \
        docker-php-ext-enable xdebug; \
        #echo 'name=value' > /usr/local/etc/php/conf.d/example.ini; \
        #echo 'name2=value2' >> /usr/local/etc/php/conf.d/example.ini; \
    fi ;

# Remove build dependencies
RUN apt purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false;
RUN rm -rf /var/lib/apt/lists/*;
RUN pecl update-channels;
RUN rm -rf /tmp/pear ~/.pearrc;

RUN useradd -m developer -u 1000
RUN usermod -a -G developer www-data

#USER developer

# Install oh-my-zsh
#ADD "https://www.random.org/cgi-bin/randbyte?nbytes=10&format=h" skipcache
#RUN wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh
#ADD "https://www.random.org/cgi-bin/randbyte?nbytes=10&format=h" skipcache
#RUN bash install.sh
#RUN cp -R /root/.oh-my-zsh /home/developer
#RUN su - developer -c "curl -vfL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -o /home/developer/ohmyzsh_setup.sh"
#RUN curl -vfL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -o ohmyzsh_setup.sh
#RUN bash ohmyzsh_setup.sh

#RUN if ! test -f "~/.zshrc"; then \
#      sed -i "s/ZSH_THEME=\"robbyrussell\"/ZSH_THEME=\"agnoster\"/gi" ~/.zshrc; \
#      sed -i "s/plugins=(git)/plugins(sudo web-search copypath copyfile copybuffer dirhistory history emoji encode64 zsh-syntax-highlighting zsh-autosuggestions)/gi" ~/.zshrc; \
#    fi ;

#RUN su - developer -c "wget -P /home/developer https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh"
#&& su developer -c "sh /home/developer/install.sh"
#RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/installs.sh)"

RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV NODE_OPTIONS=--openssl-legacy-provider

# Some old stuff
#COPY php-fpm-pool.conf /etc/php/7.4/pool.d/www.conf
#RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
#    echo "pl_PL.UTF-8 UTF-8" >> /etc/locale.gen && \
#    locale-gen

# Appending content to file example (notice >>)
#RUN { \
#    echo 'name=value'; \
#    echo 'name2=value2'; \
#} >> /usr/local/etc/php/conf.d/example.ini

# Set recommended opcache php.ini settings
#RUN { \
#    echo 'opcache.memory_consumption=128'; \
#    echo 'opcache.interned_strings_buffer=8'; \
#    echo 'opcache.max_accelerated_files=4000'; \
#    echo 'opcache.revalidate_freq=2'; \
#    echo 'opcache.fast_shutdown=1'; \
#} > /usr/local/etc/php/conf.d/opcache-recommended.ini

# Configure-error-logging
#RUN { \
#    echo 'error_reporting = E_ERROR | E_WARNING | E_PARSE | E_CORE_ERROR | E_CORE_WARNING | E_COMPILE_ERROR | E_COMPILE_WARNING | E_RECOVERABLE_ERROR'; \
#    echo 'display_errors = Off'; \
#    echo 'display_startup_errors = Off'; \
#    echo 'log_errors = On'; \
#    echo 'error_log = /dev/stderr'; \
#    echo 'log_errors_max_len = 1024'; \
#    echo 'ignore_repeated_errors = On'; \
#    echo 'ignore_repeated_source = Off'; \
#    echo 'html_errors = Off'; \
#} > /usr/local/etc/php/conf.d/error-logging.ini



WORKDIR /application
EXPOSE 9000
CMD ["php-fpm"]

