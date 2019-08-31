FROM ubuntu:14.04

ENV DEBIAN_FRONTEND noninteractive

ENV WORKDIR_FOLDER=/tmp/workdir
RUN mkdir -p $WORKDIR_FOLDER

# used https://www.fromlatest.io/#/ to reduce docker image size

##########
## Dependences
##########
# cmake-gui does not available
# git is unnecessary
# p7zip-full add for alvaro
RUN apt-get -qq update -y && apt-get -qq install -y \
    build-essential ant cmake libeigen3-dev libtbb-dev python-dev python-pip checkinstall \
    pkg-config yasm libgtk2.0-dev libjpeg-dev libtiff4-dev libjasper-dev libopenexr-dev python-dev python-numpy \
    python-tk libtbb-dev yasm libfaac-dev libopencore-amrnb-dev libopencore-amrwb-dev \
    libtheora-dev libvorbis-dev libx264-dev libv4l-dev libdc1394-22-dev libavcodec-dev libavformat-dev \
    libswscale-dev  libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev protobuf-compiler \
    libgflags-dev libgoogle-glog-dev liblmdb-dev libconfig++ libfftw3-dev rsync\
 && apt-get -qq install -y --no-install-recommends libboost-all-dev p7zip-full \
 && rm -rf /var/lib/apt/lists/* && apt-get clean

##########
## Dependences for OPEN CV
##########
# Info from: https://www.learnopencv.com/install-opencv3-on-ubuntu/
# install os libraries
# git is unnecessary
RUN apt-get -qq update -y && apt-get -qq install -y \
    build-essential checkinstall cmake pkg-config yasm gfortran libjpeg8-dev libjasper-dev libpng12-dev \
 && rm -rf /var/lib/apt/lists/* && apt-get clean


# for ubuntu 14.04
RUN apt-get -qq update -y && apt-get -qq install -y libtiff4-dev \
 && rm -rf /var/lib/apt/lists/* && apt-get clean

RUN apt-get -qq update -y && apt-get -qq install -y \
    libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libxine2-dev libv4l-dev libgstreamer0.10-dev \
    libgstreamer-plugins-base0.10-dev qt5-default libgtk2.0-dev libtbb-dev  libatlas-base-dev  libfaac-dev \
    libmp3lame-dev libtheora-dev libvorbis-dev libxvidcore-dev libopencore-amrnb-dev libopencore-amrwb-dev \
    x264 v4l-utils \
 && rm -rf /var/lib/apt/lists/* && apt-get clean

# optional libraries for opencv
RUN apt-get -qq update -y && apt-get -qq install -y \
    libprotobuf-dev protobuf-compiler libgoogle-glog-dev libgflags-dev libgphoto2-dev libeigen3-dev libhdf5-dev doxygen \
 && rm -rf /var/lib/apt/lists/* && apt-get clean

##########
## Install python 3.6 and requirements
##########

WORKDIR $WORKDIR_FOLDER

ADD https://bootstrap.pypa.io/get-pip.py $WORKDIR_FOLDER
COPY requirements.txt $WORKDIR_FOLDER

ENV PYTHON_VER=3.6

RUN apt-get -qq update -y \
 && apt-get -qq install -y software-properties-common \
 && add-apt-repository ppa:deadsnakes/ppa -y \
 && apt-get -qq update -y \
 && apt-get -qq install python$PYTHON_VER -y \
 && rm -rf /var/lib/apt/lists/*  && apt-get clean \
# set instaled python as default
 && update-alternatives --install /usr/bin/python3 python3 /usr/bin/python$PYTHON_VER 1 \
 && update-alternatives --install /usr/bin/python python /usr/bin/python$PYTHON_VER 1 \
# install pip
 && python3 get-pip.py \
 && rm get-pip.py \
# set alias for python3
 && echo "alias python=python3" >> $HOME/.bash_aliases \
 && echo "alias pip=pip3" >> $HOME/.bash_aliases \
 && /bin/bash -c "source $HOME/.bash_aliases" \
# add requirements
 && pip install --upgrade pip \
 && pip install -r requirements.txt \
# delete workdir
 && rm -rf $WORKDIR_FOLDER


