FROM ghcr.io/fzdslr/alpine:3.21

ENV CURL_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt

COPY settings.json /var/lib/transmission/
COPY start.sh /usr/local/bin/start.sh

RUN apk --no-cache add curl \
                       transmission-cli \
                       transmission-daemon && \
    mkdir /config /data

USER transmission:users
VOLUME ["/config", "/data"]
CMD ["start.sh"]
