# cms-docker

This repository aims to ease the process of building and running the [CMS](http://cms-dev.github.io/) software and its variations, like [CMSocial](https://github.com/algorithm-ninja/cmsocial).

## Requisites

* [docker](https://docs.docker.com/engine/installation/)
* [docker-compose](https://docs.docker.com/compose/install/)

## Building and running

1. Clone this repository
2. `cd cms-docker`
3. Create a `docker-compose.yml` file. You can use the existing `docker-compose.*.yml` as templates, by removing/adding/editing the existing services. You should make sure that the container's `/usr/local/etc/` folder correctly maps to a folder on the docker host which contains valid `cms.conf` and `cms.ranking.conf` files
4. Let's say that you named your service `mycms`. Build it: `docker-compose build mycms`
5. Run it: `docker-compose up --detach`
