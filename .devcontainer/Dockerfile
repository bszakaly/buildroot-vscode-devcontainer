FROM ubuntu:20.04

RUN export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install -y \
  ## Buildroot mandatory packages
  locales \
  sed \
  make \
  binutils \
  build-essential \
  diffutils \
  gcc \
  g++ \
  bash \
  patch \
  gzip \
  bzip2 \
  perl \
  tar \
  cpio \
  unzip \
  rsync \
  file \
  bc \
  findutils \
  wget \
  ## Buildroot optional packages
  python3 \
  python3-pip \
  libncurses-dev \
  git \
  ## `gcc-?-plugin-dev` packages
  gcc-8-plugin-dev \
  gcc-9-plugin-dev \
  gcc-10-plugin-dev \
  ## Cleanup
  && rm -rf /var/lib/apt/lists/*

RUN locale-gen en_US.utf8

ARG USERNAME
# Default UID & GID which you should override in your `~/.bashrc` on your host system
ARG UID=1000
ARG GID=1000

# Make sure the user permissions in the container match the host user permissions
RUN groupadd -g $GID $USERNAME && useradd -ms /bin/bash $USERNAME -u $UID -g $GID

# Configure passwordless sudo for the user
RUN mkdir -p /etc/sudoers.d && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME

USER $UID