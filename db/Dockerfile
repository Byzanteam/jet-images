FROM postgres:10.12-alpine

RUN set -eux; \
      apk update; \
      apk add --no-cache curl gcc g++ autoconf libtool automake make;

RUN set -eux; \
      mkdir -p /usr/src/packages; \
      curl -L https://github.com/hightman/scws/archive/1.2.3.tar.gz | tar -xzC /usr/src/packages/; \
      cd /usr/src/packages/scws-1.2.3/; \
      touch README; \
      aclocal; \
      autoconf; \
      autoheader; \
      libtoolize; \
      automake --add-missing; \
      ./configure; \
      make install;

RUN set -eux; \
      apk del curl gcc g++ autoconf libtool automake make; \
      rm -rf /usr/src/packages/scws-1.2.3; \
      rm -rf /var/cache/apk/*;
