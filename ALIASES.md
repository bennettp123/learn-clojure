#!/bin/bash

# shell aliases for running clojure in a docker container

### . source me

    alias lein='docker run --rm -it -v `pwd`:/pwd -w /pwd clojure lein'

