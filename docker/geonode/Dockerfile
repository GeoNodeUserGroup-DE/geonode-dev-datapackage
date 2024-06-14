ARG BASE_IMAGE_VERSION
FROM geonode/geonode:${BASE_IMAGE_VERSION}

# we override geonode from base image here
# FIXME a submodule fails to build (error -> cannot replace directory .git to file)
RUN git remote add usergroup https://github.com/GeoNodeUserGroup-DE/geonode.git \
    && git fetch usergroup \
    && git stash save \
    && git switch datapackage_tabular-data \
    && git stash pop

COPY ./requirements.txt /usr/src/more_requirements.txt
RUN yes w | pip install --src /usr/src -r /usr/src/more_requirements.txt

COPY ./geonode-mapstore-client /usr/src/django_geonode_mapstore_client_dev
RUN yes w | pip install -e /usr/src/django_geonode_mapstore_client_dev
