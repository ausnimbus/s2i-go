# AusNimbus Builder for Golang [![Build Status](https://travis-ci.org/ausnimbus/s2i-golang.svg?branch=master)](https://travis-ci.org/ausnimbus/s2i-golang) [![Docker Repository on Quay](https://quay.io/repository/ausnimbus/s2i-golang/status "Docker Repository on Quay")](https://quay.io/repository/ausnimbus/s2i-golang)

[![Golang](https://user-images.githubusercontent.com/2239920/27288064-035dee24-5549-11e7-8ba9-7bcaa9d41055.jpg)](https://www.ausnimbus.com.au/)

The [AusNimbus](https://www.ausnimbus.com.au/) builder for Golang provides a fast, secure and reliable [Golang hosting](https://www.ausnimbus.com.au/languages/golang-hosting/) environment.

This document describes the behaviour and environment configuration when running your Golang apps on AusNimbus.

## Table of Contents

- [Runtime Environments](#runtime-environments)
- [Web Process](#web-process)
- [Dependency Management](#dependency-management)
  - [Godeps](#godeps)
  - [Govendor](#govendor)
  - [Glide](#glide)
  - [Native (go get)](#native-go-get)
- [Extending](#extending)
  - [Build Stage (assemble)](#build-stage-assemble)
  - [Runtime Stage (run)](#runtime-stage-run)
  - [Persistent Environment Variables](#persistent-environment-variables)
- [Debug Mode](#debug-mode)

## Runtime Environments

AusNimbus supports the latest stable releases.

The currently supported versions are `1.7`, `1.8` and `1.9`

## Web Process

Your application's web processes must bind to port `8080`.

AusNimbus handles SSL termination at the load balancer.

## Dependency Management

The AusNimbus builder supports a number of Dependency Managers.

The dependency manager will be automatically be selected based on the following conditions:

Dependency Manager                                | Condition
--------------------------------------------------|-------------
[Godeps](https://github.com/tools/godep)          | Used when `Godeps/Godeps.json` is found in your repository.
[Govendor](https://github.com/kardianos/govendor) | Used when `vendor/vendor.json` is found in your repository.
[Glide](https://github.com/Masterminds/glide)     | Used when when `glide.yaml` is found in your repository
Native (`go get`)                                 | Fall back if no conditions are met

### Godeps

Godeps will be executed with the install package spec set to `.`. If you would like to customize this you may use the following environment variable:

NAME                    | Description
------------------------|-------------
GO_INSTALL_PACKAGE_SPEC | Overwrite the default install package spec. Default: `.`

### Govendor

Govendor will be executed with the install package spec set to `.`. If you would like to customize this you may use the following environment variable:

NAME                    | Description
------------------------|-------------
GO_INSTALL_PACKAGE_SPEC | Overwrite the default install package spec. Default: `.`

### Glide

Glide will be executed with the install package spec set to `.`. If you would like to customize this you may use the following environment variable:

NAME                    | Description
------------------------|-------------
GO_INSTALL_PACKAGE_SPEC | Overwrite the default install package spec. Default: `.`

### Native (go get)

The standard `go get` is the fallback installation method supported by the builder.

The builder will attempt to guess your `GO_PACKAGE_NAME` based on your provided source repository. It is unable to automatically detect the package name, it will fall back to `main`.

The auto detection may be undesirable, so it is recommended you set the following environment variable:

NAME                    | Description
------------------------|-------------
GO_PACKAGE_NAME         | Your full application URI (ie. github.com/ausnimbus/golang-ex)


`go install` will be executed with the install package spec set to `.`. If you would like to customize this you may use the following environment variable:

NAME                    | Description
------------------------|-------------
GO_INSTALL_PACKAGE_SPEC | Overwrite the default install package spec. Default: `.`

## Extending

AusNimbus builders are split into two stages:

- Build
- Runtime

Both stages are completely extensible, allowing you to customize or completely overwrite each stage.

### Build Stage (assemble)

If you want to customize the build stage, you need to add the executable `.s2i/bin/assemble` file in your repository.

This file should contain the logic required to build and install any dependencies your application requires.

If you only want to extend the build stage, you may use this example:

```sh
#!/bin/bash

echo "Logic to include before"

# Run the default builder logic
. /usr/libexec/s2i/assemble

echo "Logic to include after"
```

### Runtime Stage (run)

If you only want to change the executed command for the run stage you may the following environment variable.

NAME        | Description
------------|-------------
APP_RUN     | Define a custom command to start your application. eg. `node server.js`

**NOTE:** `APP_RUN` will overwrite any builder's runtime configuration (including the [Debug Mode](#debug-mode) section)

Alternatively you may customize or overwrite the entire runtime stage by including the executable file `.s2i/bin/run`

This file should contain the logic required to execute your application.

If you only want to extend the run stage, you may use this example:

```sh
#!/bin/bash

echo "Logic to include before"

# Run the default builder logic
. /usr/libexec/s2i/run
```

As the run script executes every time your application is deployed, scaled or restarted it's recommended to keep avoid including complex logic which may delay the start-up process of your application.

### Persistent Environment Variables

The recommend approach is to set your environment variables in the AusNimbus dashboard.

However it is possible to store environment variables in code using the `.s2i/environment` file.

The file expects a key=value format eg.

```
KEY=VALUE
FOO=BAR
```
