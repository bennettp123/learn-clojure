FROM clojure:lein-alpine
COPY dumb-hacks-because-emacs-is-dumb/ /usr/local/share/ca-certificates/
RUN apk add --no-cache \
        ca-certificates \
        emacs \
 && update-ca-certificates \
 && rm -rf /var/cache/apk/*

