<<<<<<< HEAD
FROM barais/ffmpeg
	
	MAINTAINER Olivier Barais: 0.1
	
	WORKDIR /root/ffmpeg_sources
	ENV CFLAGS="-O1 -Wall"
	ENV CXXFLAGS="-O1 -Wall"
	RUN tar xzvf yasm-1.3.0.tar.gz
	WORKDIR /root/ffmpeg_sources/yasm-1.3.0
	RUN sed -i 's/-O2/-O1/g' configure
	RUN sed -i 's/-O1/-O1/g' configure
	RUN sed -i 's/-O3/-O1/g' configure
	RUN ./configure --prefix="/root/ffmpeg_build" --bindir="/root/bin"
	RUN make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN tar xjvf last_x264.tar.bz2
	WORKDIR /root/ffmpeg_sources/x264-snapshot-20141218-2245
	RUN sed -i 's/-O2/-O1/g' configure
	RUN sed -i 's/-O1/-O1/g' configure
	RUN sed -i 's/-O3/-O1/g' configure
	RUN PATH="/root/bin:$PATH" ./configure --prefix="/root/ffmpeg_build" --bindir="/root/bin" --enable-static
	RUN PATH="/root/bin:$PATH" make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN unzip fdk-aac.zip
	WORKDIR /root/ffmpeg_sources/mstorsjo-fdk-aac-83ac4a9
	RUN autoreconf -fiv
	RUN sed -i 's/-O2/-O1/g' configure
	RUN sed -i 's/-O1/-O1/g' configure
	RUN sed -i 's/-O3/-O1/g' configure
	RUN ./configure --prefix="/root/ffmpeg_build" --disable-shared
	RUN make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN tar xzvf lame-3.99.5.tar.gz
	WORKDIR /root/ffmpeg_sources/lame-3.99.5
	RUN sed -i 's/-O2/-O1/g' configure
	RUN sed -i 's/-O1/-O1/g' configure
	RUN sed -i 's/-O3/-O1/g' configure
	RUN ./configure --prefix="/root/ffmpeg_build" --enable-nasm --disable-shared
	RUN make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN tar xzvf opus-1.1.tar.gz
	WORKDIR /root/ffmpeg_sources/opus-1.1
	RUN sed -i 's/-O2/-O1/g' configure
	RUN sed -i 's/-O1/-O1/g' configure
	RUN sed -i 's/-O3/-O1/g' configure
	RUN ./configure --prefix="/root/ffmpeg_build" --disable-shared
	RUN make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN tar xjvf libvpx-v1.3.0.tar.bz2
	WORKDIR /root/ffmpeg_sources/libvpx-v1.3.0
	RUN sed -i 's/-O2/-O1/g' configure
	RUN sed -i 's/-O1/-O1/g' configure
	RUN sed -i 's/-O3/-O1/g' configure
	RUN PATH="/root/bin:$PATH" ./configure --prefix="/root/ffmpeg_build" --disable-examples
	RUN PATH="/root/bin:$PATH" make
	RUN make install
	RUN make clean
	WORKDIR /root/ffmpeg_sources
	RUN tar xjvf ffmpeg-snapshot.tar.bz2
	WORKDIR /root/ffmpeg_sources/ffmpeg
	RUN sed -i 's/-O2/-O1/g' configure
	RUN sed -i 's/-O1/-O1/g' configure
	RUN sed -i 's/-O3/-O1/g' configure
	RUN PATH="/root/bin:$PATH" PKG_CONFIG_PATH="/root/ffmpeg_build/lib/pkgconfig" ./configure \
	--prefix="/root/ffmpeg_build" \
	--extra-cflags="-I/root/ffmpeg_build/include" \
	--extra-ldflags="-L/root/ffmpeg_build/lib" \
	--bindir="/root/bin" \
	--enable-gpl \
	--enable-libass \
	--enable-libfdk-aac \
	--enable-libfreetype \
	--enable-libmp3lame \
	--enable-libopus \
	--enable-libtheora \
	--enable-libvorbis \
	--enable-libvpx \
	--enable-libx264 \
	--enable-nonfree
	RUN PATH="/root/bin:$PATH" make
	RUN make install
	RUN make distclean
	RUN hash -r
