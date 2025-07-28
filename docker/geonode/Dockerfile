ARG BASE_IMAGE_VERSION
FROM geonode/geonode:${BASE_IMAGE_VERSION}

# we override geonode from base image here
# FIXME a submodule fails to build (error -> cannot replace directory .git to file)
RUN git remote add usergroup https://github.com/GeoNodeUserGroup-DE/geonode.git 2> /dev/null
RUN git fetch -q usergroup
RUN git switch geonode-v4.4.3+tabular001

COPY ./requirements.txt /usr/src/more_requirements.txt
RUN yes w | pip install --src /usr/src -r /usr/src/more_requirements.txt

COPY ./geonode-mapstore-client /usr/src/django_geonode_mapstore_client_dev
RUN yes w | pip install -e /usr/src/django_geonode_mapstore_client_dev

RUN true \
    # django logs are written to stdout by default (see uwsgi.ini)
    # forward celery logs to docker log collector
    && ln -sf /dev/stdout ${CELERY__LOG_FILE:-/var/log/celery.log}

#RUN rm .env*
