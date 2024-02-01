FROM ros:noetic

LABEL org.opencontainers.image.authors="ohin"

ARG USERNAME=yolo
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG TZ=Asia/Taipei

ENV SHELL /bin/bash

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    build-essential \
    curl \
    git \
    sudo \
    wget \
    vim \
    tmux \
    net-tools \
    gnupg2 \
    lsb-release \
    python3-pip \
    v4l-utils \
    libgl1-mesa-glx \
    libqt5gui5 \
    && rm -rf /var/lib/apt/lists/*
RUN apt-get clean all

RUN pip3 install numpy \
    && pip3 install opencv-python

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Set timezone
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

USER $USERNAME
SHELL ["/bin/bash","-ic"]

#Install Intel RealSense SDK 2.0
RUN sudo mkdir -p /etc/apt/keyrings \
    && curl -sSf https://librealsense.intel.com/Debian/librealsense.pgp | sudo tee /etc/apt/keyrings/librealsense.pgp > /dev/null \
    && sudo apt-get install apt-transport-https
RUN echo "deb [signed-by=/etc/apt/keyrings/librealsense.pgp] https://librealsense.intel.com/Debian/apt-repo `lsb_release -cs` main" | \
    sudo tee /etc/apt/sources.list.d/librealsense.list \
    && sudo apt-get update
RUN sudo apt-get install -y librealsense2-dkms \
    && sudo apt-get install -y librealsense2-utils \
    && pip3 install pyrealsense2 \
    && sudo rm -rf /var/lib/apt/lists/*

#source to bashrc
RUN echo "export PATH=/home/$USERNAME/.local/bin:$PATH" >> /home/$USERNAME/.bashrc \
    && echo "source /opt/ros/noetic/setup.bash" >> /home/$USERNAME/.bashrc \
    && source /home/$USERNAME/.bashrc

#install pytorch and ultralytics
RUN pip3 install --upgrade pip python-dateutil \
    && pip3 install pillow \
    && pip3 install torch torchvision \
    && pip3 install ultralytics

#goto workspace
WORKDIR /home/$USERNAME
RUN git clone https://github.com/gary443211/workspace