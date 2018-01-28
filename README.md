# Docker in Docker with Node, for CodeBuild

An unofficial Dockerfile for CodeBuild, combining official Docker (in Docker) and Node environments.

Based on the [Docker 17.09](https://github.com/aws/aws-codebuild-docker-images/blob/master/ubuntu/docker/17.09.0/Dockerfile) and [Node 6.3.1](https://github.com/aws/aws-codebuild-docker-images/blob/master/ubuntu/nodejs/6.3.1/Dockerfile) Dockerfiles.

For more information see [the official repo](https://github.com/aws/aws-codebuild-docker-images).

## Using in CodeBuild

Create a new project, or update an existing one. Then:

* In the _Environment: How to build_ section, select _Specify a Docker image_
* Choose _Linux_ as your _Environment type_
* Choose _Other_ for _Custom image type_
* For the _Custom image ID_, enter `tdmalone/codebuild-docker-node`

This will run your build with [this image](https://hub.docker.com/r/tdmalone/codebuild-docker-node/), which is built from this current repo.

## Building locally

* Run `git clone https://github.com/tdmalone/codebuild-docker-node.git` to download this repository to your local machine.
* Run `docker build --tag codebuild-docker-node .` to build Docker image locally.
* Once succeeded, your can run `docker run --interactive --tty codebuild-docker-node bash` to start container running the `bash` shell.

You can see the [docker build](https://docs.docker.com/engine/reference/commandline/build/) and [docker run](https://docs.docker.com/engine/reference/commandline/run/) references for more.

## Changes from the originals

Based on the Docker 17.09 file, this file:

* removes the Python installation
* removes the installation of `libmysqlclient-dev=5.5.58-*` due to **E: Version '5.5.58-*' for 'libmysqlclient-dev' was not found**
* adds the Node 6.3.1 installation

## License

[ASL](LICENSE).
