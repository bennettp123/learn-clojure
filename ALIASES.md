#!/bin/bash

# shell aliases for running clojure in a docker container

### dot-source me

    echo $BASH_SOURCE

    # spin up a docker with access to the current directory
    alias dockerme='docker run --rm -it -v `pwd`:/cwd -w /cwd'
    
    # base docker images for clojure and java
    alias clojureme='dockerme clojure:lein-alpine'
    alias javame='dockerme openjdk:jre-alpine'

    # convenience commands
    alias lein='clojureme lein'
    alias java='javame java'
    
    # build image bennetts-emacs unless it exists
    build_emacs() {
            if [ -z "$BASH_SOURCE" ]; then
                echo "docker image bennetts-emacs is missing" >&2
                return 1
            else
                pushd . && cd "$(dirname "$BASH_SOURCE"/emacs)"
                docker build -t bennetts-emacs .
                RETVAL=$?
                popd
                return $RETVAL
            fi
    }

    # emacs environment
    emacsme() {
        if ! docker inspect bennetts-emacs >/dev/null 2>&1; then
            build_emacs && dockerme bennetts-emacs "$@"
        else
            dockerme bennetts-emacs "$@"
        fi
    }

