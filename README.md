# poc-origin-pingpong-config

Shamelessly borrowed from [Spring Cloud Ping-Pong Example] (https://github.com/bijukunjummen/spring-cloud-ping-pong-sample).

The originating project has been modified to deploy to OpenShift Origin (v3) as separate containers (hence isolated git repos).  This repo has an automated build process that triggers after a push to regenerate a docker image saved to dockerhub.

This project is based on spring cloud, spring boot, and eureka.  Hysterix is included (refer to monitor project).

Five, separate repositories are used for the example (by deployment order):

1. [poc-origin-pingpong-eureka](https://github.com/todd-fritz/poc-origin-pingpong-eureka)

2. [poc-origin-pingpong-config](https://github.com/todd-fritz/poc-origin-pingpong-config)

3. [poc-origin-pingpong-monitor](https://github.com/todd-fritz/poc-origin-pingpong-monitor)

4. [poc-origin-pingpong-ping](https://github.com/todd-fritz/poc-origin-pingpong-ping)

5. [poc-origin-pingpong-pong](https://github.com/todd-fritz/poc-origin-pingpong-pong)


NOTE:  This assumes the Eureka container has been build and is running.  As such, instructions container within this file
pertain only to the config container.

## Running on a Local Machine
Running it all local is simple, do the following in sequence, in four different terminal windows:



*Run the Config server (source,java)

`cd poc-origin-pingpong-config`

`mvn spring-boot:run`


If all the applications have come up cleanly, the endpoint should be available at http://localhost:8080


Creating and pushing a new release to Dockerhub:

`eval "$(docker-machine env default)`

`mvn clean package docker:build`

`docker push tfritz/poc-origin-pingpong-config`

If running via Docker toolbox, forward port 8888 (leave host and guest host info, empty).

`docker run -d -p 8888:8888 -t --name poc-config --hostname poc-config --link poc-eureka:link-config2eureka tfritz/poc-origin-pingpong-config:1.0.0-SNAPSHOT`


Interactive run mode for debugging only:

`docker run -it --rm -p 8888:8888 --name poc-config --hostname poc-config --link poc-eureka:link-config2eureka tfritz/poc-origin-pingpong-config:1.0.0-SNAPSHOT`


Notes:
https://github.com/spotify/docker-maven-plugin#specify-build-info-in-the-pom
https://docs.docker.com/installation/mac/
https://github.com/docker/docker/issues/2509