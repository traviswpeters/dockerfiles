# To build an image from this Dockerfile, and start a container:
#
#   docker build -t twpeters/alpine-smarter:1.0 .
#   docker run --rm -ti twpeters/alpine-smarter:1.0 /bin/sh

FROM alpine:3.4

RUN apk update && apk add \
    curl \
    git \
    vim
