FROM barais/ffmpeg

MAINTAINER Olivier Barais: 0.1

WORKDIR /root/ffmpeg_sources
ENV CFLAGS="-Ofast -Wall"
ENV CXXFLAGS="-Ofast -Wall"
RUN tar xzvf yasm-1.3.0.tar.gz
WORKDIR /root/ffmpeg_sources/yasm-1.3.0
RUN sed -i 's/-O1/-Ofast/g' configure
RUN sed -i 's/-O0/-Ofast/g' configure
RUN sed -i 's/-O2/-Ofast/g' configure
RUN ./configure   --prefix="/root/ffmpeg_build" --bindir="/root/bin"
RUN make
RUN make install
RUN make distclean
WORKDIR /root/ffmpeg_sources
RUN tar xjvf last_x264.tar.bz2
WORKDIR /root/ffmpeg_sources/x264-snapshot-20141218-2245
RUN sed -i 's/-O1/-Ofast/g' configure
RUN sed -i 's/-O0/-Ofast/g' configure
RUN sed -i 's/-O2/-Ofast/g' configure
RUN sed -i 's/-O3/-Ofast/g' configure
RUN PATH="/root/bin:$PATH" ./configure --prefix="/root/ffmpeg_build" --bindir="/root/bin" --enable-static
RUN PATH="/root/bin:$PATH" make
RUN make install
RUN make distclean
WORKDIR /root/ffmpeg_sources
RUN unzip fdk-aac.zip
WORKDIR /root/ffmpeg_sources/mstorsjo-fdk-aac-83ac4a9
RUN autoreconf -fiv
RUN sed -i 's/-O1/-Ofast/g' configure
RUN sed -i 's/-O0/-Ofast/g' configure
RUN sed -i 's/-O2/-Ofast/g' configure
RUN sed -i 's/-O3/-Ofast/g' configure
RUN ./configure  --prefix="/root/ffmpeg_build" --disable-shared
RUN make
RUN make install
RUN make distclean
WORKDIR /root/ffmpeg_sources
RUN tar xzvf lame-3.99.5.tar.gz
WORKDIR /root/ffmpeg_sources/lame-3.99.5
RUN sed -i 's/-O1/-Ofast/g' configure
RUN sed -i 's/-O0/-Ofast/g' configure
RUN sed -i 's/-O2/-Ofast/g' configure
RUN sed -i 's/-O3/-Ofast/g' configure
RUN ./configure --prefix="/root/ffmpeg_build" --enable-nasm --disable-shared
RUN make
RUN make install
RUN make distclean
WORKDIR /root/ffmpeg_sources
RUN tar xzvf opus-1.1.tar.gz
WORKDIR /root/ffmpeg_sources/opus-1.1
RUN sed -i 's/-O1/-Ofast/g' configure
RUN sed -i 's/-O0/-Ofast/g' configure
RUN sed -i 's/-O2/-Ofast/g' configure
RUN sed -i 's/-O3/-Ofast/g' configure
RUN ./configure --prefix="/root/ffmpeg_build" --disable-shared
RUN make
RUN make install
RUN make distclean
WORKDIR /root/ffmpeg_sources
RUN tar xjvf libvpx-v1.3.0.tar.bz2
WORKDIR /root/ffmpeg_sources/libvpx-v1.3.0
RUN sed -i 's/-O1/-Ofast/g' configure
RUN sed -i 's/-O0/-Ofast/g' configure
RUN sed -i 's/-O2/-Ofast/g' configure
RUN sed -i 's/-O3/-Ofast/g' configure
RUN PATH="/root/bin:$PATH" ./configure --prefix="/root/ffmpeg_build" --disable-examples
RUN PATH="/root/bin:$PATH" make
RUN make install
RUN make clean
WORKDIR /root/ffmpeg_sources
RUN tar xjvf ffmpeg-snapshot.tar.bz2
WORKDIR /root/ffmpeg_sources/ffmpeg
RUN sed -i 's/-O1/-Ofast/g' configure
RUN sed -i 's/-O0/-Ofast/g' configure
RUN sed -i 's/-O2/-Ofast/g' configure
RUN sed -i 's/-O3/-Ofast/g' configure
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
