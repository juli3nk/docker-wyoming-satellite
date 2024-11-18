FROM alpine AS source

RUN apk --update add git
RUN git clone https://github.com/rhasspy/wyoming-satellite.git /tmp/wyoming-satellite


FROM python:3.11-slim-bookworm

ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

ARG SETUP_ARGS

RUN apt-get update && \
    apt-get install --no-install-recommends --yes \
	avahi-utils \
	pulseaudio-utils

WORKDIR /app

COPY --from=source /tmp/wyoming-satellite/sounds/ ./sounds/
COPY --from=source /tmp/wyoming-satellite/setup.py /tmp/wyoming-satellite/requirements*.txt /tmp/wyoming-satellite/MANIFEST.in ./
COPY --from=source /tmp/wyoming-satellite/wyoming_satellite/ ./wyoming_satellite/

COPY --from=source /tmp/wyoming-satellite/script/run ./script/
COPY --from=source /tmp/wyoming-satellite/docker/run ./

COPY actions ./actions
COPY docker/setup ./script/
RUN script/setup $SETUP_ARGS

EXPOSE 10700

ENTRYPOINT ["/app/run"]
