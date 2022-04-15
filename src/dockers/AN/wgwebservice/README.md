#Docker containers for Astrometry.net
# 1) Make a base with OS and other goodies; 2) build solver on base 3) build webservice on solver

(cd base && git clone astronomy.git && docker build -t astrometrynet/base:latest .)
(cd solver && docker build -t astrometrynet/solver:latest .)

(cd webservice && docker build -t astrometrynet/webservice:latest .)

docker run --net=host astrometrynet/webservice

# docker run -it --name ansvr  --net=host astrometrynet/webservice /bin/bash
# docker run --name ansvr  -v /opt/gao/data/Astrometry.net/CHO:/usr/local/data --net=host astrometrynet/webservice
# dcls
# docker exec -it 0a6f5ee0fb83  /bin/bash

docker ps --no-trunc
/bin/sh -c 'python manage.py runserver'



./src/astrometry/net/manage.py
./src/astrometry/net/settings/


---
docker pull ubuntu:20.04   # get this down to save time.
cd docker/base
build base: this makes the 'container' with python, compiler etc; 11 minutes.
  docker run -it --hostname testme -t testme astrometrynet/base:latest /bin/bash
  # fiddle around
  docker pause testme
  docker commit testme
