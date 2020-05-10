FROM postgres:10.12-alpine

ENV SCWS_VERSION 1.2.3
ENV ZHPARSER_VERSION 0.2.0

RUN set -eux; \
      apk add --no-cache --virtual .build-deps curl gcc g++ autoconf libtool automake make; \
      \
      mkdir -p /usr/src/packages; \
      \
# build scws
      curl -L https://github.com/hightman/scws/archive/$SCWS_VERSION.tar.gz | tar -xzC /usr/src/packages/; \
      cd /usr/src/packages/scws-$SCWS_VERSION/; \
      touch README; \
      aclocal; \
      autoconf; \
      autoheader; \
      libtoolize; \
      automake --add-missing; \
      ./configure; \
      make install; \
      \
# build zhparser
      curl -L https://github.com/amutu/zhparser/archive/v$ZHPARSER_VERSION.tar.gz | tar -xzC /usr/src/packages/; \
      cd /usr/src/packages/zhparser-$ZHPARSER_VERSION/; \
      SCWS_HOME=/usr/local make && make install; \
      \
# build zhparser
      apk del .build-deps; \
      rm -rf /usr/src/packages; \
      rm -rf /usr/local/share/doc /usr/local/share/man; \
      rm -rf /var/cache/apk/*;
