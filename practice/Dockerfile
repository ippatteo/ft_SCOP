FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        pkg-config \
        git \
        libx11-dev \
        libxrandr-dev \
        libxinerama-dev \
        libxcursor-dev \
    libxi-dev \
    libxext-dev \
    libxxf86vm-dev \
    libwayland-dev \
    wayland-protocols \
    libxkbcommon-dev \
        libglu1-mesa-dev \
        libgl1-mesa-dev \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /workspace
