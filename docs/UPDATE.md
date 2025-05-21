# Update Datapackage Dev Setup

This documentation is still work in progress, so please be cautious and think before action.
In any case, make a backup of everything relevant.

**Recommendation:** Exercise everything on a clean sandbox environment first.


## Repository Overview

GeoNode Docker Blueprint

- Serves as opinionated blueprint for GeoNode projects
  - track upstream changes
  - leverage docker and git features to adjust particular project requirements
  - see details: https://github.com/GeoNodeUserGroup-DE/geonode-blueprint-docker?tab=readme-ov-file#background 
- Serves as isolated feature development, e.g. datapackage, which involves multiple repositories
  - see also: https://github.com/GeoNodeUserGroup-DE/geonode-dev-datapackage

<pre>

                  <a href="https://github.com/GeoNodeUserGroup-DE/geonode-blueprint-docker">usergroup/geonode-docker-blueprint</a>
                                  ^
                                  | upstream
                                  |
                   <a href="https://github.com/GeoNodeUserGroup-DE/geonode-dev-datapackage">usergroup/geonode-dev-datapackage</a>

</pre>

Use features of Git or Docker (compose) to integrate external apps:

<pre>

                    <a href="https://github.com/GeoNodeUserGroup-DE/geonode-dev-datapackage">usergroup/geonode-dev-datapackage</a>
                                  |
                                  | via requirements.txt
                                  v
                     <a href="https://github.com/GeoNodeUserGroup-DE/contrib_datapackage">usergroup/contrib_datapackage</a>
</pre>

The development of datapackage extention for GeoNode incorporates customized core components which lay in their own Git repository each tracking their core upstream repository (e.g. GeoNode, geonode-mapstore-client). 
The feature development setup bundles everything necessary.
Updating features requires to merge changes from upstream core.

<pre>
                            <a href="https://github.com/GeoNode/geonode">geonode/geonode</a>
                                   ^
                    upstream/4.4.x |
                                   |
                           <a href="https://github.com/GeoNodeUserGroup-DE/geonode">usergroup/geonode</a>
         
         
                     <a href="https://github.com/GeoNode/geonode">geonode/geonode-mapstore-client</a>
                                    ^
                     upstream/4.4.x |
                                    |
                     <a href="https://github.com/GeoNodeUserGroup-DE/geonode-mapstore-client">usergroup/geonode-mapstore-client</a>

</pre>

## Update Workflow/Merge with Upstream

This description targets stable `4.4.x` to be updated.


### Update geonode-mapstore-client

Merge from `geonode/geonode-mapstore-client` on branch `upstream/4.4.x`.

> :bulb: Note:
>
> Before merging and testing any changes made to `geonode-mapstore-client` the `MapStore2` submodule must be aligned with upstream first.
> The tracking branches are configured in `.gitmodules` and can be updated with `git submodule update --remote`.

`geonode-mapstore-client` contains all compiled JavaScript artifacts which will conflict with local files in the `static/dist/js` folder.
These can be ignored/resetted during merge -- `git reset`, `git clean`, and `git restore` are helpful tools here.


### Update GeoNode

Merge from `geonode/geonode` on branch `upstream/4.4.x`.


### Update Development Setup

After updating all subcomponents, make sure to update all submodules:

- `geonode-mapstore-client`
- `externalapplications`

After that, update `usergroup/geonode-dev-datapackage` be merging from `usergroup/geonode-docker-blueprint`.


## Miscellaneous

Here is a somewhat reduced overview of the repository relationships:

![Repository Overview](./Repository%20Overview.png)
