FROM php:5.6-apache

RUN apt-get update \
    && apt-get -y install vim wget lsb-release libicu-dev mysql-client \
    && docker-php-ext-install pdo pdo_mysql mysqli intl
# RUN ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load
RUN ln -s /etc/apache2/mods-available/vhost_alias.load /etc/apache2/mods-enabled
# RUN echo ServerName $HOSTNAME > /etc/apache2/conf-available/fqdn.conf && a2enconf fqdn
# RUN a2enmod headers
# RUN a2enmod include

COPY ./base.conf /etc/apache2/sites-available/base.conf
RUN a2ensite base.conf

COPY ./php.ini /usr/local/etc/php/