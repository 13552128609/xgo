# Go cross compiler (xgo): Go 1.16.5 layer
# Copyright (c) 2014 Péter Szilágyi. All rights reserved.
#
# Released under the MIT license.

FROM karalabe/xgo-base

MAINTAINER Péter Szilágyi <peterke@gmail.com>

# Configure the Go packages and bootstrap them

#RUN \
#  export DIST_LINUX_64=https://go.dev/dl/go1.16.5.linux-amd64.tar.gz && \
#  export DIST_LINUX_64_SHA=b12c23023b68de22f74c0524f10b753e7b08b1504cb7e417eccebdd3fae49061 && \
#  \
#  export DIST_LINUX_ARM=https://go.dev/dl/go1.16.5.linux-arm64.tar.gz && \
#  export DIST_LINUX_ARM_SHA=d5446b46ef6f36fdffa852f73dfbbe78c1ddf010b99fa4964944b9ae8b4d6799 && \
#  \
#  export DIST_OSX_64=https://go.dev/dl/go1.16.5.darwin-arm64.tar.gz && \
#  export DIST_OSX_64_SHA=7b1bed9b63d69f1caa14a8d6911fbd743e8c37e21ed4e5b5afdbbaa80d070059 && \
#  \
#  export DIST_OSX_32=https://go.dev/dl/go1.16.5.darwin-amd64.tar.gz && \
#  export DIST_OSX_32_SHA=be761716d5bfc958a5367440f68ba6563509da2f539ad1e1864bd42fe553f277 && \
#  \
#  export DIST_WIN_64=https://go.dev/dl/go1.16.5.windows-amd64.zip && \
#  export DIST_WIN_64_SHA=0a3fa279ae5b91bc8c88017198c8f1ba5d9925eb6e5d7571316e567c73add39d && \
#  \
#  $BOOTSTRAP

RUN \
  export DIST_LINUX_64=https://go.dev/dl/go1.16.5.linux-amd64.tar.gz && \
  export DIST_LINUX_64_SHA=b12c23023b68de22f74c0524f10b753e7b08b1504cb7e417eccebdd3fae49061 && \
  \
  #$BOOTSTRAP_PURE
  $BOOTSTRAP

ENV GO_VERSION 1165
