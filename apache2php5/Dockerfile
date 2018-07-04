FROM ubuntu:18.04

# Environment variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_MAJOR 2
ENV APACHE_VERSION 2.4
ENV APACHE_MINOR 2.4.33
ENV PHP_MAJOR 5
ENV PHP_VERSION 5.6

# Install apache, PHP, and supplimentary programs. openssh-server, curl, and lynx-cur are for debugging the container.
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get -y install apache${APACHE_MAJOR} \
            php${PHP_VERSION} \
            php${PHP_VERSION}-dev \
            libapache${APACHE_MAJOR}-mod-php${PHP_VERSION} \
            php${PHP_VERSION}-curl \
            php${PHP_VERSION}-gd \
            php${PHP_VERSION}-gettext \
            php${PHP_VERSION}-iconv \
            php${PHP_VERSION}-imap \
            php${PHP_VERSION}-json \
            php${PHP_VERSION}-mbstring \
            php${PHP_VERSION}-mcrypt \
            php${PHP_VERSION}-mysql \
            php${PHP_VERSION}-pdo \
            php${PHP_VERSION}-pgsql \
            php${PHP_VERSION}-xdebug \
            php${PHP_VERSION}-xml

# Set timezone
#RUN timedatectl set-timezone Europe/Rome

# Replace existing default Apache configuration
ADD apache_default.conf /etc/apache${APACHE_MAJOR}/sites-enabled/000-default.conf

# Enable apache mods.
RUN a2enmod php${PHP_VERSION}
RUN a2enmod proxy
RUN a2enmod proxy_http
RUN a2enmod rewrite

# Update the PHP.ini file, enable <? ?> tags and quieten logging.
RUN sed -i "s/short_open_tag = Off/short_open_tag = On/" /etc/php/${PHP_VERSION}/apache${APACHE_MAJOR}/php.ini
RUN sed -i "s/error_reporting = .*$/error_reporting = E_ALL | E_ERROR | E_WARNING | E_PARSE/" /etc/php/${PHP_VERSION}/apache${APACHE_MAJOR}/php.ini
RUN sed -i "s/display_errors = .*$/display_errors = On/" /etc/php/${PHP_VERSION}/apache${APACHE_MAJOR}/php.ini

# Add proxy enabled apache configuration with the config we created to enable communication with Geoserver
# ADD apache_gs_proxy.conf /etc/apache${APACHE_MAJOR}/sites-available/operandum-gs.conf

# Enable Geoserver proxy configuration
# RUN a2ensite operandum-gs

# Delete default document root folder
RUN rm -f /var/www/html

# Expose apache.
EXPOSE 80

# Copy this repo into place.
# ADD www /var/www # Commented since when mounting the www directory host content of www directory overwrites the contents of container's folder

# By default start up apache in the foreground, override with /bin/bash for interactive.
CMD /usr/sbin/apache2ctl -D FOREGROUND

