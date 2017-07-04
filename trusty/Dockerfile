FROM ubuntu:trusty

RUN apt-get update && apt-get upgrade -y && apt-get install -y build-essential checkinstall wget ruby1.9 && apt-get build-dep ghostscript -y
RUN wget https://github.com/ArtifexSoftware/ghostpdl-downloads/releases/download/gs921/ghostscript-9.21.tar.gz
RUN gem install fpm
RUN mkdir /out
RUN tar -xvf ghostscript-9.21.tar.gz
RUN ln -s $(find ghostscript-* -type d | head -n 1) /ghostscript
WORKDIR /ghostscript

ADD patches/1.patch /ghostscript
ADD patches/2.patch /ghostscript

RUN patch -p1 < 1.patch
RUN patch -p1 < 2.patch

RUN ./configure
RUN make clean
RUN make
RUN mkdir /tmp/out
RUN make install DESTDIR=/tmp/out

CMD ls -al /tmp/out

CMD fpm -s dir -t deb -n ghostscript -v 9.21-trusty-talis2 -C /tmp/out -p ghostscript_VERSION_ARCH.deb \
    -d "libcupsimage2 (>=0)" \
    -d "libpaper1 (>=0)" \
    --deb-shlibs \
    usr/local && cp *.deb /out