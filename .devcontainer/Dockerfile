FROM fedora:43

# 安装必要的工具和依赖
RUN dnf update -y && dnf install -y --skip-unavailable\
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
  awk \
  binutils-multiarch \
  binutils-arm-none-eabi

# 安装 Python 依赖
RUN pip3 install "pycparser>=2.21" pyyaml "kconfiglib>=14.1.0"

# 设置环境变量
ENV LC_ALL=C.UTF-8

# 设置passwordless sudo
RUN echo "ALL ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# 接受构建参数
ARG USERNAME=developer
ARG USER_UID=1000
ARG USER_GID=1000

# 创建组和用户
RUN groupadd --gid $USER_GID $USERNAME \
  && useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash $USERNAME \
  && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
  && chmod 0440 /etc/sudoers.d/$USERNAME

# 使用简单的CMD
CMD ["/bin/bash"]