FROM alpine:3.5

RUN echo "@edge-testing http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    echo "@edge-community http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories

ENV OPENCV_VERSION 3.2.0

RUN apk update \
  && apk upgrade \
  && apk add --no-cache \
      libtbb@edge-testing \
      libjpeg \
      libpng \
      libwebp \
      tiff \
      libjasper \
      python \
      py-numpy@edge-community \
      linux-headers \
      libtbb-dev@edge-testing \
  &&  apk add --no-cache --virtual .build-deps1 \
      build-base \
      clang \
      clang-dev \
      cmake \
      git \
      pkgconf \
      wget \
      libjpeg-turbo-dev \
      libpng-dev \
      tiff-dev \
      jasper-dev \
      python-dev \
      py-numpy-dev \
  && cd /tmp \
  && wget -O opencv-$OPENCV_VERSION.tar.gz https://github.com/opencv/opencv/archive/$OPENCV_VERSION.tar.gz \
  && tar -xzf opencv-$OPENCV_VERSION.tar.gz \
  && cd /tmp/opencv-$OPENCV_VERSION \
  && mkdir build \
  && cd build \
  && CC=/usr/bin/clang CXX=/usr/bin/clang++ cmake \
    -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr \
    -D WITH_FFMPEG=NO \
    -D WITH_IPP=NO \
    -D WITH_OPENEXR=NO \
    -D WITH_TBB=YES \
    -D WITH_1394=NO \
    -D BUILD_PERF_TESTS=OFF \
    -D BUILD_TESTS=OFF .. \
  && make -j2 \
  && make install \
  && cd /tmp \
  && rm -rf /tmp/opencv-* \
  && apk del .build-deps1 \
  && apk del libtbb-dev

ENV NODE_VERSION 6.11.1

RUN addgroup -g 1000 node \
    && adduser -u 1000 -G node -s /bin/sh -D node \
    && apk add --no-cache \
        libstdc++ \
        make \
        g++ \
        libgcc \
        gcc \
    && apk add --no-cache --virtual .build-deps2 \
        binutils-gold \
        curl \
        gnupg \
        linux-headers \
  # gpg keys listed at https://github.com/nodejs/node#release-team
  && for key in \
    9554F04D7259F04124DE6B476D5A82AC7E37093B \
    94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
    FD3A5288F042B6850C66B31F09FE44734EB7990E \
    71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
    DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    B9AE9905FFD7803F25714661B63B535A4C206CA9 \
    C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
    56730D5401028683275BD23C23EFEFE93C4CFFFE \
  ; do \
    gpg --keyserver pgp.mit.edu --recv-keys "$key" || \
    gpg --keyserver keyserver.pgp.com --recv-keys "$key" || \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key" ; \
  done \
    && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION.tar.xz" \
    && curl -SLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
    && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
    && grep " node-v$NODE_VERSION.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
    && tar -xf "node-v$NODE_VERSION.tar.xz" \
    && cd "node-v$NODE_VERSION" \
    && ./configure \
    && make -j$(getconf _NPROCESSORS_ONLN) \
    && make install \
    && cd .. \
    && rm -Rf "node-v$NODE_VERSION" \
    && rm "node-v$NODE_VERSION.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
    && apk del .build-deps2

RUN npm install -g node-gyp
