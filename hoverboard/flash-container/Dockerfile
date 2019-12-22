FROM alpine:latest
WORKDIR /tmp

RUN apk add stlink --update-cache
RUN apk --no-cache add --virtual runtime-dependencies \
      libusb \
      libftdi1 &&\
    apk --no-cache add --virtual build-dependencies \
      git \
      build-base \
      libusb-dev \
      libftdi1-dev \
      automake \
      autoconf \
      libtool &&\
    git clone --depth 1 git://git.code.sf.net/p/openocd/code openocd &&\
    cd openocd &&\
    ./bootstrap &&\
    ./configure &&\
    make &&\
    make install &&\
    apk del --purge build-dependencies &&\
    rm -rf /var/cache/apk/* &&\
    rm -rf /tmp/*
