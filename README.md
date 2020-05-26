# dp3t-docker

This is a self-contained distribution of Decentralized Privacy-Preserving Proximity Tracing (DP^3T) server components, including the CovidCode health authority code generation UI and service, packaged with Docker and orchestrated with docker-compose or Swarm. It allows you to quickly setup a server environment to test the DP^3T mobile applications.

**DISCLAIMER:** This is for testing purposes only. Don't use this in a public network.

## Quick start

First, create the secret keys and admin password with:

    make -C secrets

You may now edit `docker-compose.yml` to create a regular user for testing, if needed. (You can also connect to http://localhost:8180 later to create users in the `bag-pts` realm and add them to group `bag-pts-allowed`.)

To deploy the server cluster on localhost run:

    docker-compose up

then connect to https://localhost for the public interface. Login, generate a code, and use it as follows:

    ./tests/post.sh 2020-05-21 123 123 123 123

giving the correct data and code. To list exposed buckets for today use:

    ./tests/get.sh

## Building

Check-out the project with:

    git clone --recurse-submodules https://github.com/stayawayinesctec/dp3t-docker

or just add the submodules to an existing clone with:

    git submodule update --init

Build all images with:

    docker-compose build

## Customization

This project is hardcoded for https://localhost. If you want to use something else, just find and replace https://localhost everywhere before rebuilding it, namely:

    ./frontend/environment.prod.ts
    ./frontend/Caddyfile
    ./authcodews/application.yml
    ./keycloak/create.sh
    ./tests/get.sh
    ./tests/post.sh

Many parameters of each web-service can be changed in application properties files in `./authcodews` and `./backendws`.

Secrets (keys and passwords) are managed as volumes mounted from the `./secrets` folder. You cam also opt to deploy in swarm-mode using `docker-compose-swarm.yml` after loading secrets with:

    make -C secrets load

## More information

The DP3T project is available at: https://github.com/DP-3T

The CovidCode service and UI are available at: https://github.com/admin-ch

This setup has been packaged by the [STAYAWAY team](https://stayaway.inesctec.pt) at [INESCTEC](https://inesctec.pt). It is available at https://github.com/stayawayinesctec/dp3t-docker
