#/home/git/external/astrometry.net/docker/webservice/Dockerfile
# 
#
# /usr/local    # CMachine guarantee we have one
# /opt/build    # PMachine wg place to store logs in a container.
# /opt/images   # PMachine where user puts their images
# /opt/source   # PMachine place for user to hang source (container commit etc).
# /opt/indexes  # PMachine where user hangs their index library
# /opt/bin      # PMachine where user hangs any scripts/etc files.
#
# 
# docker build -t astrometrynet/webserver:latest .
# 
# docker run -it --hostname nova-webserver  astrometrynet/webserver:latest /bin/bash
# 
# docker run -it --hostname nova-webserver 
#   -v /tmp/nova:/tmp -v /tmp/nova/build:/opt/build -v /opt/gao/data/Astrometry.net/index42xx:/opt/indexes
#   astrometrynet/webserver:latest /bin/bash
# 
# 
FROM astrometrynet/solver:latest

RUN pip3 install --no-cache-dir Django

ENV DEBIAN_FRONTEND=noninteractive
RUN apt -y update &&                       \
    apt install -y --no-install-recommends \
    apache2                                \
    libapache2-mod-wsgi-py3

# emacs-nox was added to base directly.

RUN pip3 install --no-cache-dir            \
    social-auth-core django-social-auth3 social-auth-app-django

# Where the guts of the web interface lives.
# /my/gitrepos/astrometry.net/net
WORKDIR /src/astrometry/net

RUN ln -s settings_test.py settings.py

# Yuck!  The installed 'astrometry' package conflicts with '.', so paste it in...
# RUN rm -R /usr/local/lib/python/astrometry/net && \
#     ln -s /src/astrometry/net /usr/local/lib/python/astrometry/net

RUN mkdir appsecrets                       && \
    touch appsecrets/__init__.py           && \webser
    touch appsecrets/auth.py

COPY django_db.py /src/astrometry/net/appsecrets/

#RUN git stash && git pull
#
#RUN mv migrations/* /tmp                                 
#RUN     python manage.py makemigrations                      
#RUN     python manage.py migrate                             
#RUN     python manage.py makemigrations net                  
#RUN     python manage.py migrate net                         
#RUN     python manage.py loaddata fixtures/initial_data.json 
#RUN     python manage.py loaddata fixtures/flags.json
#
#CMD python manage.py runserver

#EXPOSE 8000
