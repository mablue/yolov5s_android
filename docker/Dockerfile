FROM nvidia/cuda:11.6.1-cudnn8-devel-ubuntu20.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update --fix-missing
RUN apt-get install -y python3 python3-pip
RUN pip3 install --upgrade pip
RUN pip3 install tensorflow-gpu==2.4.0
RUN pip3 install torch==1.7.0
RUN pip3 install tensorflow-datasets

# install openvino
ENV http_proxy $HTTP_PROXY
ENV https_proxy $HTTP_PROXY
ARG DOWNLOAD_LINK=https://github.com/openvinotoolkit/openvino/archive/refs/tags/2024.2.0.tar.gz
ARG INSTALL_DIR=/opt/intel/computer_vision_sdk
ARG TEMP_DIR=/tmp/openvino_installer
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    cpio \
    sudo \
    lsb-release && \
    rm -rf /var/lib/apt/lists/*
RUN mkdir -p $TEMP_DIR && cd $TEMP_DIR
RUN wget -c $DOWNLOAD_LINK
RUN tar xf l_openvino_toolkit*.tgz
RUN cd l_openvino_toolkit*
RUN sed -i 's/decline/accept/g' silent.cfg
RUN ./install.sh -s silent.cfg
RUN rm -rf $TEMP_DIR
RUN pip3 install networkx
RUN pip3 install defusedxml

# install requirements for yolov5
RUN pip3 install -r requirements.txt
RUN apt-get update --fix-missing
RUN apt-get install -y libgl1 libgtk-3-0 libgtk-3-dev
RUN pip3 install pycocotools

# install requirements for model conversion
RUN pip3 install netron
RUN pip3 install onnx
RUN pip3 install onnx-simplifier
RUN pip3 install tensorflow-datasets
RUN pip3 install openvino2tensorflow==1.17.2
RUN pip3 install gdown

# Add a user that UID:GID will be updated by vscode
ARG USERNAME=developer
ARG GROUPNAME=developer
ARG UID=1000
ARG GID=1000
ARG PASSWORD=developer
RUN groupadd -g $GID $GROUPNAME && \
    useradd -m -s /bin/bash -u $UID -g $GID -G sudo $USERNAME && \
    echo $USERNAME:$PASSWORD | chpasswd && \
    echo "$USERNAME   ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER $USERNAME
ENV HOME /home/developer
