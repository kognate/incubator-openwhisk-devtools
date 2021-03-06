# How to setup OpenWhisk with Docker Compose

[![Build Status](https://travis-ci.org/apache/incubator-openwhisk-devtools.svg?branch=master)](https://travis-ci.org/apache/incubator-openwhisk-devtools)

An easy way to try OpenWhisk locally is to use Docker Compose.

#### Prerequisites

The following are required to build and deploy OpenWhisk with Docker Compose:

- [Docker 1.12+](https://www.docker.com/products/docker)
- [Docker Compose 1.6+](https://docs.docker.com/compose/install/)
- [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

Available Ports:

- `5984` for CouchDB
- `2181` for Zookeeper
- `9092` for Kafka
- `8400`, `8500`, `8600`, `8302` for Consul
- `8888` for OpenWhisk's Controller
- `8085` for OpenWhisk's Invoker
- `80` and `443` for the API Gateway

# Quick Start

```bash
make quick-start
```

This command downloads the `master` branch from the [OpenWhisk](https://github.com/openwhisk/openwhisk) repo, it  builds OpenWhisk, the docker containers, it starts the system and it executes a simple `hello-world` function.
At the end of the execution it prints the output of the function:
```javascript
{
    "payload": "Hello, World!"
}
```

If `PROJECT_HOME` variable is set ( i.e. `PROJECT_HOME=/path/to/openwhisk make quick-start`)
then the command skips downloading the `master` branch and uses instead the source code found in the `PROJECT_HOME` folder.
This is useful for working with a local clone, making changes to the code, and run it with `docker-compose`.

# Build

```bash
make docker
```

This command builds the docker containers for local testing and development.

> NOTE: The build may skip some components such as Swift actions in order to finish the build faster.

# Start

```bash
make run
```

This command starts OpenWhisk by calling `docker-compose up`, it initializes the database and the CLI.

# Stop

The following command stops the `docker-compose`:

```bash
make stop
```

To remove the stopped containers, clean the database files and the temporary files use:

 ```bash
 make destroy
 ```

# Running a hello-world function

Once OpenWhisk is up and running you can execute a `hello-world` function:

```bash
make hello-world
```

This command creates a new JS action, it invokes it, and then it deletes it.
  The javascript action is:
```javascript
function main(params) {
    var name = params.name || "World";
    return {payload: "Hello, " + name + "!"};
}
```
The result of the invokation should be printed on the terminal:
```
{
    "payload": "Hello, World!"
}
```

## Logs

- OpenWhisk Controller - `~/tmp/openwhisk/controller/logs/`
- OpenWhisk Invoker - `~/tmp/openwhisk/invoker/logs/`
- `docker-compose` logs - `~/tmp/openwhisk/docker-compose.log`


## Custom Docker containers for actions

To start `docker-compose` with custom images used for running actions use the following 2 variables:

- `DOCKER_REGISTRY` - specify a custom docker registry. I.e ```DOCKER_REGISTRY=registry.example.com make quick-start```
- `DOCKER_IMAGE_PREFIX` - specify a custom image prefix. I.e. ```DOCKER_IMAGE_PREFIX=my-prefix make quick-start```

These 2 variable allow you to execute a JS action using the container `registry.example.com/my-prefix/nodejs6action`.
