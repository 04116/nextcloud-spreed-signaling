# Modified from https://gitlab.com/powerpaul17/nc_talk_backend/-/blob/dcbb918d8716dad1eb72a889d1e6aa1e3a543641/docker/janus/Dockerfile
FROM alpine:3.11

RUN apk add --no-cache curl autoconf automake libtool pkgconf build-base \
  glib-dev libconfig-dev libnice-dev jansson-dev openssl-dev zlib libsrtp-dev \
  gengetopt libwebsockets-dev git

# usrsctp
ARG USRSCTP_VERSION=2.2.0
RUN cd /tmp && \
    curl -L https://github.com/cisco/libsrtp/archive/v$USRSCTP_VERSION.tar.gz | tar -xz && \
    cd libsrtp-$USRSCTP_VERSION && \
    ./configure --prefix=/usr --enable-openssl && \
    make && make install

# JANUS
ARG JANUS_VERSION=0.9.2
RUN mkdir -p /usr/src/janus && \
    cd /usr/src/janus && \
    curl -L https://github.com/meetecho/janus-gateway/archive/v$JANUS_VERSION.tar.gz | tar -xz && \
    cd /usr/src/janus/janus-gateway-$JANUS_VERSION && \
    ./autogen.sh && \
    ./configure --disable-rabbitmq --disable-mqtt --disable-boringssl && \
    make && \
    make install && \
    make configs

WORKDIR /usr/src/janus/janus-gateway-$JANUS_VERSION

CMD [ "janus" ]
