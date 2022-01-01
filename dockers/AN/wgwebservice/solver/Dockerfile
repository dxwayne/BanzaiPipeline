# /home/git/external/astrometry.net/docker/solver/Dockerfile
# time to run ~2.5 minutes
#
# /usr/local    # CMachine guarantee we have one
# /opt/build    # PMachine wg place to store logs in a container.
# /opt/images   # PMachine where user puts their images
# /opt/source   # PMachine place for user to hang source (container commit etc).
# /opt/indexes  # PMachine where user hangs their index library
# /opt/bin      # PMachine where user hangs any scripts/etc files.
#
#
# Goal: to base COPY the patched/clean AN clone from the PMachine directory.
# Make  astrometry net, install in CMachine:/usr/local/astrometry.
#
# TO TEST: (as needed)
# docker stop   <bd352e36779d>  # astrometrynet/solver:latest
# docker rmi -f <bd352e36779d>  # astrometrynet/solver:latest
#
# -no-cache    # start from scratch
# --build-arg "USERCMDS=$HOME/bin/an"
#
# docker build -t astrometrynet/solver:latest  --no-cache .
#
# SIMPLE TEST:  quick overall-sanity test:
#
# docker run -it --hostname nova-solver astrometrynet/solver:latest /bin/bash
#   ls /opt/indexes # make sure lots of index files are in
#   solve-field     # see if the help part pops out
#   exit            # quit sanity text, exit container.
#
# MORE INDEPTH TEST:
#
# docker run -it --hostname nova-solver -v /tmp/test:/tmp -v /tmp:/opt/log -v /opt/gao/data/Astrometry.net/index42xx:/opt/indexes  astrometrynet/solver:latest /bin/bash
#
#   solve-field              # look for help screen.
#   which solve-field        # /usr/local/bin/solve-field
#   ls /src/astrometry/demo  # yup! the demo files
#   ls /opt/indexes          # yup! the index files
#   ls /home/nova            # arg not there.
#
#
# docker build -t astrometrynet/solver:latest --build-arg USERCMDS=$HOME/bin/an  --no-cache .
#
# Container: /opt/indexes has index-4200- lots of files.

FROM astrometrynet/base:latest
#ARG USERCMDS=/dev/null

# --build-arg USERCMDS=$HOME/bin/an

# Astrometry.net clone added to base at /home/nova/src/astrometry
# git clone http://github.com/dstndstn/astrometry.net/astrometry.git

RUN cd /home/nova/src/astrometry                                                         \
    && make                                           >  /opt/log/cmachine_astrometry.log 2>&1  \
    && make py                                        >> /opt/log/cmachine_astrometry.log 2>&1  \
    && make extra                                     >> /opt/log/cmachine_astrometry.log 2>&1  \
    && make install INSTALL_DIR=/usr/local/astrometry >> /opt/log/cmachine_astrometry.log 2>&1  \
    && make clean                                     >> /opt/log/cmachine_astrometry.log 2>&1


# add us to the path
ENV PATH="/usr/local/astrometry/bin:/home/nova/bin:/opt/bin:${PATH}"
WORKDIR /home/nova


#Place to find user's images
RUN mkdir -p /opt/images
CMD /bin/bash
