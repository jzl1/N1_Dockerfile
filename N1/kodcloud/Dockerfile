FROM php:7.1-alpine

# Label for Information about this Image.
LABEL org.opencontainers.image.authors="lstcml" \
        org.opencontainers.image.title="N1_Kodcloud" \
        org.opencontainers.image.description="Kodcloud on arm64" \
        org.opencontainers.image.url="https://hub.docker.com/r/lstcml/n1_kodcloud" \
        org.opencontainers.image.source="https://github.com/a583560750/N1_Dockerfile"

ENV KODEXPLORER_VERSION=4.40
ENV KODEXPLORER_URL="http://static.kodcloud.com/update/download/kodexplorer$KODEXPLORER_VERSION.zip"

RUN set -x \
  && mkdir -p /usr/src/kodexplorer \
  && apk --update --no-cache add wget bash unzip\
  && wget -O /tmp/kodexplorer.zip "$KODEXPLORER_URL" \
  && unzip /tmp/kodexplorer.zip -d /usr/src/kodexplorer/ \
  && rm -rf /tmp/*

RUN set -x \
  && apk add --no-cache --update \
        freetype libpng libjpeg-turbo \
        freetype-dev libpng-dev libjpeg-turbo-dev \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install -j "$(getconf _NPROCESSORS_ONLN)" gd \
  && apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

WORKDIR /var/www/html

COPY entrypoint.sh /usr/local/bin/

#COPY kodcloud/* /usr/src/kodexplorer/

RUN chmod +x /usr/local/bin/entrypoint.sh

EXPOSE 80

ENTRYPOINT ["entrypoint.sh"]
CMD [ "php", "-S", "0000:80", "-t", "/var/www/html" ]

