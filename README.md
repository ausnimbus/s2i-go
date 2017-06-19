# AusNimbus Builder for Golang [![Build Status](https://travis-ci.org/ausnimbus/s2i-golang.svg?branch=master)](https://travis-ci.org/ausnimbus/s2i-golang) [![Docker Repository on Quay](https://quay.io/repository/ausnimbus/s2i-golang/status "Docker Repository on Quay")](https://quay.io/repository/ausnimbus/s2i-golang)

[![Golang](https://user-images.githubusercontent.com/2239920/27288064-035dee24-5549-11e7-8ba9-7bcaa9d41055.jpg)](https://www.ausnimbus.com.au/)

The [AusNimbus](https://www.ausnimbus.com.au/) builder for Golang provides a fast, secure and reliable [Golang hosting](https://www.ausnimbus.com.au/languages/golang-hosting/) environment.

It supports the following dependency managers:

- [Godeps](https://github.com/tools/godep) - Used when `Godeps/Godeps.json` is found
- [Govendor](https://github.com/kardianos/govendor) - Used when `vendor/vendor.json` is found
- [Glide](https://github.com/Masterminds/glide) - Used when `glide.yaml` is found
- Native dependency management (`go get`)

Web processes must bind to port `8080`,
and only the HTTP protocol is permitted for incoming connections.

## Environment variables

* **GO_PACKAGE_NAME**
  * Your full application URI (ie. github.com/ausnimbus/golang-ex)
  * **NOTE:** Only used in native vendoring
  * Default: main

* **GO_INSTALL_PACKAGE_SPEC**
  * Overwrite the default install path
  * Default: `.`

## Versions

The versions currently supported are:

- 1.7
- 1.8
