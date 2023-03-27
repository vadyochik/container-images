# Tor container image

This is very simple minimal tor configuration.

By default the image is meant to be used as a sidecar proxy container for other containerized apps.

You may want to pass additional options as arguments to the image to extend its functionality scope:

`podman run -d --rm --name tor-proxy -p 127.0.0.1:9050:9050 ghcr.io/vadyochik/tor --SocksPort 0.0.0.0:9050`

