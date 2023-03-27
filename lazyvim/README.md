# LazyVim inside tmux

## Build container image

```
docker build --build-arg USER --pull -t lazyvim .
```

## Create and start container

```
docker compose up -d
docker attach lazyvim-lazyvim-1
```

## Tear down

Remove the container and its named volume:

```
docker compose down -v
```

## Hints

Tmux resurrect plugin: Use `ctrl+b,ctrl+s` to save your current tmux session and `ctrl+b,ctrl+r` to restore it later.

Volume mounts in `docker-compose.yml` file are used to set correct timezone, and forward ssh and gpg auth stuff from host into container.

