#From inside this folder
# docker build -t flixr/pprz-dep .
# docker run -t -i flixr/pprz-dep /bin/bash
# docker export pprz-dep | gzip -c > pprz-dep.tgz
# docker import pprz-dep < pprz-dep.tgz

FROM ubuntu:16.04
MAINTAINER Felix Ruess <felix.ruess@gmail.com>

# Add Tini
ENV TINI_VERSION v0.8.4
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

# setup environment
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8

# add Paparazzi PPA
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y software-properties-common
RUN add-apt-repository ppa:paparazzi-uav/ppa
RUN add-apt-repository ppa:team-gcc-arm-embedded/ppa

# install paparazzi-dev which pull in the dependencies
# also install cross compiler and some stuff for X
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y \
    gcc-arm-embedded \
    libcanberra-gtk-module \
    paparazzi-dev \
    paparazzi-jsbsim \
    x11-apps \
    gedit

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENTRYPOINT ["/tini", "--"]
CMD ["bash"]
