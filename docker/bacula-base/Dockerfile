FROM debian:bullseye

MAINTAINER Leandro Ramos <leandroramos@disroot.org>

ENV DEBIAN_VERSION bullseye
ENV BACULA_VERSION 11.0.5

# get your key on: https://www.bacula.org/bacula-binary-package-download/
ENV BACULA_KEY 61cf096a918f8

ENV EMAIL leandroramos@disroot.org

## GPG key import
RUN apt-get update && apt-get upgrade -y && apt-get install wget gnupg2 -y && wget -qO- https://www.bacula.org/downloads/Bacula-4096-Distribution-Verification-key.asc | apt-key add - && wget -qO - https://www.bacula.org/downloads/baculum/baculum.pub | apt-key add -

# Creating bacula-community.list and baculum.list repositories
COPY bacula-community.list /etc/apt/sources.list.d/bacula-community.list
COPY baculum.list /etc/apt/sources.list.d/baculum.list

RUN sed -i s/BACULA_VERSION/$(echo $BACULA_VERSION)/g /etc/apt/sources.list.d/bacula-community.list \
  && sed -i s/BACULA_KEY/$(echo $BACULA_KEY)/g /etc/apt/sources.list.d/bacula-community.list \
  && sed -i s/DEBIAN_VERSION/$(echo $DEBIAN_VERSION)/g /etc/apt/sources.list.d/bacula-community.list

# Installing packages
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install php-bcmath php*-mbstring baculum-api baculum-api-apache2 baculum-common bacula-console baculum-web baculum-web-apache2 postgresql bacula-postgresql bacula-aligned -y && apt clean

# Creating Baculum sudoers file
COPY baculum-sudoers /etc/sudoers.d/baculum

# Configuring user permissions
RUN usermod -aG bacula www-data && chown -R www-data:bacula /opt/bacula/working /opt/bacula/etc && chmod -R g+rwx /opt/bacula/working /opt/bacula/etc && a2enmod rewrite && a2ensite baculum-web baculum-api && service apache2 restart

EXPOSE 9095 9096
