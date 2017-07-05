#!/bin/bash

# shell aliases for running clojure in a docker container

### dot-source me

    # spin up a docker with access to the current directory
    alias dockerme='docker run --rm -it -v `pwd`:/cwd -w /cwd'
    
    # base docker images for clojure and java
    alias clojureme='dockerme clojure:lein-alpine'
    alias javame='dockerme openjdk:jre-alpine'

    # convenience commands
    alias lein='clojureme lein'
    alias java='javame java'
    
    # stash BASH_SOURCE into a persistent variable, for later
    if ! [ -z "$BASH_SOURCE" ]; then
        MY_CLOSURE_ALIAS_DIR="$(cd "$(dirname "$BASH_SOURCE")"; pwd)"
    fi

    # build image bennetts-emacs unless it exists
    build_emacs() {
            if [ -z "$MY_CLOSURE_ALIAS_DIR" ]; then
                echo "docker image bennetts-emacs is missing" >&2
                return 1
            else
                pushd . || return 1
                cd "${MY_CLOSURE_ALIAS_DIR}/emacs" \
                    && docker build -t bennetts-emacs .
                RETVAL=$?
                popd
                return $RETVAL
            fi
    }

    # emacs environment
    emacsme() {
        if ! docker inspect bennetts-emacs >/dev/null 2>&1; then
            build_emacs && dockerme -v "${MY_CLOSURE_ALIAS_DIR}/emacs":/root bennetts-emacs "$@"
        else
            dockerme bennetts-emacs "$@"
        fi
    }

    alias emacs='emacsme emacs'

