FROM alpine AS source

RUN apk --update add git
RUN git clone https://github.com/rhasspy/wyoming-openwakeword.git /tmp/wyoming-openwakeword


FROM python:3.11-slim-bookworm

ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install --yes --no-install-recommends libopenblas-dev

WORKDIR /app

COPY --from=source /tmp/wyoming-openwakeword/script/setup ./script/
COPY --from=source /tmp/wyoming-openwakeword/setup.py /tmp/wyoming-openwakeword/requirements*.txt /tmp/wyoming-openwakeword/MANIFEST.in ./
COPY --from=source /tmp/wyoming-openwakeword/wyoming_openwakeword/ ./wyoming_openwakeword/

RUN script/setup

COPY --from=source /tmp/wyoming-openwakeword/script/run ./script/
COPY docker/run ./

EXPOSE 10400

ENTRYPOINT ["/app/run"]
