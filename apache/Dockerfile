# Changes to original:
# 1- point to 'joomlagovbr' repository
# 2- copy custom php.ini settings

FROM php:5.6-apache
MAINTAINER Michael Babker <michael.babker@joomla.org> (@mbabker)

# Enable Apache Rewrite Module
RUN a2enmod rewrite

# Install PHP extensions
RUN apt-get update && apt-get install -y libpng12-dev libjpeg-dev zip unzip && rm -rf /var/lib/apt/lists/* \
	&& docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
	&& docker-php-ext-install gd
RUN docker-php-ext-install mysqli

VOLUME /var/www/html

# Define Joomla version and expected SHA1 signature
ENV JOOMLA_VERSION 3.4.3
ENV JOOMLA_SHA1 cd35ed61029d2ed0dc38cc70073944786bba7979

# Download package and extract to web volume
RUN curl -o joomla.zip -SL https://github.com/joomlagovbr/joomla-3.x/archive/dev.zip \
	&& mkdir /usr/src/joomla \
	&& unzip joomla.zip -d /tmp \
        && cp -R /tmp/joomla-3.x-dev/* /usr/src/joomla \
	&& rm joomla.zip \
	&& chown -R www-data:www-data /usr/src/joomla

# Copy init scripts and custom .htaccess
COPY docker-entrypoint.sh /entrypoint.sh
COPY makedb.php /makedb.php

# Copy php ini settings
COPY website-customizations.ini /usr/local/etc/php/conf.d/webite-customizations.ini

ENTRYPOINT ["/entrypoint.sh"]
CMD ["apache2-foreground"]
