# Geonode Datapackage Feature Development

This repository assembles a setup to push forward develop of the non-spatial data import feature for [GeoNode](https://geonode.org/), a spatial content management system.
All needed components are available as [Docker](https://www.docker.com/) images and will be set up and run via the [docker compose](https://docs.docker.com/compose/) tool.

## Background

The whole setup is based on the [Docker Blueprint for a GeoNode Installation](https://github.com/GeoNodeUserGroup-DE/geonode-blueprint-docker).
The blueprint is an opnionated GeoNode setup, but helps to keep everything necessary in one place to develop the datapackage feature which adds changes on multiple repositories:

- [GeoNodeUserGroup-DE/geonode/](https://github.com/GeoNodeUserGroup-DE/geonode/tree/datapackage_tabular-data) (branch `datapackage_tabular-data`)
- [GeoNodeUserGroup-DE/geonode-mapstore-client/](https://github.com/GeoNodeUserGroup-DE/geonode-mapstore-client/tree/datapackage_tabular-data) (Branch `datapackage_tabular-data`)
- https://github.com/GeoNodeUserGroup-DE/geonode-importer/
- https://github.com/GeoNodeUserGroup-DE/importer-datapackage/

Under `./.devcontainer` you find a configuration to run and debug the project as [`devcontainer`](https://containers.dev/).
The setup integrates nicely with IDEs [like vs-code](https://code.visualstudio.com/docs/devcontainers/containers).

For detailed background information about the genesis of the blueprint and how this relates to the actual GeoNode upstream checkout the [Background section](https://github.com/GeoNodeUserGroup-DE/geonode-blueprint-docker?tab=readme-ov-file#background) of the blueprint repository itself.

## Setup Project

Make sure you have installed `git`, `Docker` and `docker compose`.

Clone the [repository containing a GeoNode Docker setup]( https://github.com/GeoNodeUserGroup-DE/geonode-dev-datapackage) and change directory your local working copy:

```
git clone --recurse-submodules https://github.com/GeoNodeUserGroup-DE/geonode-dev-datapackage geonode-dev-datapackage
cd geonode-dev-datapackage
```

## Configuration

> :bulb: **Note**
>
> Settings (e.g. geodatabase parameters) are mainly configured in the `.env` file. 
> To review in-built default settings of an image, run the `env` command on an image.
> For example `docker run geonode/geoserver env | sort`.
>
> For a complete set of available options take the [GeoNode Settings](https://docs.geonode.org/en/master/basic/settings/index.html#settings) documentation as a reference.

The containers get configured during creation via environment variables. 
The `geonode/settings.py` settings module takes further configuration of the GeoNode containers (`django` and `celery`) and aligns some names with those documented.


Copy the `sample.env` to `.env` and make your changes (`.env` is not versioned).
For a quick start taking default values you can run `docker compose up -d --env-file=sample.env`.


Have a look at the [Ways to set environment variables in Compose](https://docs.docker.com/compose/environment-variables/set-environment-variables/) documentation.


### TLS Config

If you want to configure a TLS certificate, you can mount key and cert as `pem`s in the `geonode` service within the `docker-compose.yml` file.
Uncomment the corresponding lines:

 ```sh
 volumes:
    - nginx-confd:/etc/nginx
    - statics:/mnt/volumes/statics
    # Link to a custom certificate here
    #- <path-to-cert>.pem:/geonode-certificates/autoissued/fullchain.pem
    #- <path-to-key>.pem:/geonode-certificates/autoissued/privkey.pem
 ```


## Start and Run

### Docker-Compose Basics

Run `docker compose up -d` to start all geonode components.
Review all started components by executing `docker compose ps`. 
You can follow logs via `docker compose logs -f` and optionally pass a service to only follow a service's log.

Stop all components via `docker compose down`, and pass a `-v` flag to clean up all volumes (CAUTION: removes all persisted data).

For more features and available commands, `docker compose --help`, or read [the docker compose CLI documentation](https://docs.docker.com/compose/reference/).
