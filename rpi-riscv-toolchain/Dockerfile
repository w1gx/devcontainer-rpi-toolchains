FROM ubuntu:latest as env

# Options for setup script
ARG INSTALL_ZSH="true"
ARG UPGRADE_PACKAGES="true"
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
COPY library-scripts/*.sh library-scripts/*.env /tmp/library-scripts/
RUN yes | unminimize 2>&1 \ 
    && bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "true" "true" \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts

RUN apt-get -y update && apt-get -y install \
    autoconf automake autotools-dev curl python3 python-is-python3  \
    libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex \
    texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev \
    libglib2.0-dev libfdt-dev libpixman-1-dev vim cpplint \
    cmake ninja-build && \
    apt clean

FROM env AS toolchain
RUN git clone --recursive https://github.com/riscv/riscv-gnu-toolchain
WORKDIR /riscv-gnu-toolchain
RUN ./configure --prefix=/opt/riscv && make linux -j `nproc`

FROM env AS qemu
RUN apt-get -y install \
    pkg-config libglib2.0-dev libpixman-1-dev
RUN git clone https://gitlab.com/qemu-project/qemu.git
WORKDIR /qemu
RUN ./configure --target-list=riscv64-linux-user && make -j `nproc` && make install

FROM env
COPY --from=toolchain /opt/riscv /opt/riscv
COPY --from=qemu /usr/local/bin/qemu* /usr/local/bin
RUN ln -s /opt/riscv/bin/* /usr/local/bin/
WORKDIR /root
