FROM php:7.2-fpm
RUN cp $(PWD)/ca
RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini && \
	sed -i -e "s|;session.save_path = "/tmp"|session.save_path = "/tmp" |" /usr/local/etc/php/php.ini && \
	sed -i -e "s|;extension=php_openssl.dll|extension=php_openssl.dll |" /usr/local/etc/php/php.ini && \
	sed -i -e "s|max_execution_time = 30|max_execution_time = 3000 |" /usr/local/etc/php/php.ini && \
	sed -i -e "s|upload_max_filesize = 2M|upload_max_filesize = 2000M |" /usr/local/etc/php/php.ini && \
	sed -i -e "s|memory_limit = 128M|memory_limit = 512M |" /usr/local/etc/php/php.ini && \
	sed -i -e "s|;curl.cainfo =|curl.cainfo = /usr/loccal/etc/cacert.pem |" /usr/local/etc/php/php.ini && \
        docker-php-ext-install pdo_mysql && \
	pecl install mailparse-3.0.2  && \
	pecl install xdebug-2.6.1 && \
	docker-php-ext-enable mailparse xdebug

RUN ["apt-get", "update"]
RUN ["apt-get", "install", "-y", "libonig-dev", "libfreetype6-dev", "libjpeg-dev", "libpng-dev", "libcurl4-openssl-dev", "libzip-dev", "libicu-dev", "libldap-dev", "imagemagick", "unzip", "pkg-config", "patch"]

RUN ["pecl", "install", "APCu"]
RUN ["docker-php-ext-enable", "apcu.so"]
RUN ["docker-php-ext-configure", "gd", "--with-freetype-dir=/usr/include/", "--with-jpeg-dir=/usr/include/"]
RUN ["docker-php-ext-install", "-j2","curl", "mbstring", "pdo_mysql", "zip", "exif", "intl", "fileinfo", "ldap", "mysqli", "gd"]

COPY ./xdebug.ini $PHP_INI_DIR/conf.d/
