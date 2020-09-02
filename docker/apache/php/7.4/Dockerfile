ARG php_version

FROM php:${php_version}

# Update
RUN apt-get -y update --fix-missing && \
    apt-get upgrade -y && \
    apt-get --no-install-recommends install -y apt-utils && \
    rm -rf /var/lib/apt/lists/*

# Install useful tools and install important libaries
RUN apt-get -y update && \
    apt-get -y --no-install-recommends install nano wget \
    dialog \
    libsqlite3-dev \
    libsqlite3-0 && \
    apt-get -y --no-install-recommends install default-mysql-client \
    zlib1g-dev \
    libzip-dev \
    libicu-dev && \
    apt-get -y --no-install-recommends install --fix-missing apt-utils \
    build-essential \
    git \
    curl \
    libonig-dev && \ 
    apt-get -y --no-install-recommends install --fix-missing libcurl4 \
    libcurl4-openssl-dev \
    zip \
    unzip \
    vim \
    libxml2-dev \
    openssl && \
    rm -rf /var/lib/apt/lists/* 

# Install xsl PHP7 Extension #
RUN apt-get update && apt-get install -y \
		libxslt-dev \
	&& docker-php-ext-install xsl

# Install gd PHP7 Extension #
RUN apt-get update && apt-get install -y \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libpng-dev \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) gd

# Install ldap PHP7 Extension #
RUN \
    apt-get update && \
    apt-get install libldap2-dev -y && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap

# Other PHP7 Extensions
RUN docker-php-ext-install pdo_mysql && \
    docker-php-ext-install pdo_sqlite && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install tokenizer && \
    docker-php-ext-install json && \
    docker-php-ext-install zip && \
    docker-php-ext-install -j$(nproc) intl && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install gettext && \
    docker-php-ext-install xml && \
    docker-php-ext-install exif


# Install and enable xdebug.
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Install composer #
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install and enable browscap
RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini 
RUN mkdir -p /usr/local/etc/php/extra/ \
    && curl "http://browscap.org/stream?q=Full_PHP_BrowsCapINI" -o /usr/local/etc/php/extra/full_php_browscap.ini \
    && mv /usr/local/etc/php/extra/full_php_browscap.ini /usr/local/etc/php/extra/browscap.ini

# Add correct rights for www folder.
RUN chmod -R 777 /var/www \
    && chown -R www-data:www-data /var/www \
    && usermod -u 1000 www-data \
    && chsh -s /bin/bash www-data
RUN cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/vhost.conf
RUN cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/abcis.sf5.conf


RUN a2enmod rewrite
RUN a2enmod headers
RUN a2ensite vhost.conf
RUN a2ensite abcis.sf5.conf

RUN ln -sf /dev/stdout /var/log/apache2/access.log \
    && ln -sf /dev/stderr /var/log/apache2/error.log2

RUN sed -i -e "s/^ *memory_limit.*/memory_limit = -1/g" -e "s/^ *max_execution_time.*/max_execution_time = 0/g" /usr/local/etc/php/php.ini

# YARN -----------------------------------------------------------------------------------------------------------------
# Install Yarn
RUN wget -q https://nodejs.org/dist/v12.15.0/node-v12.15.0-linux-x64.tar.xz && \
  tar -C /usr/local --strip-components 1 -xJf node-v12.15.0-linux-x64.tar.xz && \
  curl -o- -L https://yarnpkg.com/install.sh | bash -s -- --version 1.22.4 && \
  ln -sf $HOME/.yarn/bin/yarn /usr/local/bin/yarn && \
  ln -sf $HOME/.yarn/bin/yarnpkg /usr/local/bin/yarnpkg
# end YARN -------------------------------------------------------------------------------------------------------------