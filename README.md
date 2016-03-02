# Golang S2I Docker images

[![Build Status](https://travis-ci.org/ausnimbus/s2i-golang.svg?branch=master)](https://travis-ci.org/ausnimbus/s2i-golang)

This repository contains the source for building various versions of
the Golang application as a reproducible Docker image
[source-to-image](https://github.com/openshift/source-to-image)
to be run on [AusNimbus](https://www.ausnimbus.com.au/).

Images are built with Golang binary packages.
The resulting image can be run using [Docker](http://docker.io).

## Versions

The versions currently supported are:

- 1.7
- 1.8
