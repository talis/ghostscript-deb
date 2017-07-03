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

#\CMD fpm -s dir -t deb -n imagemagick -v 6.9.7-10-trusty-talis -C /tmp/out -p imagemagick_VERSION_ARCH.deb \
#    -d "libdjvulibre-dev (>=0)" \
#    -d "libwmf-dev (>=0)" \
#    -d "libopenexr-dev (>=0)" \
#    -d "libpango1.0-dev (>=0)" \
#    -d "libc6 (>=0)" \
#    -d "libmagickcore5 (>=0)" \
#    -d "libmagickwand5 (>=0)" \
#    -d "hicolor-icon-theme (>=0)" \
#    --deb-shlibs \
#    usr/local && cp *.deb /out