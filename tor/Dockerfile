FROM alpine:3.17.2

RUN apk add --no-cache tor=0.4.7.13-r0 && \
  echo "SocksPort 9050" > /etc/tor/torrc && \
  echo "ControlPort 9051" >> /etc/tor/torrc && \
  echo "DataDirectory /var/lib/tor" >> /etc/tor/torrc

VOLUME ["/var/lib/tor"]

USER tor

ENTRYPOINT ["/usr/bin/tor"]
