FROM debian:stable

# 设置非交互模式以避免时间区选择等提示
ENV DEBIAN_FRONTEND=noninteractive

# 更新系统并安装必要的工具和依赖
RUN apt-get update && apt-get upgrade -y && \
  apt-get install -y \
  cmake \
  python3 \
  python3-pip \
  python3-setuptools \
  wget \
  build-essential \
  sudo \
  curl \
  git \
  bash \
  ninja-build \
  unzip \
  doxygen \
  clang-format \
  gcc-arm-none-eabi \
  gdb-multiarch \
  clangd-19 \
  binutils-multiarch

# 配置shell为bash
RUN echo "dash dash/sh boolean false" | debconf-set-selections && \
  dpkg-reconfigure -p critical dash

# 安装 Python 依赖
RUN pip3 install "pycparser>=2.21" pyyaml "kconfiglib>=14.1.0" --break-system-packages

# 设置环境变量
ENV LC_ALL=C.UTF-8

# 设置passwordless sudo
RUN echo "ALL ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# 接受构建参数
ARG USERNAME=developer
ARG USER_UID=1000
ARG USER_GID=1000

# 调试信息 - 打印构建参数
RUN echo "Building with USERNAME=$USERNAME, USER_UID=$USER_UID, USER_GID=$USER_GID"

# 显示当前系统中的用户和组
RUN echo "Current system users with UID 1000:" && getent passwd 1000 || echo "No user with UID 1000" && \
    echo "Current system groups with GID 1000:" && getent group 1000 || echo "No group with GID 1000"

# 创建组和用户（处理已存在的情况）
RUN set -e; \
    echo "=== Creating group ===" && \
    if ! getent group $USER_GID >/dev/null 2>&1; then \
        echo "Creating new group $USERNAME with GID $USER_GID" && \
        groupadd --gid $USER_GID $USERNAME && \
        echo "Successfully created group $USERNAME with GID $USER_GID"; \
    else \
        EXISTING_GROUP=$(getent group $USER_GID | cut -d: -f1) && \
        echo "Group with GID $USER_GID already exists: $EXISTING_GROUP" && \
        if [ "$EXISTING_GROUP" != "$USERNAME" ]; then \
            echo "Adding user $USERNAME to existing group $EXISTING_GROUP"; \
        fi; \
    fi && \
    echo "=== Creating user ===" && \
    if ! getent passwd $USER_UID >/dev/null 2>&1; then \
        echo "Creating new user $USERNAME with UID $USER_UID" && \
        useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash $USERNAME && \
        echo "Successfully created user $USERNAME with UID $USER_UID"; \
    else \
        EXISTING_USER=$(getent passwd $USER_UID | cut -d: -f1) && \
        echo "User with UID $USER_UID already exists: $EXISTING_USER" && \
        if [ "$EXISTING_USER" != "$USERNAME" ]; then \
            echo "Warning: UID conflict! Expected $USERNAME but found $EXISTING_USER"; \
        fi; \
    fi && \
    echo "=== Setting sudo permissions ===" && \
    echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME && \
    echo "Successfully set sudo permissions for $USERNAME"

# 验证用户创建
RUN echo "=== Final user verification ===" && \
    echo "Checking if user $USERNAME exists:" && \
    if getent passwd $USERNAME >/dev/null 2>&1; then \
        echo "✓ User $USERNAME exists" && \
        id $USERNAME && \
        groups $USERNAME; \
    else \
        echo "✗ User $USERNAME does not exist!" && \
        echo "Available users:" && \
        getent passwd | grep -E ":(100[0-9]|1000):" && \
        exit 1; \
    fi

# 使用简单的CMD
CMD ["/bin/bash"]