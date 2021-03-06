# Go cross compiler (xgo): Base cross-compilation layer
# Copyright (c) 2014 Péter Szilágyi. All rights reserved.
#
# Released under the MIT license.

FROM ubuntu:20.04

MAINTAINER Péter Szilágyi <peterke@gmail.com>

# Mark the image as xgo enabled to support xgo-in-xgo
ENV XGO_IN_XGO 1


# Configure the Go environment, since it's not going to change
ENV PATH   /usr/local/go/bin:$PATH
ENV GOPATH /go


# Inject the remote file fetcher and checksum verifier
ADD fetch.sh /fetch.sh
ENV FETCH /fetch.sh
RUN chmod +x $FETCH

ARG DEBIAN_FRONTEND=noninteractive

# Make sure apt-get is up to date and dependent packages are installed
RUN \
  apt-get update && \
  apt-get install -y automake autogen build-essential ca-certificates                    \
    gcc-9-arm-linux-gnueabi g++-9-arm-linux-gnueabi libc6-dev-armel-cross                \
    gcc-9-arm-linux-gnueabihf g++-9-arm-linux-gnueabihf libc6-dev-armhf-cross            \
    gcc-9-aarch64-linux-gnu g++-9-aarch64-linux-gnu libc6-dev-arm64-cross                \
    gcc-9-mips-linux-gnu g++-9-mips-linux-gnu libc6-dev-mips-cross                       \
    gcc-9-mipsel-linux-gnu g++-9-mipsel-linux-gnu libc6-dev-mipsel-cross                 \
    gcc-9-mips64-linux-gnuabi64 g++-9-mips64-linux-gnuabi64 libc6-dev-mips64-cross       \
    gcc-9-mips64el-linux-gnuabi64 g++-9-mips64el-linux-gnuabi64 libc6-dev-mips64el-cross \
    gcc-9-multilib g++-9-multilib gcc-mingw-w64 g++-mingw-w64 clang llvm-dev             \
    libtool libxml2-dev uuid-dev libssl-dev swig openjdk-8-jdk pkg-config patch          \
    make xz-utils cpio wget zip unzip p7zip git mercurial bzr texinfo help2man           \
    cmake  zlib1g-dev                                                                           \
    --no-install-recommends

# Fix any stock package issues
RUN ln -s /usr/include/asm-generic /usr/include/asm

# Configure the container for OSX cross compilation
#ENV OSX_SDK     MacOSX10.11.sdk
ENV OSX_SDK     MacOSX10.12.sdk

ENV OSX_NDK_X86 /usr/local/osx-ndk-x86

RUN \
#  OSX_SDK_PATH=https://s3.dockerproject.org/darwin/v2/$OSX_SDK.tar.xz && \
#  $FETCH $OSX_SDK_PATH dd228a335194e3392f1904ce49aff1b1da26ca62       && \

  OSX_SDK_PATH=https://github.com/phracker/MacOSX-SDKs/releases/download/11.3/$OSX_SDK.tar.xz && \
    $FETCH $OSX_SDK_PATH b314704d85934481c9927a0450db1768baf9af9efe649562fcb1a503bb44512f       && \

  \
  git clone https://github.com/tpoechtrager/osxcross.git && \
  mv `basename $OSX_SDK_PATH` /osxcross/tarballs/        && \
  \
  #sed -i -e 's|-march=native||g' /osxcross/build_clang.sh /osxcross/wrapper/build.sh && \
  sed -i -e 's|-march=native||g' /osxcross/build_clang.sh && \
  UNATTENDED=yes OSX_VERSION_MIN=10.6 /osxcross/build.sh                             && \
  mv /osxcross/target $OSX_NDK_X86                                                   && \
  \
  rm -rf /osxcross

ENV LD_LIBRARY_PATH $OSX_NDK_X86/lib

ADD patch.tar.xz $OSX_NDK_X86/SDK/$OSX_SDK/usr/include/c++
ENV PATH $OSX_NDK_X86/bin:$PATH

# Inject the old Go package downloader and tool-chain bootstrapper
ADD bootstrap.sh /bootstrap.sh
ENV BOOTSTRAP /bootstrap.sh
RUN chmod +x $BOOTSTRAP

# Inject the new Go root distribution downloader and bootstrapper
ADD bootstrap_pure.sh /bootstrap_pure.sh
ENV BOOTSTRAP_PURE /bootstrap_pure.sh
RUN chmod +x $BOOTSTRAP_PURE

# Inject the Go source distribution downloader and bootstrapper
ADD bootstrap_repo.sh /bootstrap_repo.sh
ENV BOOTSTRAP_REPO /bootstrap_repo.sh
RUN chmod +x $BOOTSTRAP_REPO

# Inject the C dependency cross compiler
ADD build_deps.sh /build_deps.sh
ENV BUILD_DEPS /build_deps.sh
RUN chmod +x $BUILD_DEPS

# Inject the container entry point, the build script
ADD build.sh /build.sh
ENV BUILD /build.sh
RUN chmod +x $BUILD

ENTRYPOINT ["/build.sh"]
