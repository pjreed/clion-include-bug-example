FROM osrf/ros:humble-desktop-full

ARG DEBIAN_FRONTEND=noninteractive

RUN sudo apt update \
    && sudo apt install -y \
        build-essential \
        gdb \
        ninja-build \
    && rm -rf /var/lib/apt/lists/*


