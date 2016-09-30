FROM ubuntu:16.04
MAINTAINER dominik.infuehr@gmail.com

RUN apt-get update \
    && apt-get install -y \
        ant \
        autoconf \
        automake \
        build-essential \
        cpio \
        mercurial \
        wget \
        zip

RUN mkdir -p /code/tools
WORKDIR /code

RUN cd tools \
    && hg clone http://icedtea.classpath.org/hg/release/icedtea7-2.6/ \
    && cd icedtea7-2.6 \
    && hg update -r icedtea-2.6.4

RUN cd tools/icedtea7-2.6 \
    && sh autogen.sh \
    && ./configure --disable-bootstrap --enable-cacao
