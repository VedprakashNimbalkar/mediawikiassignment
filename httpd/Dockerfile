FROM centos:centos8

MAINTAINER Vedprakash Nimbalkar <vedprakash.nimbalkar@gmail.com>

# Description
# This image provides an Apache 2.4 + PHP 7.4 environment for running PHP applications.
# Exposed ports:
# * 8080 - alternative port for http
EXPOSE 80
EXPOSE 443

ENV PHP_VERSION=7.4 \
    PHP_VER_SHORT=74 \
    NAME=php


# Install Apache httpd and PHP
# Install basic package
RUN dnf -y update && dnf clean all
RUN dnf module reset php
RUN dnf -y module enable php:7.4
RUN dnf -y install httpd php php-mysqlnd php-gd php-xml php-mbstring php-json


ENV PHP_CONTAINER_SCRIPTS_PATH=/usr/share/container-scripts/php/ \
    APP_DATA=${APP_ROOT}/src \
    PHP_DEFAULT_INCLUDE_PATH=/usr/share/pear \
    PHP_SYSCONF_PATH=/etc \
    PHP_HTTPD_CONF_FILE=php.conf \
    HTTPD_CONFIGURATION_PATH=${APP_ROOT}/etc/conf.d \
    HTTPD_MAIN_CONF_PATH=/etc/httpd/conf \
    HTTPD_MAIN_CONF_D_PATH=/etc/httpd/conf.d \
    HTTPD_MODULES_CONF_D_PATH=/etc/httpd/conf.modules.d \
    HTTPD_VAR_RUN=/var/run/httpd \
    HTTPD_DATA_PATH=/var/www \
    HTTPD_DATA_ORIG_PATH=/var/www \
    HTTPD_VAR_PATH=/var

RUN systemctl enable httpd

# smoke test
RUN httpd -v

COPY httpd-foreground /usr/local/bin/
COPY mpm.conf /etc/httpd/conf.modules.d/00-mpm.conf

COPY httpd.conf /etc/httpd/conf/httpd.conf
# Add mediawiki app data
RUN mkdir -p /var/www/mediawiki
# RUN mkdir -p /var/run/php/
COPY mediawiki /var/www/mediawiki
#COPY 000-mpm.conf /etc/httpd/conf.modules.d/000-mpm.conf
RUN chown apache:apache /var/www/mediawiki
RUN chown -Rv apache:apache /var/www/mediawiki
RUN chown apache:apache  /etc/php-fpm.d/www.conf

# Apache gets grumpy about PID files pre-existing
CMD ["httpd-foreground"]
