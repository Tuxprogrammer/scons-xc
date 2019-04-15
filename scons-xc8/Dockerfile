#
# Dockerfile for scons-xc8
# Build environment for the Microchip MPLAB-XC8 IDE with SConstruct for Bitbucket Pipelines
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

# Download and install xc8
RUN wget --progress=bar:force:noscroll http://www.microchip.com/mplabxc8linux -O xc8.run \
    && chmod +x ./xc8.run \
    && ./xc8.run --mode unattended --unattendedmodeui none \
    --netservername localhost --LicenseType FreeMode --prefix /opt/microchip/xc8 \
    && rm ./xc8.run

# Add xc8-gcc to path
ENV PATH $PATH:/opt/microchip/xc8/bin

# Report version back to user
RUN [ -x /opt/microchip/xc8/bin/xc8 ] && xc8 --ver && /usr/bin/env python3 $(which scons) --version
