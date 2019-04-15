#
# Dockerfile for scons-xc16
# Build environment for the Microchip MPLAB-XC16 IDE with SConstruct for Bitbucket Pipelines
#
# Author: Spencer Callicott <tuxprogrammer@gmail.com>
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 

FROM atlassian/default-image:2

LABEL MAINTAINER = 'Spencer Callicott <tuxprogrammer@gmail.com>'

ENV DEBIAN_FRONTEND noninteractive

# Install base dependencies
RUN dpkg --add-architecture i386 \
    && apt-get update -yq \
    && apt-get upgrade -yq \
    && apt-get install -yq --no-install-recommends build-essential bzip2 cpio curl python unzip wget \
    libc6:i386 libx11-6:i386 libxext6:i386 libstdc++6:i386 libexpat1:i386 \
    libxext6 libxrender1 libxtst6 libgtk2.0-0 libxslt1.1 libncurses5-dev python3-pip python3-setuptools python3-wheel python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Install scons
RUN pip3 install setuptools scons psutil

# Download and install xc16
RUN wget --progress=bar:force:noscroll http://www.microchip.com/mplabxc16linux -O xc16.run \
    && chmod +x ./xc16.run \
    && ./xc16.run --mode unattended --unattendedmodeui none \
    --netservername localhost --LicenseType FreeMode --prefix /opt/microchip/xc16 \
    && rm ./xc16.run

# Add xc16-gcc to path
ENV PATH $PATH:/opt/microchip/xc16/bin

# Report version back to user
RUN [ -x /opt/microchip/xc16/bin/xc16-gcc ] && xc16-gcc --version && /usr/bin/env python3 $(which scons) --version
