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
5. Run it: `docker-compose up -d mycms`. The `-d` detaches from the service log data; if you want, you can remove the flag to stay attached
6. Now the `mycms` service should be running, verify this by issuing `docker-compose ps`

You're done! Now you should be able to access `http://localhost:port` (the ports exposed in `docker-compose.yml`) from your browser.

## Get inside the service

This passage is not needed (in theory) but can be very useful for debugging or for "emergencies" :smile:

If you want to obtain *direct access* to the service (e.g. to make a quick edit without rebuilding and restarting it) the easiest way, until docker includes a command to do that, is to add this function to your `~/.bashrc` file:

```
## Very useful function to get inside docker containers
## NOTE: you can change "enter" to anything else, if needed
enter() {
  docker exec -ti $1 env TERM=xterm bash -l
}
```

Then *restart* your terminal (close it and reopen it), and you should be able to get inside the `mycms` service by issuing:

```
$ enter mycms
```
