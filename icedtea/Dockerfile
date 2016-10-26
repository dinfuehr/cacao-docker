FROM ubuntu:16.04
MAINTAINER dominik.infuehr@gmail.com

RUN apt-get update \
    && apt-get install -y software-properties-common \
    && add-apt-repository ppa:openjdk-r/ppa \
    && apt-get update \
    && apt-get install -y \
        ant \
        autoconf \
        automake \
        gawk \
        build-essential \
        cpio \
        fastjar \
        mercurial \
        openjdk-7-jdk \
        wget \
        zip \
        libkrb5-dev \
        zlib1g-dev \
        libjpeg-dev \
        libgif-dev \
        pkg-config \
        libgtk2.0-dev \
        libattr1-dev \
        libpcsclite-dev \
        libsctp-dev \
        libasound2-dev \
        librhino-java \
        liblcms2-dev \
        libcups2-dev \
        libxtst-dev \
        libtool \
        gettext \
        binutils-dev \
        autoconf \
        automake \
        libiberty-dev \
    && ln -s /usr/lib/jvm/java-7-openjdk-amd64 /usr/lib/jvm/java-7-openjdk

RUN mkdir -p /code/tools
WORKDIR /code

RUN cd tools \
    && wget http://search.maven.org/remotecontent\?filepath\=junit/junit/4.10/junit-4.10.jar \
          -O junit-4.10.jar \
    && mkdir -p /usr/share/java \
    && cp junit-4.10.jar /usr/share/java/junit4.jar

RUN cd tools \
    && wget http://icedtea.classpath.org/download/source/icedtea-2.6.4.tar.gz \
    && tar xf icedtea-2.6.4.tar.gz

RUN cd tools/icedtea-2.6.4 \
    && ./configure --disable-bootstrap --enable-cacao \
    && make -j4

RUN cd tools/icedtea-2.6.4/cacao \
    && hg clone https://bitbucket.org/cacaovm/cacao-staging \
    && ln -sfn cacao-staging cacao \
    && cd cacao \
    && sh autogen.sh

RUN cd tools \
    && wget http://ufpr.dl.sourceforge.net/project/jasmin/jasmin/jasmin-2.4/jasmin-2.4.zip \
    && unzip jasmin-2.4.zip \
    && cp jasmin-2.4/jasmin.jar /usr/share/java/jasmin-sable.jar

RUN cd tools/icedtea-2.6.4/cacao/cacao \
    && ./configure --host= --build= --target= --prefix=$PWD/../install \
         --with-java-runtime-library=openjdk7 \
         --with-java-runtime-library-prefix=$PWD/../../openjdk \
         --with-java-runtime-library-classes=$PWD/../../bootstrap/jdk1.6.0/jre/lib/rt.jar \
         --enable-jre-layout --enable-maintainer-mode \
         --disable-optimizations --enable-disassembler

RUN cd tools/icedtea-2.6.4/cacao/cacao \
    && make \
    && for l in `find ../../openjdk.build -name libjvm.so`; do cp -fv ./src/cacao/.libs/libjvm.so $l; done

RUN cd tools/icedtea-2.6.4 \
    && ./openjdk.build/j2sdk-image/bin/java -version

