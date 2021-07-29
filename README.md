Supported tags
==============

Tags identify a specific release (i.e. version) of ShowVoc, while the tag `latest` is reference to the latest release (currently, `1.0.0`).

What is ShowVoc?
=============

ShowVoc is the web application for [PMKI](https://ec.europa.eu/isa2/actions/overcoming-language-barriers_en) an action funded by the ISA<sup>2</sup> programme aiming at overcoming language barriers withing
a digital single market. As part of its mission, the action developed a pilot of an open data portal that combines traditional data provision following LOD policies with global activities (e.g. global search and navigation of dataset relationships).

The portal is powered by [Semantic Turkey](http://semanticturkey.uniroma2.it/), an open-source platform for Knowledge Acquisition and Management realized by the [ART Research Group](http://art.uniroma2.it/) at the [University of Rome Tor Vergata](http://www.uniroma2.it/).

How to use this image
=====================

Build an image from sources
---------------------------

* `cd` into the subdirectory associated with the version of interest
* copy the *full* distribution of ShowVoc, which can be found among the [downloads](https://bitbucket.org/art-uniroma2/showvoc/downloads/).
* issue the following command

  `docker build -t showvoc:<version> .`

  where `<version>` is the version number (e.g. `1.0.0`)

Start a ShowVoc instance
---------------------

Executing the following command to start a new container.

`docker run -p 1979:1979 --name showvoc-instance-name -t showvoc:tag`

where *showvoc-instance-name* is the name assigned to the newly created container, *tag* is the tag specifying the desired ShowVoc version.

The parameter `-p` is use to the the port `1979` in the *container* (on the right) to the
same port in the *host* (on the left). If the port should only be accessible locally, use `-p 127.0.0.1:1979:1979`.

After a while, ShowVoc should be reachable at `http://localhost:1979/showvoc` 

Caveats
=======

Where to store data
-------------------

A docker container has a *writable layer* on top of the layers associated with the image from which it was spawned from. This layer is where every modification to the file system is stored. Unfortunately, its content is lost when the container is deleted, it is difficult to use from the host, and hardly shareable with other containers.

For these reasons, important data should be stored in *volumes* managed by Docker (preferable, but more complex) or mapped to the host file system.

The latter can be done in this manner:

1. Create a directory for ShowVoc

   `mkdir -p volumes/stdata`

2. Start your `showvoc` container like this:

   `docker run -v ${PWD}/volumes/stdata:/opt/vocbench3/data -p 1979:1979 --name showvoc-instance-name -t showvoc:tag`

The container is executed as root
---------------------------------

The container is executed by `root`, which may not be appropriate for production deployments.


Example deployment
==================

The file `docker-compose.yml` for [Docker Compose](https://docs.docker.com/compose/) specified a deployment using [Ontext GraphDB](http://graphdb.ontotext.com/) SE. It also uses [VocBench 3](http://vocbench.uniroma2.it/) to host the development of resources.

Follow these instructions to create the deployment:
 * create the data directory for Semantic Turkey used by ShowVoc and VocBench, respectively
   
   ```
   mkdir volumes/showvoc-stdata
   mkdir volumes/development-stdata
   ```
 * create the data directory for GraphDB. In fact, there will be two instances used by
   ShowVoc and VocBench 3, respectively
   
   ```
   mkdir volumes/showvoc-gdbhome
   mkdir volumes/development-gdbhome
   ```

* place the license for GraphDB SE under `volumes/showvoc-gdbhome/conf` and `volumes/development-gdbhome/conf` in a file named `graphdb.license`

* start the deployment in detached mode

  `docker-compose up -d`

  assuming the the file `docker-compose.yml` is available in the present working directory.

After a while, ShowVoc should be available at the address `http://localhost:1979/showvoc`. The development VocBench 3 instance should be available at the address `http://localhost:1979/vocbench3`

The GraphDB instances is linked to ShowVoc is accessible from the that container using the address `http://showvoc-graphdb:7200` (e.g. when creating a project backed by remote repositories). Within the host, this GraphDB instance can be invoked at the address `http://localhost:7200`.
Similarly, the instance linked to the development VocBench is reachable at `http://development-graphdb:7200` within the container and `http://localhost:7201` within the host.

The following command will stop the deployment:

`docker-compose stop`
