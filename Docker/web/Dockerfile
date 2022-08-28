#Dockerfile
FROM php:8.1-apache-bullseye

# create php.ini
RUN cp "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
# enable zend opcache
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-enable pdo_mysql
RUN apt-get update
RUN apt install -y libicu-dev
RUN apt install curl
RUN docker-php-ext-install intl
RUN docker-php-ext-enable intl
RUN a2enmod rewrite
# allow .htaccess override
RUN sed -i '/<Directory \/var\/www\/>/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf
# set document root for symfony
RUN sed -i '/<VirtualHost \*:80>/,/<\/VirtualHost>/ s/DocumentRoot \/var\/www\/html/DocumentRoot \/var\/www\/html\/public/' /etc/apache2/sites-enabled/000-default.conf
# install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
RUN apt install -y zip
RUN apt install -y npm
RUN npm install
# install symfony
RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' --output symfony.setup.deb.sh
RUN bash symfony.setup.deb.sh
RUN apt install -y symfony-cli

EXPOSE 80/tcp