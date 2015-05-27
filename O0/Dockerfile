<<<<<<< HEAD
FROM barais/ffmpeg
	
	MAINTAINER Olivier Barais: 0.1
	
	WORKDIR /root/ffmpeg_sources
	ENV CFLAGS="-O0 -Wall"
	ENV CXXFLAGS="-O0 -Wall"
	RUN tar xzvf yasm-1.3.0.tar.gz
	WORKDIR /root/ffmpeg_sources/yasm-1.3.0
	RUN sed -i 's/-O2/-O0/g' configure
	RUN sed -i 's/-O1/-O0/g' configure
	RUN sed -i 's/-O3/-O0/g' configure
	RUN ./configure --prefix="/root/ffmpeg_build" --bindir="/root/bin"
	RUN make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN tar xjvf last_x264.tar.bz2
	WORKDIR /root/ffmpeg_sources/x264-snapshot-20141218-2245
	RUN sed -i 's/-O2/-O0/g' configure
	RUN sed -i 's/-O1/-O0/g' configure
	RUN sed -i 's/-O3/-O0/g' configure
	RUN PATH="/root/bin:$PATH" ./configure --prefix="/root/ffmpeg_build" --bindir="/root/bin" --enable-static
	RUN PATH="/root/bin:$PATH" make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN unzip fdk-aac.zip
	WORKDIR /root/ffmpeg_sources/mstorsjo-fdk-aac-83ac4a9
	RUN autoreconf -fiv
	RUN sed -i 's/-O2/-O0/g' configure
	RUN sed -i 's/-O1/-O0/g' configure
	RUN sed -i 's/-O3/-O0/g' configure
	RUN ./configure --prefix="/root/ffmpeg_build" --disable-shared
	RUN make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN tar xzvf lame-3.99.5.tar.gz
	WORKDIR /root/ffmpeg_sources/lame-3.99.5
	RUN sed -i 's/-O2/-O0/g' configure
	RUN sed -i 's/-O1/-O0/g' configure
	RUN sed -i 's/-O3/-O0/g' configure
	RUN ./configure --prefix="/root/ffmpeg_build" --enable-nasm --disable-shared
	RUN make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN tar xzvf opus-1.1.tar.gz
	WORKDIR /root/ffmpeg_sources/opus-1.1
	RUN sed -i 's/-O2/-O0/g' configure
	RUN sed -i 's/-O1/-O0/g' configure
	RUN sed -i 's/-O3/-O0/g' configure
	RUN ./configure --prefix="/root/ffmpeg_build" --disable-shared
	RUN make
	RUN make install
	RUN make distclean
	WORKDIR /root/ffmpeg_sources
	RUN tar xjvf libvpx-v1.3.0.tar.bz2
	WORKDIR /root/ffmpeg_sources/libvpx-v1.3.0
	RUN sed -i 's/-O2/-O0/g' configure
	RUN sed -i 's/-O1/-O0/g' configure
	RUN sed -i 's/-O3/-O0/g' configure
	RUN PATH="/root/bin:$PATH" ./configure --prefix="/root/ffmpeg_build" --disable-examples
	RUN PATH="/root/bin:$PATH" make
	RUN make install
	RUN make clean
	WORKDIR /root/ffmpeg_sources
	RUN tar xjvf ffmpeg-snapshot.tar.bz2
	WORKDIR /root/ffmpeg_sources/ffmpeg
	RUN sed -i 's/-O2/-O0/g' configure
	RUN sed -i 's/-O1/-O0/g' configure
	RUN sed -i 's/-O3/-O0/g' configure
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
# docker-ffmpeg-centos
# ffmpeg with lots of codecs on centos base

FROM centos
MAINTAINER Mohamed Boussaa "mohamed.boussaa@inria.fr"


RUN yum install -y \
  autoconf \
  automake \
  gcc \
  gcc-c++ \
  git \
  libtool \
  make \
  nasm \
  pkgconfig \
  zlib-devel


RUN mkdir /root/ffmpeg_sources
WORKDIR /root/ffmpeg_sources


# Yasm
# Yasm is an assembler used by x264 and FFmpeg.

RUN git clone --depth 1 git://github.com/yasm/yasm.git

RUN cd yasm && \
  autoreconf -fiv && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
  ./configure && \
  make && \
  make install


# libx264
# H.264 video encoder.

RUN git clone --depth 1 git://git.videolan.org/x264
RUN cd x264 && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
  ./configure --enable-static && \
  make && \
  make install


# libfdk_aac
# AAC audio encoder.

RUN git clone --depth 1 git://git.code.sf.net/p/opencore-amr/fdk-aac
RUN cd fdk-aac && \
  autoreconf -fiv && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
  ./configure --disable-shared && \
  make && \
  make install


# libmp3lame
# MP3 audio encoder.

RUN curl -L -O http://downloads.sourceforge.net/project/lame/lame/3.99/lame-3.99.5.tar.gz
RUN tar xzvf lame-3.99.5.tar.gz
RUN cd lame-3.99.5 && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
  ./configure --disable-shared --enable-nasm && \
  make && \
  make install


# libopus
# Opus audio decoder and encoder.

RUN git clone git://git.opus-codec.org/opus.git
RUN cd opus && \
  autoreconf -fiv && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
  ./configure --disable-shared && \
  make && \
  make install


# libogg
# Ogg bitstream library. Required by libtheora and libvorbis.

RUN curl -O http://downloads.xiph.org/releases/ogg/libogg-1.3.2.tar.gz
RUN tar xzvf libogg-1.3.2.tar.gz
RUN cd libogg-1.3.2 && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
  ./configure --disable-shared && \
  make && \
  make install


# libvorbis
# Vorbis audio encoder. Requires libogg.

RUN curl -O http://downloads.xiph.org/releases/vorbis/libvorbis-1.3.4.tar.gz
RUN tar xzvf libvorbis-1.3.4.tar.gz
RUN cd libvorbis-1.3.4 && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
  ./configure --with-ogg --disable-shared && \
  make && \
  make install


# libvpx
# VP8/VP9 video encoder.

RUN git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git
RUN cd libvpx && \
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
  ./configure --disable-examples && \
  make && \
  make install


# ffmpeg (finally!!!)

RUN git clone --depth 1 git://source.ffmpeg.org/ffmpeg
ENV PKG_CONFIG_PATH /usr/local/lib/pkgconfig
RUN cd ffmpeg &&\
  sed -i 's/-O2/-O1/g' configure && \
  sed -i 's/-O1/-O1/g' configure && \
  sed -i 's/-O3/-O1/g' configure && \
  ./configure --enable-gpl --enable-nonfree --enable-libfdk_aac --enable-libmp3lame --enable-libopus --enable-libvorbis --enable-libvpx --enable-libx264 &&\
  make &&\
  make install &&\
  hash -r


# cleanup 
WORKDIR /root
RUN rm -rf /root/ffmpeg_sources

RUN yum erase -y \
  autoconf \
  automake \
  gcc \
  gcc-c++ \
  git \
  libtool \
  nasm \
  zlib-devel

RUN yum clean all
>>>>>>> 39e00244d550fe58405e9eb279c2c88fe1db6e18
