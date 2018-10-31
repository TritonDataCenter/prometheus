# Prometheus

This repository represents the version of
[Prometheus](https://github.com/prometheus/prometheus) that is used as part of
[Joyent Triton](https://github.com/joyent/triton).

This fork replaces Prometheus's vendored version of fsnotify with
[our fsnotify fork](https://github.com/joyent/fsnotify), which adds FEN support
to allow illumos functionality.

## Repository Management

This repository is downstream of
[Prometheus](https://github.com/prometheus/prometheus).

To better understand and maintain our differences from Prometheus, we try to
manage branches and tags in a specific fashion. First and foremost, all
branches and tags from the upstream Prometheus repository are mirrored here.

Anything that is Joyent-specific begins with a `joyent/` prefix.

Branches with Joyent modifications are named `joyent/<version>`, such as
`joyent/2.5`. This is a branch that tracks the Prometheus
`2.5` branch. These branches will have all of our patches
rebased on top of them. Currently, this repository is consumed by
`triton-prometheus`, which includes a submodule for this repository. The
submodule version will be based on a tag in this repository that uses the form
`joyent/v<version>j<branch release num>`. Note that Prometheus uses a branch per
secondary version number (e.g. `2.5`), and assigns multiple tags and releases
with tertiary version numbers (e.g. `2.5.0`) from each branch. Thus, assuming
a given Joyent release were based on version `2.5.0`, the release tag would be:
`joyent/v2.5.0j1`. If we need to cut another release
from this upstream release, we would tag it `joyent/v2.5.0j2` and continue to
increment the number after the `j`. Note we use the `j` instead of `r`
which would more traditionally be used to indicate a revision.  We use
`j` in case Prometheus for some reason wants to use `r` in its version strings.

When it comes time to update to a newer version of Prometheus, we would take
the following steps:

* Ensure that we have pushed all changes from `prometheus/prometheus` and synced
  all of our branches and tags.
* Identify the release tag that corresponds to the point release. For
  this example, we'll say that's `v2.5.0`.
* Create a new branch named `joyent/<version>` from the tag. In this
  case we would name the branch `joyent/2.5` to match Prometheus's naming
  scheme.
* Rebase all of our patches on to that new branch, removing any patches
  that are no longer necessary.
* Test the new version of Prometheus.
* Review and Commit all relevant changes.
* Create a new tag `joyent/v2.5.0j1`.
* Update [triton-prometheus](https://github.com/joyent/triton-prometheus) to
  point to the new tag.

# Prometheus [![Build Status](https://travis-ci.org/prometheus/prometheus.svg)][travis]

[![CircleCI](https://circleci.com/gh/prometheus/prometheus/tree/master.svg?style=shield)][circleci]
[![Docker Repository on Quay](https://quay.io/repository/prometheus/prometheus/status)][quay]
[![Docker Pulls](https://img.shields.io/docker/pulls/prom/prometheus.svg?maxAge=604800)][hub]
[![Go Report Card](https://goreportcard.com/badge/github.com/prometheus/prometheus)](https://goreportcard.com/report/github.com/prometheus/prometheus)
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/486/badge)](https://bestpractices.coreinfrastructure.org/projects/486)

Visit [prometheus.io](https://prometheus.io) for the full documentation,
examples and guides.

Prometheus, a [Cloud Native Computing Foundation](https://cncf.io/) project, is a systems and service monitoring system. It collects metrics
from configured targets at given intervals, evaluates rule expressions,
displays the results, and can trigger alerts if some condition is observed
to be true.

Prometheus' main distinguishing features as compared to other monitoring systems are:

- a **multi-dimensional** data model (timeseries defined by metric name and set of key/value dimensions)
- a **flexible query language** to leverage this dimensionality
- no dependency on distributed storage; **single server nodes are autonomous**
- timeseries collection happens via a **pull model** over HTTP
- **pushing timeseries** is supported via an intermediary gateway
- targets are discovered via **service discovery** or **static configuration**
- multiple modes of **graphing and dashboarding support**
- support for hierarchical and horizontal **federation**

## Architecture overview

![](https://cdn.jsdelivr.net/gh/prometheus/prometheus@c34257d069c630685da35bcef084632ffd5d6209/documentation/images/architecture.svg)

## Install

There are various ways of installing Prometheus.

### Precompiled binaries

Precompiled binaries for released versions are available in the
[*download* section](https://prometheus.io/download/)
on [prometheus.io](https://prometheus.io). Using the latest production release binary
is the recommended way of installing Prometheus.
See the [Installing](https://prometheus.io/docs/introduction/install/)
chapter in the documentation for all the details.

Debian packages [are available](https://packages.debian.org/sid/net/prometheus).

### Docker images

Docker images are available on [Quay.io](https://quay.io/repository/prometheus/prometheus).

You can launch a Prometheus container for trying it out with

    $ docker run --name prometheus -d -p 127.0.0.1:9090:9090 quay.io/prometheus/prometheus

Prometheus will now be reachable at http://localhost:9090/.

### Building from source

To build Prometheus from the source code yourself you need to have a working
Go environment with [version 1.10 or greater installed](http://golang.org/doc/install).

You can directly use the `go` tool to download and install the `prometheus`
and `promtool` binaries into your `GOPATH`:

    $ go get github.com/prometheus/prometheus/cmd/...
    $ prometheus --config.file=your_config.yml

You can also clone the repository yourself and build using `make`:

    $ mkdir -p $GOPATH/src/github.com/prometheus
    $ cd $GOPATH/src/github.com/prometheus
    $ git clone https://github.com/prometheus/prometheus.git
    $ cd prometheus
    $ make build
    $ ./prometheus --config.file=your_config.yml

The Makefile provides several targets:

  * *build*: build the `prometheus` and `promtool` binaries
  * *test*: run the tests
  * *test-short*: run the short tests
  * *format*: format the source code
  * *vet*: check the source code for common errors
  * *assets*: rebuild the static assets
  * *docker*: build a docker container for the current `HEAD`

## More information

  * The source code is periodically indexed: [Prometheus Core](http://godoc.org/github.com/prometheus/prometheus).
  * You will find a Travis CI configuration in `.travis.yml`.
  * See the [Community page](https://prometheus.io/community) for how to reach the Prometheus developers and users on various communication channels.

## Contributing

Refer to [CONTRIBUTING.md](https://github.com/prometheus/prometheus/blob/master/CONTRIBUTING.md)

## License

Apache License 2.0, see [LICENSE](https://github.com/prometheus/prometheus/blob/master/LICENSE).


[travis]: https://travis-ci.org/prometheus/prometheus
[hub]: https://hub.docker.com/r/prom/prometheus/
[circleci]: https://circleci.com/gh/prometheus/prometheus
[quay]: https://quay.io/repository/prometheus/prometheus
