# With Ubuntu 22.04, there is no need to install Mesa drivers
# from third-party ppa. The default driver already has lavapipe
# and vulkaninfo from the LunarG SDK works correctly.
FROM mcr.microsoft.com/vscode/devcontainers/base:ubuntu-24.04

LABEL Author="Juan David Adarve Bermudez <juanda0718@gmail.com>"

ARG DEBIAN_FRONTEND=noninteractive
ARG LUNARG_VERSION="1.4.313"

ENV TZ=Etc/UTC

# Base dependencies
RUN sudo apt update \
    && sudo apt install -y \
    python3-dev \
    software-properties-common \
    ca-certificates \
    curl \
    wget \
    build-essential \
    clang \
    python3-pip \
    git \
    unzip \
    golang \
    doxygen \
    graphviz \
    nodejs \
    npm \
    && rm -rf /var/lib/apt/lists/*

# Vulkan SDK from LunarG
RUN wget -qO - https://packages.lunarg.com/lunarg-signing-key-pub.asc | sudo apt-key add - \
    && sudo wget -qO /etc/apt/sources.list.d/lunarg-vulkan-${LUNARG_VERSION}-noble.list https://packages.lunarg.com/vulkan/${LUNARG_VERSION}/lunarg-vulkan-${LUNARG_VERSION}-noble.list \
    && sudo apt update \
    && sudo apt install -y vulkan-sdk

RUN pip3 install --no-cache-dir --break-system-packages \
    numpy \
    pytest \
    jinja2 \
    markupsafe \
    imageio \
    sphinx \
    sphinx-rtd-theme

# Bazelisk
RUN wget https://github.com/bazelbuild/bazelisk/releases/download/v1.27.0/bazelisk-amd64.deb \
    && sudo dpkg -i bazelisk-amd64.deb \
    && rm bazelisk-amd64.deb
