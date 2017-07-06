# p6-rakudo-docker

# SYNOPSIS

Build scripts to build and manage Rakudo/Perl 6 docker images.

# USAGE

    ./build -v <version> -r [rakudo|rakudo-star]
             [-d <debian_release>] [-f <image_name>] [-t <tag>]

      -r  - Type of rakudo build.
            'rakudo-star' is one of the Rakduo stable releases.
            'rakudo' is one of the monthly builds.
      -v  - Version of Rakudo to build e.g. 2017-06
      -n  - Name to be given to the image. e.g. myfoo/rakudo-star
      -t  - Tag to apply to the image. e.g. latest
      -f  - Parent image (FROM) to use as base
      -d  - Debian release to use. e.g. 'jessie', 'stretch', 'buster', etc
            This chooses an appropriate parent image as well as set the release
            within the container environment.

      e.g.
        $ ./build -r rakudo -v 2017-06
        ...

        $ docker images | grep -iE 'IMAGE ID|rakudo'
        REPOSITORY      TAG             IMAGE ID      CREATED     SIZE
        shalomb/rakudo  2017.06         98cd7c20843b  3 mins ago  378MB
        shalomb/rakudo  20170628T181702 98cd7c20843b  3 mins ago  378MB
        shalomb/rakudo  latest          98cd7c20843b  3 mins ago  378MB
        shalomb/rakudo  20170628T181505 9d18b25a2a8c  4 hours ago 374MB
        shalomb/rakudo  20170628T181253 d38edd736744  4 hours ago 369MB
        ...

     ./run shalomb/rakudo:2017.06 perl6 -e '.say for 0..10'

# DESCRIPTION
These scripts cater to the build and management of a catalogue of 
Rakudo / Perl 6 docker images of different versions.

`./build` builds a [Rakudo](http://rakudo.org/) Debian-based docker image with
the [zef](https://github.com/ugexe/zef) module manager and a few other utilities
to make the image usable with Debian (and related) expectations - e.g. beside a
regular Debian host install. (Alpine Linux may be cool but it is deficient
for non-production use-cases where you often need access to the wider GNU/Linux
ecosystem).

As `./build` takes a rakudo version to build - it is possible to create/maintain
docker images for different versions of Rakudo - making switching between
versions trivial. This can be useful in learning, dev/test, etc environments.

`./run` instantiates a docker instance that is run in the same PID space as the
host with appropriate volumes mounted making interactions with the host
transparent. i.e. running a containerized rakudo against code on the host
is as trivial as

    (docker host) $ ./run shalomb/rakudo:2017.06 perl6 $PWD/../path/to/script.p6

# TODO

* Manage zef module installations on a shared volume?

