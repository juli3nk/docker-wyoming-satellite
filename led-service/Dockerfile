FROM alpine AS source

RUN apk --update add git
RUN git clone https://github.com/rhasspy/wyoming-satellite.git /tmp/wyoming-satellite


FROM python:3.11-slim-bookworm

ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install --yes gcc

WORKDIR /app

COPY --from=source /tmp/wyoming-satellite/requirements_respeaker.txt ./
COPY --from=source /tmp/wyoming-satellite/examples/2mic_service.py ./
COPY docker/setup ./script/
COPY docker/run ./

RUN script/setup

EXPOSE 10500

ENTRYPOINT ["/app/run"]
