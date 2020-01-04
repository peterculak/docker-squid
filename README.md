# Introduction

`Dockerfile` to create a [Docker](https://www.docker.com/) container image for [Squid proxy server](http://www.squid-cache.org/).

Squid is a caching proxy for the Web supporting HTTP, HTTPS, FTP, and more. It reduces bandwidth and improves response times by caching and reusing frequently-requested web pages. Squid has extensive access controls and makes a great server accelerator.

# Getting started

## Installation

Automated builds of the image are available on [Dockerhub](https://hub.docker.com/r/fr0z3nk0/squid) and is the recommended method of installation.

```bash
docker pull docker pull fr0z3nk0/squid:latest
```

## Quickstart

Start Squid using:

```bash
docker run --name squid -d --restart=always \
  --publish 3128:3128 \
  --volume /srv/docker/squid/cache:/var/spool/squid \
  fr0z3nk0/squid:latest
```

## Command-line arguments

You can customize the launch command of the Squid server by specifying arguments to `squid` on the `docker run` command. For example the following command prints the help menu of `squid` command:

```bash
docker run --name squid -it --rm \
  --publish 3128:3128 \
  --volume /srv/docker/squid/cache:/var/spool/squid \
  fr0z3nk0/squid:latest
```

## Persistence

For the cache to preserve its state across container shutdown and startup you should mount a volume at `/var/spool/squid`.

> *The [Quickstart](#quickstart) command already mounts a volume for persistence.*

## Configuration

Squid is a full featured caching proxy server and a large number of configuration parameters. To configure Squid as per your requirements mount your custom configuration at `/etc/squid/squid.conf`.

```bash
docker run --name squid -d --restart=always \
  --publish 3128:3128 \
  --volume /path/to/squid.conf:/etc/squid/squid.conf \
  --volume /srv/docker/squid/cache:/var/spool/squid \
  fr0z3nk0/squid:latest
```

To reload the Squid configuration on a running instance you can send the `HUP` signal to the container.

```bash
docker kill -s HUP squid
```

## Usage

Configure your web browser network/connection settings to use the proxy server which is available at `127.0.0.1:3128`

## Logs

To access the Squid logs, located at `/var/log/squid/`, you can use `docker exec`. For example, if you want to tail the access logs:

```bash
docker exec -it squid tail -f /var/log/squid/access.log
```

You can also mount a volume at `/var/log/squid/` so that the logs are directly accessible on the host.

# Maintenance

## Upgrading

To upgrade to newer releases:

  1. Download the updated Docker image:

  ```bash
  docker pull fr0z3nk0/squid:latest
  ```

  2. Stop the currently running image:

  ```bash
  docker stop squid
  ```

  3. Remove the stopped container

  ```bash
  docker rm -v squid
  ```

  4. Start the updated image

  ```bash
  docker run -name squid -d \
    [OPTIONS] \
    fr0z3nk0/squid:latest
  ```

## Shell Access

For debugging and maintenance purposes you may want access the containers shell. If you are using Docker version `1.3.0` or higher you can access a running containers shell by starting `bash` using `docker exec`:

```bash
docker exec -it squid bash
```