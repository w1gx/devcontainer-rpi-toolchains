FROM ubuntu:20.04 as env

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
# Install additional packages
RUN apt-get -y update && apt-get -y install \
    autoconf automake autotools-dev curl python3 python-is-python3 python3.8-venv \
    libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex \
    texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev \
    libglib2.0-dev libfdt-dev libpixman-1-dev vim \
    cmake ninja-build libffi-dev libssl-dev ccache \
    libusb-1.0-0 dfu-util && \
    apt clean
# Install esp-idf
RUN su vscode --command "cd /home/vscode && \
    mkdir -p espc3; cd espc3; \
    git clone --recursive https://github.com/espressif/esp-idf.git && \
    cd esp-idf && \
    ./install.sh esp32c3 && \
    . ./export.sh"


    
