# Docker for Visual Regression Testing (VRT)

We are using a Docker image to perform visual regression testing, so that we get consistent results regardless of 
the host-OS that is running the test. 

## Installing Docker for Mac

Follow the [instructions](https://docs.docker.com/docker-for-mac/install/#download-docker-for-mac) to install Docker for Mac

## Install the VRT image

```
docker pull uglow/backstopjs
```

**Make sure you allocate *enough memory* (more than 2GB) to Docker.** You can change this in your Docker Preferences (in OSX).


## Using the image

See [CONTRIBUTING.md](../../CONTRIBUTING.md).


The base commands in `package.json` are listed below:

```
# On OSX:
docker run --rm -v $PWD:/src/ -it --add-host dockerhost:`ipconfig getifaddr en0` uglow/backstopjs test --config=config/testVisualRegression/backstop.ci.config.js

# ON Linux:
docker run --rm -v $PWD:/src/ -it --add-host dockerhost:`/sbin/ip route|awk '/default/ { print  $3}'` uglow/backstopjs test --config=config/testVisualRegression/backstop.ci.config.js
```

The reason for two different commands is that there are OS-specific ways to get the HOST IP address and
pass it to the container.


## Debugging the image

```
docker run --rm -v $PWD:/src/ -it --entrypoint=/bin/bash uglow/backstopjs
```

## Rebuilding the image

You would only ever need to rebuild the image if you are changing it. Once you change it you also need to publish it.

### Changing the image

1. Open a terminal and navigate into the `config/docker` directory
2. Edit `Dockerfile` as required
3. Run `docker build`

### Publishing the image

Hopefully one day there will be a local AusPost repo. Until then, you will need your own Docker account to publish to
the global registry.

1. Create a Docker account on [https://hub.docker.com/](https://hub.docker.com/) 
2. Run `docker login` locally, and enter your Docker credentials
3. Run `docker image push <image>:latest`
