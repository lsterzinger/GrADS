
FROM ubuntu:22.04 

WORKDIR /root

RUN apt update  \
    && apt install -y git build-essential wget cmake 

ARG githubpath=https://github.com/lsterzinger/GrADS.git
RUN git clone $githubpath 

WORKDIR /root/GrADS

RUN mkdir supplibs
ENV GRADSROOT=/root/GrADS
ENV SUPPLIBS=/root/GrADS/supplibs
RUN mkdir $SUPPLIBS/tarfiles
RUN mkdir $SUPPLIBS/src

# Build Jasper manually
RUN cd $SUPPLIBS/tarfiles/ && wget https://github.com/jasper-software/jasper/releases/download/version-4.2.3/jasper-4.2.3.tar.gz \
    && tar xzvf jasper-4.2.3.tar.gz -C $SUPPLIBS/src && cd  $SUPPLIBS/src/jasper-4.2.3 \
    && cmake -H./ -B$SUPPLIBS -DCMAKE_INSTALL_PREFIX=../  \
    && cmake --build  $SUPPLIBS --target install

# Install dependencies
RUN apt install -y libreadline-dev libncurses-dev zlib1g-dev libpng-dev libjpeg-dev \
    libgd-dev libcairo2-dev libnetcdf-dev=1:4.8.1-1 libhdf5-dev

RUN cd $GRADSROOT && PKG_CONFIG=$(which pkg-config) ./configure --prefix=/usr/local
RUN make && make install 

RUN mkdir  /usr/local/lib/grads && echo "gxdisplay  Cairo  /usr/local/lib/libgxdCairo.so\ngxprint    Cairo  /usr/local/lib/libgxpCairo.so" > /usr/local/lib/grads/udpt
ARG DISPLAY=:0.0