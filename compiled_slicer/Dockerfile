FROM ubuntu:16.04
MAINTAINER Pierre-Olivier Quirion <poq@criugm.qc.ca>

# download slicer

ENV SLICER_DIR /usr/local/Slicer
ENV SCT_DIR /usr/local/sct


RUN apt-get update
RUN apt-get install --force-yes -y subversion git-core git-svn  \
    make gcc g++ libx11-dev libxt-dev libgl1-mesa-dev libglu1-mesa-dev \
    libfontconfig-dev libxrender-dev libncurses5-dev curl

WORKDIR /usr/local
RUN curl -O https://cmake.org/files/v3.8/cmake-3.8.2-Linux-x86_64.tar.gz \
    && tar -xzvf cmake-3.8.2-Linux-x86_64.tar.gz

WORKDIR /usr/local/bin

RUN ln -s ../cmake-3.8.2-Linux-x86_64/bin/* .

RUN apt-get install --force-yes -y libqt4-dev  libqtwebkit-dev libqtcore4 \
    libqtgui4 qt4-dev-tools

ADD . $SCT_DIR

RUN mkdir /tmp/Slicer && mkdir -p /tmp/slicer_build
WORKDIR /tmp
#RUN git clone https://github.com/Slicer/Slicer.git
#ADD Slicer /tmp/


RUN cd /tmp/slicer_build && cmake -DCMAKE_BUILD_TYPE:STRING=Release -DSlicer_USE_SYSTEM_QT:BOOL=1 $SCT_DIR/Slicer
RUN  cd /tmp/slicer_build && make -j 8
RUN cd /tmp/slicer_build && make install

#RUN git apply $SCT_DIR/install/Slicer_patch
RUN mkdir -p  /tmp/build_ant
