#!/bin/bash

# shell aliases for running clojure in a docker container

### . source me

    alias dockerme='docker run --rm -it -v `pwd`:/cwd -w /cwd'
    alias lein='dockerme clojure:lein-alpine lein'
    alias java='dockerme openjdk:jre-alpine java'

