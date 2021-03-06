# Docker in Docker with Node, for CodeBuild

An unofficial Dockerfile for CodeBuild, combining the official Docker (in Docker) and Node environments.

Based on AWS' [Docker 17.09](https://github.com/aws/aws-codebuild-docker-images/blob/master/ubuntu/docker/17.09.0/Dockerfile) and [Node 6.3.1](https://github.com/aws/aws-codebuild-docker-images/blob/master/ubuntu/nodejs/6.3.1/Dockerfile) Dockerfiles.

For more information see [the official repo](https://github.com/aws/aws-codebuild-docker-images).

## Using in CodeBuild

To use this Dockerfile in your build, create a new CodeBuild project, or update an existing one. Then:

* In the _Environment: How to build_ section, select _Specify a Docker image_
* Choose _Linux_ as your _Environment type_
* Choose _Other_ for _Custom image type_
* For the _Custom image ID_, enter `tdmalone/codebuild-docker-node`
* Select the _Privileged_ checkbox

This will run your build with [this image](https://hub.docker.com/r/tdmalone/codebuild-docker-node/), which is built from this current repo.

Then, in your `buildspec.yml`, somewhere before you run or build a Docker image, make sure you execute this:

    nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://0.0.0.0:2375 --storage-driver=overlay&
    timeout 15 sh -c "until docker info; do echo .; sleep 1; done"

(This comes from [this AWS CodeBuild sample](https://docs.aws.amazon.com/codebuild/latest/userguide/sample-docker-custom-image.html)).

## Running locally

If you want to run without having to build it:

    docker run --interactive --privileged --tty tdmalone/codebuild-docker-node bash

That will download and start a container with the pre-built image, running the `bash` shell.

Otherwise, to build it yourself:

* Run `git clone https://github.com/tdmalone/codebuild-docker-node.git` to download this repository to your local machine.
* Change to the directory you cloned into, eg. `cd codebuild-docker-node`.
* Run `docker build --tag codebuild-docker-node .` to build the Docker image locally.
* Once succeeded, you can then run `docker run --interactive --privileged --tty codebuild-docker-node bash` to start the container in a `bash` shell.

You can see the [docker build](https://docs.docker.com/engine/reference/commandline/build/) and [docker run](https://docs.docker.com/engine/reference/commandline/run/) references for more.

## Changes from the originals

Based on [CodeBuild's Docker 17.09 file](https://github.com/aws/aws-codebuild-docker-images/blob/master/ubuntu/docker/17.09.0/Dockerfile), this repo:

* removes the Python installation
* removes the installation of `libmysqlclient-dev=5.5.58-*` due to error **E: Version '5.5.58-*' for 'libmysqlclient-dev' was not found**
* adds the Node 6.3.1 installation

## License

[ASL](LICENSE).
