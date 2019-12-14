FROM ubuntu:xenial

ENV DEBIAN_FRONTEND=noninteractive
ENV PATH=$PATH:/bin/:/usr/bin/

# Apt packages
ADD apt_requirements.txt apt_requirements.txt
RUN apt-get update && \
    apt-get install -y $(grep -vE "^\s*#" apt_requirements.txt  | tr "\n" " ") &&\
# CMake
    wget https://cmake.org/files/v3.7/cmake-3.7.2-Linux-x86_64.sh && \
    chmod +x ./cmake-3.7.2-Linux-x86_64.sh &&\
    ./cmake-3.7.2-Linux-x86_64.sh --skip-license && \

# Cleanup
    rm ./cmake-3.7.2-Linux-x86_64.sh && \
    apt-get -y autoremove && apt-get -y autoclean && apt-get -y purge wget

#Install dxvk and wine
RUN apt-get update && \
    #WINE install
    dpkg --add-architecture i386 && \
    wget -nc https://dl.winehq.org/wine-builds/winehq.key
    sudo apt-key -y add winehq.key
    apt-add-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main'
    apt-get update && \
    sudo apt install --install-recommends winehq-devel
    #Vulkan install
    add-apt-repository ppa:graphics-drivers/ppa
    apt-get upgrade && \
    sudo apt install nvidia-graphics-drivers-396 nvidia-settings vulkan vulkan-utils
    #DXVK install
    wget http://ftp.br.debian.org/debian/pool/main/d/dxvk/dxvk_0.96+ds1-1_all.deb
    wget http://ftp.br.debian.org/debian/pool/main/d/dxvk/dxvk-wine64-development_0.96+ds1-1_amd64.deb
    sudo dpkg -i -f dxvk*
    sudo apt install -f


ENV CC=gcc-4.8
ENV CXX=g++-4.8

