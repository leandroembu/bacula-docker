FROM debian:bullseye

MAINTAINER Leandro Ramos <leandroramos@disroot.org>

ENV DEBIAN_VERSION bullseye
ENV BACULA_VERSION 11.0.5

# get your key on: https://www.bacula.org/bacula-binary-package-download/
ENV BACULA_KEY 61cf096a918f8

ENV EMAIL leandroramos@disroot.org

## GPG key import
RUN wget -O- https://www.bacula.org/downloads/Bacula-4096-Distribution-Verification-key.asc | gpg --dearmor | tee /usr/share/keyrings/bacula-community-archive-keyring.gpg

# Creating bacula-community.list repository
COPY bacula-community.list /etc/apt/sources.list.d/bacula-community.list

RUN sed -i s/BACULA_VERSION/$(echo $BACULA_VERSION)/g /etc/apt/sources.list.d/bacula-community.list \
  && sed -i s/BACULA_KEY/$(echo $BACULA_KEY)/g /etc/apt/sources.list.d/bacula-community.list \
  && sed -i s/DEBIAN_VERSION/$(echo $DEBIAN_VERSION)/g /etc/apt/sources.list.d/bacula-community.list

RUN DEBIAN_FRONTEND=noninteractive apt-get install postgresql bacula-postgresql bacula-aligned && apt clean
