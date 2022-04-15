# /home/git/external/astrometry.net/docker/solver/Dockerfile
#
# /usr/local    # CMachine guarantee we have one
# /opt/build    # PMachine wg place to store logs in a container.
# /opt/images   # PMachine where user puts their images
# /opt/source   # PMachine place for user to hang source (container commit etc).
# /opt/indexes  # PMachine where user hangs their index library
# /opt/bin      # PMachine where user hangs any scripts/etc files.
#
# docker stop astrometrynet/base # if needed
# docker rmi -f astrometrynet/base
# docker build -t astrometrynet/base:latest .

# quick sanity test
# docker run -it --hostname basenova -v /tmp/test:/tmp  astrometrynet/base:latest /bin/bash
#
#
# quick overall-sanity test
# make sure lots of index files are in /opt/indexes
# exit from container
# docker run -it --hostname basenova -v /tmp/test:/tmp
#                                  -v /opt/gao/data/Astrometry.net/index42xx:/opt/indexes
#                  astrometrynet/base:latest /bin/bash
#
# Container: /opt/indexes has index-4200- lots of files.
#
FROM ubuntu:20.04

###################################################
# add compile environment (leave intact)
# get file supports etc
# Add python3 from apt
# Add less and vi (vim) for peeking into this mix.
###################################################
ENV DEBIAN_FRONTEND=noninteractive
RUN apt -y update && apt install -y apt-utils     && \
    apt install -y --no-install-recommends           \
    build-essential       \
    make                  \
    gcc                   \
    git                   \
    file                  \
    pkg-config            \
    wget                  \
    curl                  \
    swig                  \
    netpbm                \
    wcslib-dev            \
    wcslib-tools          \
    zlib1g-dev            \
    libbz2-dev            \
    libcairo2-dev         \
    libcfitsio-dev        \
    libcfitsio-bin        \
    libgsl-dev            \
    libjpeg-dev           \
    libnetpbm10-dev       \
    libpng-dev            \
    python3               \
    python3-dev           \
    python3-pip           \
    python3-pil           \
    python3-tk            \
    python3-setuptools    \
    python3-wheel         \
    python3-numpy         \
    python3-scipy         \
    python3-matplotlib    \
    kmod                  \
    unionfs-fuse          \
    less                  \
    locate                \
    emacs-nox             \
    vim

###################################################
# Retain the apt env for next builds. To remove:
#     && apt-get clean
#     && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
###################################################

###################################################
# Pip installs -- beef up python
###################################################
RUN for x in              \
    fitsio                \
    astropy               \
    ; do pip3 install --no-cache-dir $x; done

###################################################
# force some directories to be here (may not need)
# /opt/build   -v <mylogstosave>:/tmp/build # (or ignore)
# /opt/indices -v <myindicespath>:/opt/indices
# -- ln to things within the PMachine filesystem for /opt/sources
#  -- ln to things within the PMachine filesystem for /opt/indexes
#  -- ln to things within the PMachine filesystem for /opt/bin
###################################################

RUN mkdir -p /usr/local    # guarantee we have one
RUN mkdir -p /opt          # /build    # wg place to store logs in a container.
#RUN mkdir -p /opt/images   # where user puts their images
#RUN mkdir -p /opt/source   # place for user to hang source (container commit etc).
#RUN mkdir -p /opt/indexes  # where user hangs their index library
#RUN mkdir -p /opt/bin      # where user hangs any scripts/etc files.                  
RUN mkdir /src

# add a unprivileged user. (handy for exec/attach)
RUN useradd -m -d /home/nova --shell /bin/bash -G dialout -p "$(openssl passwd -1 novanova)" nova

# copy a PMachine git repo into this container.
# leave make to next step: build... solver
WORKDIR /home/nova/src
ADD astrometry .

# set contailer's default python = python3
RUN ln -s /usr/bin/python3 /usr/bin/python
ENV PYTHONPATH=/usr/local/lib/python

WORKDIR /home/nova
CMD /bin/bash


