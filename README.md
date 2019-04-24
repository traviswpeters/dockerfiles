# dockerfiles
A collection of Dockerfiles. *(Some borrowed and modified; some created by me.)*

Use with Docker http://www.docker.io.

**References:**
* **https://takacsmark.com/dockerfile-tutorial-by-example-dockerfile-best-practices-2018/**
* **https://www.dataquest.io/blog/docker-data-science/**
* **https://github.com/jessfraz/dockerfiles**
* https://github.com/datascienceworkshops/dockerfiles
* https://github.com/kstaken/dockerfile-examples

See also https://github.com/docker-library.

## Background

A **Dockerfile** is a text file that defines a Docker image.

You'll use a Dockerfile to create your own custom Docker image
(i.e., to define your custom environment to be used in a Docker container.)

A Docker image is created by building a Dockerfile with the `docker build` command.

From a developer perspective, a Dockerfile is nothing more than a description of the build steps of your environment.

**Docker containers (containers)** are runtime environments.
You usually run one main process in one Docker container.
You can think of this like one Docker container provides one service in your project.

For example you can start one container to be your MySQL database
and start another container to be your Wordpress server
and connect these containers together to get a Wordpress project setup.

You can start containers to run all the tech you can think of,
you can run databases, web servers, web frameworks, test servers, execute big data scripts, work on shell scripts, etc.

Docker containers are started by running a **Docker image**. A Docker image is a pre-built environment for a certain technology or service. A Docker image is not a runtime, it’s rather a collection of files, libraries and configuration files that build up an environment.

## Building Containers from Images

To build an image with docker is pretty simple.

For example:
```bash
cd rethinkdb
docker build -t="rethinkdb" .
```

Then to run that image and attach to it at the same time:
```bash
docker run --rm --interactive --tty rethinkdb
```

Or to run it in the background
```bash
docker run -d rethinkdb
```

## Dockerfile Building Workflow

I use a fairly straightforward 4 step approach to build my Dockerfiles in an iterative manner. It is as follows:

1. **Pick the right base image** -
    In this step I experiment with the base images available on-line for the technology in question.
    I usually check out different flavors, like an image based on Debian Jessie and another on based on Alpine.

    I also check out the images made by others for a specific technology.
    For node.js, for example, I’m not building my own images from a Linux base, I usually use the offical node images as a base image.

    When working with php, I usually start from php with the Apache web server included and add my stuff myself.

2. **Go to the shell and build your environment** -
    As a next step I go with a try-and-fail approach.
    I pull my chosen images to my computer and start a container in interactive mode with a shell.
    E.g.,
    ```bash
    docker run --rm -ti IMAGE /bin/sh
    ```

    I start manually executing the steps in the container and see how things work out. Once a step seems to be OK, I add it to my Dockerfile.

    If something goes wrong, I change the course, and I update the Dockerfile immediately.

3. **Add the steps to your Dockerfile and build your image** -
    I keep adding steps continuously as I make progress with my setup in the container.

    Every now and then I stop and build my image from the Dockerfile to make sure that it produces the same results every time.

    Then I use the newly built image to start a container with a shell and go on with my installation and set-up steps.

4. **Repeat steps 2 and 3** -
    I keep repeating steps 2 and 3 until I reach the stage that I like.

## Basic Dockerfile commands

We’ll cover the following basic instructions to get you started:

```bash
FROM - every Dockerfile starts with FROM, with the introduction of multi-stage builds as of version 17.05, you can have more than one FROM instruction in one Dockerfile.
COPY vs ADD - these two are often confused, so I’ll explain the difference.
ENV - well, setting environment variables is pretty important.
RUN - let’s run commands.
VOLUME - another source of confusion, what’s the difference between Dockerfile VOLUME and container volumes?
USER - when root is too mainstream.
WORKDIR - set the working directory.
EXPOSE - get your ports right.
ONBUILD - give more flexibility to your team and clients.
```

## Notes on VOLUMEs

You can use the VOLUME instruction in a Dockerfile to tell Docker that the stuff you store in that specific directory should be stored on the host file system not in the container file system. This implies that stuff stored in the volume will persist and be available also after you destroy the container.

*In other words it is best practice to crate a volume for your data files, database files, or any file or directory that your users will change when they use your application.*

The data stored in the volume will remain on the host machine even if you stop the container and remove the container with `docker rm`. (The volume will be removed on exit if you start the container with docker `run --rm`, though.)

You can inspect your volumes with the `docker volume ls` and `docker volume inspect` commands.

The difference between the `VOLUME` instruction in Dockerfile and starting your container with docker run -v ... is this:
`VOLUME` in Dockerfile will create a new empty directory for your files under the standard Docker structure,
i.e. `/var/lib/docker/volumes`.
`docker run -v ...` can do more---you can mount existing directories from your host file system into your container
and you can also specify the path of the directory on the host.

Now you may think that `docker run -v ...` is the better option,
but keep in mind that mounting an existing directory assumes that the directory exists on the host,
which may give you portability issues.

## Tips

#### Minimize the number of steps in the Dockerfile.

#### Sort multi-line instructions.

Start your Dockerfile with the steps that are least likely to change.
The best practice is to structure your Dockerfile according to the following:
1. Install tools that are needed to build your application.
2. Install dependencies, libraries and packages.
3. Build your application.

#### Use a .dockerignore file.

The directory where you issue the docker build command is called the build context. Docker will send all of the files and directories in your build directory to the Docker daemon as part of the build context. If you have stuff in your directory that is not needed by your build, you’ll have an unnecessarily larger build context that results in a larger image size.

You can remedy this situation by adding a .dockerignore file that works similarly to .gitignore. You can specify the list of folders and files that should be ignored in the build context.

If you want to have a look at the size of your build context, just check out the first line of your docker build output.

#### Containers should be ephemeral

This would belong to generic Docker guidelines, but it’s never enough to stress this point. It is your best interest to design and build Docker images that can be destroyed and recreated/replaced automatically or with minimal configuration.

Which means that you should create Dockerfiles that define stateless images. Any state, should be kept outside of your containers.

#### One container should have one concern
Think of containers as entities that take responsibility for one aspect of your project.
So design your application in a way that your web server, database, in-memory cache and other components have their own dedicated containers.

You’ll see the benefits of such a design when scaling your app horizontally.

It’s a good idea to check out the official Dockerfile best practices page for more info: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
