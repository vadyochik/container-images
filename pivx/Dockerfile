# Base image
FROM alpine:latest AS build

# Recieve Fuzzbawls key for verification purposes
RUN apk add --no-cache gnupg && \
  gpg --keyserver hkps://keys.openpgp.org --recv-keys 6F993B250557E7B016ADE5713BDCDA2D87A881D9

WORKDIR /tmp

ARG TARGETPLATFORM
ENV PIVX_VERSION=5.5.0

RUN \
  case "${TARGETPLATFORM}" in \
  "linux/386") platform="i686-pc-linux-gnu" ;; \
  "linux/amd64") platform="x86_64-linux-gnu" ;; \
  "linux/arm64"|"linux/arm64/v8") platform="aarch64-linux-gnu" ;; \
  esac && \
  wget "https://github.com/PIVX-Project/PIVX/releases/download/v${PIVX_VERSION}/pivx-${PIVX_VERSION}-${platform}.tar.gz" && \
  wget "https://github.com/PIVX-Project/PIVX/releases/download/v${PIVX_VERSION}/SHA256SUMS.asc" && \
  gpgv --keyring ~/.gnupg/pubring.kbx ./SHA256SUMS.asc && \
  grep "pivx-${PIVX_VERSION}-${platform}.tar.gz" ./SHA256SUMS.asc | sha256sum -c - && \
  tar xvzf "./pivx-${PIVX_VERSION}-${platform}.tar.gz" && \
  mkdir -v /tmp/artifacts && \
  ln ./pivx-${PIVX_VERSION}/share/pivx/sapling-*.params /tmp/artifacts/ && \
  ln ./pivx-${PIVX_VERSION}/bin/pivxd /tmp/artifacts/ && \
  ln ./pivx-${PIVX_VERSION}/bin/pivx-cli /tmp/artifacts/ && \
  ls -la /tmp/artifacts

# Final image
FROM debian:11-slim

COPY --from=build /tmp/artifacts/pivx* /usr/local/bin/
COPY --from=build /tmp/artifacts/sapling-*.params /home/pivx/.pivx-params/

RUN useradd --uid 1000 --home-dir /home/pivx --shell /usr/sbin/nologin pivx && \
  mkdir -vm 0750 /home/pivx/.pivx && \
  chown -vR pivx:pivx /home/pivx

EXPOSE 51472

VOLUME ["/home/pivx/.pivx"]

USER pivx

ENTRYPOINT ["/usr/local/bin/pivxd", "-printtoconsole" ]

