FROM ubuntu:latest

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
    gcc \
    gdb \ 
    gcc-arm-none-eabi \
    gdb-arm-none-eabi \
    cmake \ 
    doxygen mscgen dia \
    cpplint \
    ruby \
    pip \
    udev \
    ninja-build \
    python3 python3-pip

# Install JLink
RUN wget --post-data 'accept_license_agreement=accepted&non_emb_ctr=confirmed&submit=Download+software' https://www.segger.com/downloads/jlink/JLink_Linux_arm.deb
RUN apt-get -y update
RUN apt-get -y install \
    libice6 libsm6 libx11-xcb1 libxcb-icccm4 libxcb-image0 libxcb-keysyms1 \
    libxcb-randr0 libxcb-render-util0 libxcb-render0 libxrender1 libxcb-shape0 libxcb-shm0 \
    libxcb-sync1 libxcb-util1 libxcb-xfixes0 libxcb-xkb1 libxext6 \
    libxkbcommon-x11-0 libxkbcommon0 x11-common xkb-data libxml2 libxslt-dev
# Work-around for missing udev in docker
RUN echo '#!/bin/bash\necho not running udevadm "$@"' > /usr/bin/udevadm && chmod +x /usr/bin/udevadm 
RUN dpkg -i JLink_Linux_arm.deb

# Install ceedling
RUN gem install ceedling
# Install gcovr
RUN pip install gcovr
