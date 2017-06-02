# Golang S2I Builder

[![Build Status](https://travis-ci.org/ausnimbus/s2i-golang.svg?branch=master)](https://travis-ci.org/ausnimbus/s2i-golang)
[![Docker Repository on Quay](https://quay.io/repository/ausnimbus/s2i-golang/status "Docker Repository on Quay")](https://quay.io/repository/ausnimbus/s2i-golang)

This repository contains the source for the [source-to-image](https://github.com/openshift/source-to-image)
builders used to deploy [Go applications](https://www.ausnimbus.com.au/languages/golang/)
on [AusNimbus](https://www.ausnimbus.com.au/).

## Dependency Managers

- Godeps
- Govendor
- Glide
- Native

## Environment variables

- GO_PACKAGE_NAME
- GO_INSTALL_PACKAGE_SPEC
- GO_FLAGS

## Versions

The versions currently supported are:

- 1.7.6
- 1.8.3
