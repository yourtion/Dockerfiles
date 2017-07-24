#1 from base alpine
FROM alpine:3.6

#2 Add Edge and bleeding repos
RUN echo -e '@edgunity http://nl.alpinelinux.org/alpine/edge/community\n@edge http://nl.alpinelinux.org/alpine/edge/main\n@testing http://nl.alpinelinux.org/alpine/edge/testing' >> /etc/apk/repositories

#3 upgrade System
RUN apk update && apk upgrade && \
  apk add --no-cache --update \
    python3 \
    python3-dev \
    make \
    cmake \
    gcc \
    g++ \
    pkgconf \
    unzip \
    wget \
    py-pip \
    build-base \
    gsl \
    libavc1394-dev  \
    libtbb@testing  \
    libtbb-dev@testing   \
    libjpeg  \
    libjpeg-turbo  \
    libjpeg-turbo-dev \
    libpng-dev \
    libdc1394-dev \
    clang-dev \
    clang \
    tiff-dev \
    libwebp-dev \
    py-numpy-dev@edgunity \
    py-scipy-dev@testing \
    openblas-dev@edgunity \
    linux-headers \
    boost \
    boost-dev \
    openblas \
    openblas-dev

#3 defining compilers
ENV CC /usr/bin/clang
ENV CXX /usr/bin/clang++

#4 opencv3
ENV OPENCV_VERSION 3.2.0

RUN mkdir /opt && cd /opt && \
  wget https://github.com/opencv/opencv/archive/$OPENCV_VERSION.zip && \
  unzip $OPENCV_VERSION.zip && \
  rm -rf $OPENCV_VERSION.zip && \
  cd /opt/opencv-$OPENCV_VERSION && \
  mkdir build && \
  cd build && \
  cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_FFMPEG=NO \
  -D WITH_IPP=NO -D WITH_OPENEXR=NO .. && \
  make VERBOSE=1 && \
  make && \
  make install && \
  rm -rf /opt/opencv-$OPENCV_VERSION

#5 dlib

ENV DLIB_VERSION 19.4

RUN cd /opt && wget https://github.com/davisking/dlib/archive/v19.4.zip && \
  unzip v19.4.zip && \
  rm -rf v$DLIB_VERSION.zip && \
  cd /opt/dlib-$DLIB_VERSION && \
  mkdir build && \
  cd build && \
  cmake ..  && \
  make  && \
  make install &&\ 
  rm -rf /opt/
