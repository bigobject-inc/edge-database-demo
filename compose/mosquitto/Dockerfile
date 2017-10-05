# Modified from https://hub.docker.com/_/eclipse-mosquitto/
FROM buildpack-deps:jessie-curl
MAINTAINER Cheng-Ching Huang <cchuang@bigobject.io>

LABEL Description="Eclipse Mosquitto MQTT Broker"

RUN apt-get update && apt-get install -y --no-install-recommends mosquitto && \
    mkdir -p /mosquitto/config /mosquitto/data /mosquitto/log && \
    cp /etc/mosquitto/mosquitto.conf /mosquitto/config && \
    chown -R mosquitto /mosquitto && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get purge -y --auto-remove

EXPOSE 1883

COPY docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["/usr/sbin/mosquitto", "-c", "/mosquitto/config/mosquitto.conf"]
