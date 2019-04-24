# dockerfiles
A collection of Dockerfiles.

Use with Docker http://www.docker.io.

*NOTE: some have been borrowed and modified; some were created by me.*

**Sources:**
* https://github.com/datascienceworkshops/dockerfiles
* https://github.com/kstaken/dockerfile-examples

## Building Containers from Images

For example, to build an image with docker is pretty simple:
```bash
cd rethinkdb
docker build -t="rethinkdb" .
```

Then to run that image and attach to it at the same time:
```bash
docker run -i -t rethinkdb
```

Or to run it in the background
```bash
docker run -d rethinkdb
```
