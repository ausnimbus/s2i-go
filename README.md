# Golang S2I Docker images

[![Build Status](https://travis-ci.org/ausnimbus/s2i-golang.svg?branch=master)](https://travis-ci.org/ausnimbus/s2i-golang)
[![Docker Repository on Quay](https://quay.io/repository/ausnimbus/s2i-golang/status "Docker Repository on Quay")](https://quay.io/repository/ausnimbus/s2i-golang)

This repository contains the source for the [source-to-image](https://github.com/openshift/source-to-image)
builders used to deploy [Go applications](https://www.ausnimbus.com.au/languages/golang/)
on [AusNimbus](https://www.ausnimbus.com.au/).

The builders are built using PHP binaries from php.net

If you are interested in using SCL-based PHP binaries, use [s2i-php-scl](https://github.com/ausnimbus/s2i-php-scl)

## Versions

The versions currently supported are:

- 1.7.5
- 1.8.1
