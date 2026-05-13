FROM php:8.2-fpm

# Set Environment Variables
ENV DEBIAN_FRONTEND noninteractive

RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
            git \
            zip \
            unzip \
            curl \
            wget \
            lsb-release \
            xdg-utils \
            ca-certificates \
            libmemcached-dev \
            libz-dev \
            libpq-dev \
            libssl-dev \
            libmcrypt-dev \
            libonig-dev \
            zlib1g-dev \
            libicu-dev g++ \
            libzip-dev \
            libmagickwand-dev \
            libjpeg-dev \
            libpng-dev \
            libfreetype6-dev \
            libwebp-dev \
            libxpm-dev \
            libgmp-dev \
            fonts-liberation \
            libasound2 \
            libatk-bridge2.0-0 \
            libatk1.0-0 \
            libc6 \
            libcairo2 \
            libcups2 \
            libdbus-1-3 \
            libexpat1 \
            libfontconfig1 \
            libgbm1 \
            libgcc1 \
            libglib2.0-0 \
            libgtk-3-0 \
            libnspr4 \
            libnss3 \
            libpango-1.0-0 \
            libpangocairo-1.0-0 \
            libstdc++6 \
            libx11-6 \
            libx11-xcb1 \
            libxcb1 \
            libxcomposite1 \
            libxcursor1 \
            libxdamage1 \
            libxext6 \
            libxfixes3 \
            libxi6 \
            libxrandr2 \
            libxrender1 \
            libxss1 \
            libxtst6; \


    docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_pgsql \
    && docker-php-ext-install sockets exif \


    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \


    && docker-php-ext-configure gmp \
    && docker-php-ext-install gmp \


    && rm -rf /var/lib/apt/lists/*



RUN set -eux; \
    docker-php-source extract; \
    curl -fsSL https://pecl.php.net/get/redis | tar -xz; \
    REDIS_DIR=$(ls -d redis-* | head -n 1); \
    mv "$REDIS_DIR" /usr/src/php/ext/redis; \
    docker-php-ext-install redis; \
    docker-php-source delete




RUN set -eux; \
    docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg \
        --with-webp \
        --with-xpm \
    && docker-php-ext-install -j$(nproc) gd \
    && php -r "var_dump(gd_info());"


RUN apt-get update && apt-get install -y \
    npm

ENV NVM_DIR=/usr/local/nvm
RUN mkdir -pv "$NVM_DIR" && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash && \
    bash -c "export NVM_DIR=$NVM_DIR && [ -s \"$NVM_DIR/nvm.sh\" ] && \. \"$NVM_DIR/nvm.sh\" && nvm install 18 && nvm use 18"

RUN bash -c "source /usr/local/nvm/nvm.sh && nvm use 18"

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer


ARG user=www-data
ARG uid=1000


RUN mkdir -p /home/$user/.composer && \
 chown -R $user:$user /home/$user && \
 chown -R $user:$user /var/www

WORKDIR /app


USER $user

EXPOSE 9000

COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
