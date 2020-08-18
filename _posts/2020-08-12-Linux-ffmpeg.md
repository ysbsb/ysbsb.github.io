---
layout: post
title: 리눅스 우분투에서 ffmpeg 설치하기
subtitle: Linux Ubuntu ffmpeg install
date:   2020-08-18 10:41:41
author: Subin Yang
category: [Linux]
tags: [Linux]

---





> 리눅스 생활일지편 입니다.
>
> 이번 포스팅에서는 우분투에서 ffmpeg 설치하는 방법에 대해 이야기 하려고 합니다.



ffmpeg를 설치하기에 앞서 우분투에서 package update와 upgrade를 해줍니다.



```shell
$ sudo apt-get update
$ sudo apt-get upgrade
```





<h3>dependency libraries 설치</h3>

```shell
# dependency libraries 설치
$ sudo apt-get -y install autoconf automake build-essential cmake git-core libass-dev libfreetype6-dev libgnutls28-dev libsdl2-dev libgpac-dev libsdl1.2-dev libtheora-dev libtool libva-dev libvdpau-dev libvorbis-dev libxcb1-dev libxcb-shm0-dev libx11-dev libxext-dev libxfixes-dev libxcb-xfixes0-dev pkg-config texinfo texi2html wget yasm zlib1g-dev

```



<h3>YASM install</h3>

```shell
$ mkdir ~/ffmeg_sources
$ cd ~/ffmpeg_sources
$ wget http://www.tortall.net/projects/yasm/releases/yasm-1.2.0.tar.gz
$ tar xzvf yasm-1.2.0.tar.gz
$ cd yasm-1.2.0
$ ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin"
$ make
$ make install
$ make distclean
```





<h3>NASM install</h3>

```shell
$ cd ~/ffmpeg_sources && \
wget https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.bz2 && \
tar xjvf nasm-2.14.02.tar.bz2 && \
cd nasm-2.14.02 && \
./autogen.sh && \
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" && \
make && \
make install
```





<h3>libx264 install</h3>

```shell
$ cd ~/ffmpeg_sources
$ wget http://download.videolan.org/pub/x264/snapshots/last_x264.tar.bz2
# error message
--2020-08-12 06:06:53--  http://download.videolan.org/pub/x264/snapshots/last_x264.tar.bz2
Resolving download.videolan.org (download.videolan.org)... 213.36.253.2, 2a01:e0d:1:3:58bf:fa02:c0de:5
Connecting to download.videolan.org (download.videolan.org)|213.36.253.2|:80... connected.
HTTP request sent, awaiting response... 404 Not Found
2020-08-12 06:06:54 ERROR 404: Not Found.
```



```shell
$ cd ~/ffmpeg_sources && \
git -C x264 pull 2> /dev/null || git clone --depth 1 https://code.videolan.org/videolan/x264.git && \
cd x264 && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --enable-static --enable-pic && \
PATH="$HOME/bin:$PATH" make && \
make install
```



<h3>libx265 install</h3>

```shell
$ sudo apt-get install libx265-dev libnuma-dev
```



<h3>libvpx install</h3>

```shell
$ sudo apt-get install libvpx-dev
```



<h3>libfdk-aac install</h3>

```shell
$ sudo apt-get install libfdk-aac-dev
```



<h3>libmp3lame install</h3>

```shell
$ sudo apt-get install libmp3lame-dev
```



<h3>libopus install</h3>

```shell
$ sudo apt-get install libopus-dev
```



<h3>libaom install</h3>

```shell
$ cd ~/ffmpeg_sources && \
git -C aom pull 2> /dev/null || git clone --depth 1 https://aomedia.googlesource.com/aom && \
mkdir -p aom_build && \
cd aom_build && \
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off -DENABLE_NASM=on ../aom && \
PATH="$HOME/bin:$PATH" make && \
make install
```





<h3>ffmpeg install</h3>

```shell
$ apt-get install libunistring-dev
```



```shell
$ cd ~/ffmpeg_sources && \
wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 && \
tar xjvf ffmpeg-snapshot.tar.bz2 && \
cd ffmpeg && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="$HOME/ffmpeg_build" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I$HOME/ffmpeg_build/include" \
  --extra-ldflags="-L$HOME/ffmpeg_build/lib" \
  --extra-libs="-lpthread -lm" \
  --bindir="$HOME/bin" \
  --enable-gpl \
  --enable-gnutls \
  --enable-libaom \
  --enable-libass \
  --enable-libfdk-aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-nonfree && \
PATH="$HOME/bin:$PATH" make && \
make install && \
hash -r
```




