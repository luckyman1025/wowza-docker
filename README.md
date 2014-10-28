# Table of Contents
- [Introduction](#introduction)
    - [Version](#version)
    - [Changelog](Changelog.md)
- [Contributing](#contributing)
- [Issues](#issues)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Data Store](#data-store)
- [Shell Access](#shell-access)
- [Upgrading](#upgrading)
- [References](#references)

# Introduction

Dockerfile to build a [Wowza Streaming Engine](http://www.wowza.com/products/streaming-engine) server.

**NOTE**: By using this image you are agreeing to comply with the [Wowza EULA](Wowza_EULA.txt)

## Version

Current Version: **4.1.0**

# Contributing

If you find this image useful here's how you can help:

- Send a Pull Request with your awesome new features and bug fixes
- Help new users with [Issues](https://github.com/sameersbn/docker-wowza/issues) they may encounter
- Send me a tip on [Gittip](https://gittip.com/sameersbn/) or using Bitcoin at **16rDxVqJPyYAFYPLduTaSiwe7ZiY1hHqKM**

# Issues

Docker is a relatively new project and is active being developed and tested by a thriving community of developers and testers and every release of docker features many enhancements and bugfixes.

Given the nature of the development and release cycle it is very important that you have the latest version of docker installed because any issue that you encounter might have already been fixed with a newer docker release.

For ubuntu users I suggest [installing docker](https://docs.docker.com/installation/ubuntulinux/) using docker's own package repository since the version of docker packaged in the ubuntu repositories are a little dated.

Here is the shortform of the installation of an updated version of docker on ubuntu.

```bash
sudo apt-get purge docker.io
curl -s https://get.docker.io/ubuntu/ | sudo sh
sudo apt-get update
sudo apt-get install lxc-docker
```

Fedora and RHEL/CentOS users should try disabling selinux with `setenforce 0` and check if resolves the issue. If it does than there is not much that I can help you with. You can either stick with selinux disabled (not recommended by redhat) or switch to using ubuntu.

If using the latest docker version and/or disabling selinux does not fix the issue then please file a issue request on the [issues](https://github.com/sameersbn/docker-wowza/issues) page.

In your issue report please make sure you provide the following information:

- The host ditribution and release version.
- Output of the `docker version` command
- Output of the `docker info` command
- The `docker run` command you used to run the image (mask out the sensitive bits).

# Installation

Pull the the image from the docker index. This is the recommended method of installation as it is easier to update image. These builds are performed by the **Docker Trusted Build** service.

```bash
docker pull sameersbn/wowza:4.1.0
```

You can also pull the `latest` tag which is built from the repository *HEAD*

```bash
docker pull sameersbn/wowza:latest
```

Alternately you can build the image locally.

```bash
git clone https://github.com/sameersbn/docker-wowza.git
cd docker-wowza
docker build --tag="$USER/wowza" .
```

# Quick Start

Before you can start using this image you need to acquire a license for the Wowza Streaming Engine. If you do not have one, you can [request a free trial license](http://www.wowza.com/pricing/trial).

```bash
docker run --name='wowza' -it --rm \
  -e 'WOWZA_KEY=xxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxx' \
  -p 1935:1935 -p 8086:8086 -p 8087:8087 -p 8088:8088 \
  sameersbn/wowza:4.1.0
```

Point your browser to `http://localhost:8088` and login using the default username and password:

* username: **admin**
* password: **admin**

Refer to the wowza [quickstart guide](http://www.wowza.com/forums/content.php?3-quick-start-guide) for wowza configuration instructions.

# Data Store

The wowza image is configured to save all configurations at `/data`. As such we should mount a volume at `/data`.

```bash
docker run --name='wowza' -it --rm \
  -e 'WOWZA_KEY=xxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxx' \
  -p 1935:1935 -p 8086:8086 -p 8087:8087 -p 8088:8088 \
  -v /opt/wowza:/data \
  sameersbn/wowza:4.1.0
```

Upon the first run the image will copy all configurations at this location allowing users to manually edit the configurations if required.

# Shell Access

For debugging and maintenance purposes you may want access the containers shell. If you are using docker version `1.3.0` or higher you can access a running containers shell using `docker exec` command.

```bash
docker exec -it wowza bash
```

If you are using an older version of docker, you can use the [nsenter](http://man7.org/linux/man-pages/man1/nsenter.1.html) linux tool (part of the util-linux package) to access the container shell.

Some linux distros (e.g. ubuntu) use older versions of the util-linux which do not include the `nsenter` tool. To get around this @jpetazzo has created a nice docker image that allows you to install the `nsenter` utility and a helper script named `docker-enter` on these distros.

To install `nsenter` execute the following command on your host

```bash
docker run --rm -v /usr/local/bin:/target jpetazzo/nsenter
```

Now you can access the container shell using the command

```bash
sudo docker-enter wowza
```

For more information refer https://github.com/jpetazzo/nsenter

# Upgrading

To upgrade to newer releases, simply follow this 3 step upgrade procedure.

- **Step 1**: Pull the latest version from the docker index

```bash
docker pull sameersbn/wowza:4.1.0
```

- **Step 2**: Stop and remove the running container

```bash
docker stop wowza
docker rm wowza
```

- **Step 3**: Start the updated image

```bash
docker run --name=wowza -d [OPTIONS] sameersbn/wowza:4.1.0
```

# References

  * http://www.wowza.com/products/streaming-engine
