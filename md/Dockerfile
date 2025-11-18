FROM alpine:3.22.2@sha256:4b7ce07002c69e8f3d704a9c5d6fd3053be500b7f1c69fc0d80990c2ad8dd412

HEALTHCHECK NONE

ENTRYPOINT []

ARG USER_NAME=default
ARG USER_HOME=/home/default
ARG USER_ID=1000
ARG USER_GECOS=Default

SHELL ["/bin/ash", "-euo", "pipefail", "-c"]

RUN apk upgrade --no-cache \
    && apk add --no-cache \
    aspell=0.60.8.1-r0 \
    aspell-de=20161207.7.0-r0 \
    aspell-en=2020.12.07-r0 \
    aspell-fr=0.50.3-r0 \
    aspell-ru=0.99_p7-r2 \
    aspell-uk=1.4.0-r3 \
    coreutils=9.7-r1 \
    curl=8.14.1-r2 \
    git=2.49.1-r0 \
    go-md2man=2.0.7-r4 \
    jq=1.8.1-r0 \
    markdown=1.0.1-r3 \
    npm=11.4.2-r0 \
    pandoc-cli=3.6.4-r0 \
    pipx=1.7.1-r0 \
    py3-pip=25.1.1-r0 \
    python3=3.12.12-r0 \
    && apk cache --no-cache clean \
    && rm -rf /var/cache/apk/*

COPY requirements.txt /tmp/requirements.txt

RUN pipx ensurepath --global \
    && xargs -a /tmp/requirements.txt -n 1 pipx install --global \
    && rm -f /tmp/requirements.txt

COPY package.json /tmp/package.json

RUN jq -r '.dependencies | to_entries[] | "\(.key)@\(.value)"' /tmp/package.json \
    | xargs -n 1 npm install -g \
    && rm -f /tmp/package.json

RUN adduser \
  --home "${USER_HOME}" \
  --uid "${USER_ID}" \
  --gecos "${USER_GECOS}" \
  --disabled-password \
  "${USER_NAME}"

USER "${USER_NAME}"

ENV HOME="${USER_HOME}"

WORKDIR "${HOME}"
