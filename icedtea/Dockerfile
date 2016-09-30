FROM ubuntu:16.04
MAINTAINER dominik.infuehr@gmail.com

RUN apt-get update \
    && apt-get install -y \
        mercurial \

RUN mkdir -p /code/tools
WORKDIR /code

RUN mkdir build \
    && cd build \
    && ../cacao-staging/configure --enable-disassembler \
    && make \
    && make check

RUN hg clone http://icedtea.classpath.org/hg/release/icedtea7-2.6/ \
    && hg update -r icedtea-2.6.4

RUN cd tools \
    && sh autogen.sh \
    && ./configure --disable-bootstrap --enable-cacao
