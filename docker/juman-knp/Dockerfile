FROM ruby:latest

WORKDIR /root

ARG JUMANPP_VERSION
ARG JUMAN_VERSION
ARG KNP_VERSION

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update --fix-missing &&\
    apt-get install -y --fix-missing apt-utils dialog
RUN apt-get upgrade -y --fix-missing &&\
    apt-get install -y --fix-missing libboost-dev &&\
    apt-get install -y --fix-missing google-perftools &&\
    apt-get install -y --fix-missing libgoogle-perftools-dev
# python container had already
RUN apt-get install -y --fix-missing gcc &&\
    apt-get install -y --fix-missing g++ &&\
    apt-get install -y --fix-missing make &&\
    apt-get install -y --fix-missing wget
# to decompress *.tar.bz2
RUN apt-get install -y --fix-missing bzip2

# install JUMANPP
RUN wget http://lotus.kuee.kyoto-u.ac.jp/nl-resource/jumanpp/jumanpp-${JUMANPP_VERSION}.tar.xz -O /tmp/jumanpp.tar.xz &&\
    tar xJvf /tmp/jumanpp.tar.xz -C /tmp &&\
    cd /tmp/jumanpp-${JUMANPP_VERSION} &&\
    ./configure --prefix=/usr/local/ && make && make install &&\
    rm -rf /var/cache/apk/*

# install JUMAN
RUN wget http://nlp.ist.i.kyoto-u.ac.jp/nl-resource/juman/juman-${JUMAN_VERSION}.tar.bz2 -O /tmp/juman.tar.bz2 &&\ 
    tar xf /tmp/juman.tar.bz2 -C /tmp &&\
    cd /tmp/juman-${JUMAN_VERSION} &&\ 
    ./configure --prefix=/usr/local/ && make && make install &&\
    rm -rf /tmp/juman-${JUMAN_VERSION} &&\
    rm -rf /var/cache/apk/* &&\
    apt-get update && apt-get install -y --fix-missing libjuman4

# install KNP
RUN apt-get install -y --fix-missing zlib1g-dev &&\
    wget http://nlp.ist.i.kyoto-u.ac.jp/nl-resource/knp/knp-${KNP_VERSION}.tar.bz2 -O /tmp/knp.tar.bz2 &&\
    tar xf /tmp/knp.tar.bz2 -C /tmp &&\
    cd /tmp/knp-${KNP_VERSION} / &&\
    ./configure --prefix=/usr/local/ --with-juman-prefix=/usr/local/ && make && make install &&\
    rm -rf /tmp/knp-${KNP_VERSION} &&\
    rm -rf /var/cache/apk/*

# to use Japanese
RUN apt-get install -y -f locales
RUN locale-gen ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LC_CTYPE ja_JP.UTF-8
RUN localedef -f UTF-8 -i ja_JP ja_JP.utf8

# clean up all temporary files 
RUN apt-get clean &&\
    apt-get autoclean -y &&\
    apt-get autoremove -y &&\
    apt-get clean &&\
    rm -rf /var/tmp/* &&\
    rm -rf /var/lib/apt/lists/* &&\    
    rm -f /etc/ssh/ssh_host_*
