ARG BASE_IMAGE_VERSION
FROM geonode/geoserver:${BASE_IMAGE_VERSION}

## GeoServer plugins
#
# Make sure GeoServer container is prepared for each plugin!
# Use further RUN statements to install neccessary dependencies.
# Refer to plugin documentation what is required to use a plugin.
COPY ./plugins /usr/local/tomcat/webapps/geoserver/WEB-INF/lib
