# syntax=docker/dockerfile:1

FROM ghcr.io/fzdslr/unrar-alpine:latest AS unrar

FROM ghcr.io/fzdslr/alpine:3.21

# ARG BUILD_DATE
# ARG VERSION
ARG TRANSMISSION_VERSION
# LABEL build_version="Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"

RUN \
  echo "**** install build packages ****" && \
  apk add --no-cache --virtual=build-dependencies \
    build-base && \
  echo "**** install packages ****" && \
  apk add --no-cache \
    findutils \
    p7zip \
    python3 && \
  echo "**** install transmission ****" && \
  if [ -z ${TRANSMISSION_VERSION+x} ]; then \
    TRANSMISSION_VERSION=$(curl -sL "http://dl-cdn.alpinelinux.org/alpine/v3.21/community/loongarch64/APKINDEX.tar.gz" | tar -xz -C /tmp \
    && awk '/^P:transmission$/,/V:/' /tmp/APKINDEX | sed -n 2p | sed 's/^V://'); \
  fi && \
  apk add --no-cache \
    transmission-cli==${TRANSMISSION_VERSION} \
    transmission-daemon==${TRANSMISSION_VERSION} \
    transmission-extra==${TRANSMISSION_VERSION} \
    transmission-remote==${TRANSMISSION_VERSION} && \
#  printf "Linuxserver.io version: ${VERSION}\nBuild-date: ${BUILD_DATE}" > /build_version && \
  echo "**** cleanup ****" && \
  apk del --purge \
    build-dependencies && \
  rm -rf \
    /tmp/* \
    $HOME/.cache

# copy local files
COPY root/ /

# add unrar
COPY --from=unrar /usr/bin/unrar-alpine /usr/bin/unrar

# ports and volumes
EXPOSE 9091 51413/tcp 51413/udp
VOLUME /config
