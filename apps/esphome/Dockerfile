# syntax=docker/dockerfile:1

FROM docker.io/library/python:3.13-slim
ARG VERSION

ENV \
    CRYPTOGRAPHY_DONT_BUILD_RUST=1 \
    PIP_BREAK_SYSTEM_PACKAGES=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_ROOT_USER_ACTION=ignore \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    UV_NO_CACHE=true \
    UV_SYSTEM_PYTHON=true \
    UV_EXTRA_INDEX_URL="https://wheels.home-assistant.io/musllinux/"

ENV \
    HOME="/config" \
    PLATFORMIO_CORE_DIR=/cache/pio \
    ESPHOME_BUILD_PATH=/cache/build \
    ESPHOME_DATA_DIR=/cache/data

USER root
WORKDIR /app

RUN \
    apt-get update \
    && \
    apt-get install -y --no-install-recommends --no-install-suggests \
        ca-certificates \
        catatonit \
        curl \
        git \
        iputils-ping \
        libcairo2 \
        libmagic1 \
        openssh-client \
        patch \
    && pip install uv \
    && uv pip install \
        setuptools \
        "esphome[displays]==${VERSION}" \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /tmp/* /var/{cache,log}/* /var/lib/apt/lists/* /usr/src/*

COPY . /

USER nobody:nogroup

WORKDIR /config

ENTRYPOINT ["/usr/bin/catatonit", "--", "/entrypoint.sh"]
CMD ["dashboard", "/config"]
