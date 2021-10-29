FROM postgres:13.4-alpine

ENV VERSION 13.4_zhparser-e909539

ENV SCWS_VERSION 1.2.3
ENV ZHPARSER_VERSION e909539a0d9b87b69acd76d4d3a3b073095bf5a5

RUN set -eux \
      && apk add --quiet --purge --no-cache --virtual .build-deps curl gcc g++ autoconf libtool automake make clang-dev llvm10-dev \
      && mkdir -p /usr/src/packages \
      \
# build scws
      && curl -Ls https://github.com/hightman/scws/archive/$SCWS_VERSION.tar.gz | tar -xzC /usr/src/packages/ \
      && cd /usr/src/packages/scws-$SCWS_VERSION/ \
      && touch README && aclocal && autoconf && autoheader && libtoolize && automake --add-missing \
      && ./configure && make install \
      \
# # build zhparser
      && curl -Ls --output /usr/src/packages/zhparser-$ZHPARSER_VERSION.zip https://github.com/amutu/zhparser/archive/$ZHPARSER_VERSION.zip \
      && unzip -qq /usr/src/packages/zhparser-$ZHPARSER_VERSION.zip -d /usr/src/packages/ \
      && cd /usr/src/packages/zhparser-$ZHPARSER_VERSION/ \
      && SCWS_HOME=/usr/local make && make install \
      \
# build zhparser
      && apk del --quiet --purge --rdepends .build-deps \
      && rm -rf /usr/src/packages \
      && rm -rf /usr/local/share/doc /usr/local/share/man \
      && rm -rf /var/cache/apk/*
