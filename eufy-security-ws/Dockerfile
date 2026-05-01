ARG BUILD_FROM
FROM $BUILD_FROM

ARG EUFY_SECURITY_CLIENT_REPO
ARG EUFY_SECURITY_CLIENT_REF
ARG EUFY_SECURITY_CLIENT_CACHE_KEY
ARG EUFY_SECURITY_WS_REPO
ARG EUFY_SECURITY_WS_REF
ARG EUFY_SECURITY_WS_CACHE_KEY

WORKDIR /usr/src/app
RUN \
    set -x \
    && apk add --no-cache \
    git \
    nodejs \
    npm \
    jq \
    && echo "client=${EUFY_SECURITY_CLIENT_CACHE_KEY} ws=${EUFY_SECURITY_WS_CACHE_KEY}" \
    && git clone --depth 1 --branch "${EUFY_SECURITY_CLIENT_REF}" "${EUFY_SECURITY_CLIENT_REPO}" /tmp/eufy-security-client \
    && cd /tmp/eufy-security-client \
    && npm install --force \
    && npm run build \
    && npm pack --pack-destination /tmp \
    && git clone --depth 1 --branch "${EUFY_SECURITY_WS_REF}" "${EUFY_SECURITY_WS_REPO}" /tmp/eufy-security-ws \
    && cd /tmp/eufy-security-ws \
    && npm pkg set "dependencies.eufy-security-client=file:/tmp/eufy-security-client-3.8.0.tgz" \
    && npm install --force --package-lock=false \
    && npm run build \
    && npm pack --pack-destination /tmp \
    && cd /usr/src/app \
    && npm install --force /tmp/eufy-security-ws-2.1.0.tgz \
    && rm -rf /tmp/eufy-security-client /tmp/eufy-security-ws /tmp/*.tgz

COPY run.sh /
RUN chmod a+x /run.sh
WORKDIR /data

CMD [ "/run.sh" ]
