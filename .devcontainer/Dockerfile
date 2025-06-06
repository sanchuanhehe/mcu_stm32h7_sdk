FROM fedora:43

# 安装必要的工具和依赖
RUN dnf update -y && dnf install -y \
  cmake \
  python3 \
  python3-pip \
  python3-setuptools \
  wget \
  gcc \
  gcc-c++ \
  make \
  sudo \
  curl \
  git \
  bash \
  ninja-build \
  unzip \
  doxygen \
  clang-tools-extra \
  arm-none-eabi-gcc-cs \
  arm-none-eabi-gcc-cs-c++ \
  gdb \
  clang-tools-extra \
  awk

# 安装 Python 依赖
RUN pip3 install "pycparser>=2.21" pyyaml "kconfiglib>=14.1.0"

# 设置环境变量
ENV LC_ALL=C.UTF-8

# 设置passwordless sudo
RUN echo "ALL ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# 创建vscode用户
RUN useradd -m -s /bin/bash -u 1000 vscode && \
  echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
  usermod -aG wheel vscode

USER vscode