=======

FROM ubuntu:14.04

MAINTAINER Nelson Darkwah Oppong "ndo@felixnelson.de"

# from: https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu

RUN apt-get update && \
    apt-get -y install wget git-core autoconf automake build-essential libass-dev libgpac-dev \
      libsdl1.2-dev libtheora-dev libtool libva-dev libvdpau-dev libvorbis-dev libx11-dev \
      libxext-dev libxfixes-dev pkg-config texi2html zlib1g-dev unzip libass-dev libmp3lame-dev librtmp-dev

RUN mkdir ~/ffmpeg_sources

# yasm
RUN cd ~/ffmpeg_sources && \
    wget --quiet "http://www.tortall.net/projects/yasm/releases/yasm-1.2.0.tar.gz" && \
    tar xzvf yasm-1.2.0.tar.gz && \
    cd yasm-1.2.0 && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
    ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
    make && \
    make install && \
    make distclean

# libx264
RUN cd ~/ffmpeg_sources && \
    wget --quiet "http://download.videolan.org/pub/x264/snapshots/last_x264.tar.bz2" && \
    tar xjvf last_x264.tar.bz2 && \
    cd x264-snapshot* && \
    PATH="$PATH:$HOME/bin" && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --enable-static --disable-opencl && \
    PATH="$PATH:$HOME/bin" make && \
    make install && \
    make distclean

# libfdk-aac
RUN cd ~/ffmpeg_sources && \
    wget --quiet -O fdk-aac.zip "https://github.com/mstorsjo/fdk-aac/zipball/master"  && \
    unzip fdk-aac.zip && \
    cd mstorsjo-fdk-aac* && \
    autoreconf -fiv && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
    ./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
    make && \
    make install && \
    make distclean

# libopus
RUN cd ~/ffmpeg_sources && \
    wget --quiet "http://downloads.xiph.org/releases/opus/opus-1.1.tar.gz" && \
    tar xzvf opus-1.1.tar.gz && \
    cd opus-1.1 && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
    ./configure --prefix="$HOME/ffmpeg_build" --disable-shared && \
    make && \
    make install && \
    make distclean

# libvpx
RUN cd ~/ffmpeg_sources && \
    wget --quiet "http://webm.googlecode.com/files/libvpx-v1.3.0.tar.bz2" && \
    tar xjvf libvpx-v1.3.0.tar.bz2 && \
    cd libvpx-v1.3.0 && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
    ./configure --prefix="$HOME/ffmpeg_build" --disable-examples && \
    make && \
    make install && \
    make clean

# ffmpeg
RUN cd ~/ffmpeg_sources && \
    wget --quiet "http://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2" && \
    tar xjvf ffmpeg-snapshot.tar.bz2 && \
    cd ffmpeg && \
    PATH="$PATH:$HOME/bin" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
./configure \
      --prefix="$HOME/ffmpeg_build" \
      --extra-cflags="-I$HOME/ffmpeg_build/include" \
      --extra-ldflags="-L$HOME/ffmpeg_build/lib" \
      --bindir="$HOME/bin" \
      --enable-gpl \
      --enable-libass \
      --enable-libfdk-aac \
      --enable-libfreetype \
      --enable-libmp3lame \
      --enable-libopus \
      --enable-libtheora \
      --enable-libvorbis \
      --enable-libvpx \
      --enable-libx264 \
      --enable-librtmp \
      --enable-nonfree && \
    PATH="$PATH:$HOME/bin" make && \
    make install && \
    make distclean && \
    hash -r
>>>>>>> 39e00244d550fe58405e9eb279c2c88fe1db6e18
