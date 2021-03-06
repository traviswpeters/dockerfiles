# dockerfiles
A collection of Dockerfiles. *(Some borrowed and modified; some created by me.)*

Use with Docker http://www.docker.io.

See also https://hub.docker.com & https://github.com/docker-library.

## Resources

#### *Highly* Recommended Resources
* [*THE* Docker Cheatsheet :)](https://github.com/wsargent/docker-cheat-sheet)
* [Getting Started with Docker in Development](https://rock-it.pl/getting-started-with-docker-in-development/)
* [10 Docker Image Security Best Practices](https://snyk.io/blog/10-docker-image-security-best-practices/)
* [How to Write Excellent Dockerfiles](https://rock-it.pl/how-to-write-excellent-dockerfiles/)
* [Building Better Docker Images](https://jonathan.bergknoff.com/journal/building-better-docker-images/)

#### Get Images
* [Docker Hub](https://hub.docker.com/search?q=&type=image)
* [Alpine Linux](https://alpinelinux.org) is often recommended as a base image.

#### Dockerfile References & Sources
* **https://takacsmark.com/dockerfile-tutorial-by-example-dockerfile-best-practices-2018/**
* **https://github.com/jessfraz/dockerfiles**
* **https://github.com/jupyter/docker-stacks**
* **https://www.dataquest.io/blog/docker-data-science/**
* https://github.com/datascienceworkshops/dockerfiles
* https://github.com/kstaken/dockerfile-examples

#### Notebooks & Datasets
- [ ] [Seaborne - Example Gallery](https://seaborn.pydata.org/examples/index.html)
- [ ] [A gallery of interesting Jupyter Notebooks](https://github.com/jupyter/jupyter/wiki/A-gallery-of-interesting-Jupyter-Notebooks)
- [ ] [Official matplotlib gallery](https://matplotlib.org/gallery.html)
- [ ] [matplotlib-gallery](https://github.com/rasbt/matplotlib-gallery) (IPython Notebooks)
- [ ] [Estimate of Public Jupyter Notebooks on GitHub](https://github.com/parente/nbestimate)

#### Notebooks & Datasets
- [ ] [Probabilistic Programming and Bayesian Methods for Hackers](https://nbviewer.jupyter.org/github/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/blob/master/Chapter1_Introduction/Ch1_Introduction_PyMC3.ipynb)
- [ ] [Bokeh](https://nbviewer.jupyter.org/github/bokeh/bokeh-notebooks/blob/master/index.ipynb)
- [ ] [Plotly](https://nbviewer.jupyter.org/github/plotly/python-user-guide/blob/master/Index.ipynb)
- [ ] [XKCD Plots?!](https://nbviewer.jupyter.org/url/jakevdp.github.com/downloads/notebooks/XKCD_plots.ipynb)
- [ ] [Lightning + Jupyter](https://nbviewer.jupyter.org/github/lightning-viz/lightning-example-notebooks/blob/master/index.ipynb)
- [ ] [Python for Signal Processing](https://nbviewer.jupyter.org/github/unpingco/Python-for-Signal-Processing/tree/master/)
- [ ] [Current Events Analysis](https://nbviewer.jupyter.org/gist/darribas/4121857)

#### Later...

- [x] [Matplotlib tutorial: Plotting tweets mentioning Trump, Clinton, and Sanders](https://www.dataquest.io/blog/matplotlib-tutorial/)
- [ ] [Making Publication Ready Python Notebooks](http://blog.juliusschulz.de/blog/ultimate-ipython-notebook)
- [ ] [Using Interact](https://ipywidgets.readthedocs.io/en/stable/examples/Using%20Interact.html)

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

## Keyboard shortcuts
* Toggle between **edit** and **command** mode with `Esc` and `Enter`, respectively.
* Once in **command** mode:
    * Scroll up and down your cells with your Up and Down keys.
    * Press `A` or `B` to insert a new cell above or below the active cell.
    * `M` will transform the active cell to a Markdown cell.
    * `Y` will set the active cell to a code cell.
    * `D + D` (D twice) will delete the active cell.
    * `Z` will undo cell deletion.
    * Hold `Shift` and press `Up` or `Down` to select multiple cells at once.
    * With multiple cells selected, `Shift + M` will merge your selection.
* `Ctrl + Shift + -`, in edit mode, will split the active cell at the cursor.
* You can also click and `Shift + Click` in the margin to the left of your cells to select them.

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

## Working with the Data Science Containers

See: **https://github.com/jupyter/docker-stacks**.

*Building and working with the official Jupyter containers... to be run from `~/projects/dockerfiles`*
```bash
cd base-notebook && docker build -t="base-notebook" .
cd minimal-notebook && docker build -t="minimal-notebook" .
cd scipy-notebook && docker build -t="scipy-notebook" .
```

```bash
cd _research_notebook
docker build -t="my-research-notebook" .
$ docker run --rm -p 8888:8888 -v ~/projects/thesis-peters/data/:/home/twp my-research-notebook
```

```bash
# a minimal-notebook that only has Python and Jupyter installed (with ports exposed)
docker run -p 8888:8888 jupyter/minimal-notebook
# the same, but with a mounted directory (HOST_DIR:CONTAINER_DIR)
docker run -p 8888:8888 -v ~/projects/thesis-peters/data/notebooks:/home/twp jupyter/minimal-notebook
```

```bash
# install new python modules (e.g., tensorflow) from outside the container.
# NOTE: changes made in this way will not persist from container to container.
docker exec CONTAINERID pip install tensorflow
```

```bash
# This command pulls the jupyter/datascience-notebook image from Docker Hub if it is not already present on the local host.
# It then starts an ephemeral container running a Jupyter Notebook server and exposes the server on host port 10000.
# The command mounts the current working directory on the host as /home/jovyan/work in the container.
# The server logs appear in the terminal. Visiting http://<hostname>:10000/?token=<token> in a browser loads JupyterLab,
# where hostname is the name of the computer running docker and token is the secret token printed in the console.
# Docker destroys the container after notebook server exit, but any files written to ~/work in the container remain intact on the host.:
# docker run --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v ~/projects/thesis-peters/data/:/home/twp jupyter/datascience-notebook
docker run --rm -p 8888:8888 -e NB_USER=twp -e HOME=/home/$NB_USER  --user root -w /home/$NB_USER -v ~/projects/thesis-peters/data/:/home/twp jupyter/scipy-notebook start-notebook.sh
```
## Share

***There are many options for sharing notebooks...***

- https://nbviewer.jupyter.org - A simple way to share Jupyter Notebooks
(see the note about [Rendering Notebooks on GitHub
](https://blog.jupyter.org/rendering-notebooks-on-github-f7ac8736d686))
- https://mybinder.org - Turn a Git repo into a collection of interactive notebooks
- https://kyso.io - Create, upload and share your data-science notebooks, charts, datasets and articles.

***Before you share...***

A shared notebook will appear exactly in the state it was in when you export or save it, including the output of any code cells. Therefore, to ensure that your notebook is share-ready, so to speak, there are a few steps you should take before sharing:
- Click “Cell > All Output > Clear”.
- Click “Kernel > Restart & Run All”.

Wait for your code cells to finish executing and check they did so as expected.

This will ensure your notebooks don’t contain intermediary output, have a stale state, and executed in order at the time of sharing.

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
