ARG VERSION=latest
FROM alpine:$VERSION

RUN apk add \
      alpine-sdk \
      bash \
      fd \
      git \
      gpg \
      gpg-agent \
      lazygit \
      luajit-dev \
      neovim \
      neovim-doc \
      npm \
      openssh-client-default \
      py3-pip \
      py3-pynvim \
      python3 \
      ripgrep \
      stylua \
      tmux \
      --update

ARG USER
ENV USER=${USER:-b00za}

RUN adduser -s /bin/bash -D ${USER} && \
    git clone https://github.com/LazyVim/starter /home/${USER}/.config/nvim && \
    mkdir -m 2700 /home/${USER}/.gnupg && \
    chown -R ${USER}:${USER} /home/${USER}

COPY --chown=${USER}:${USER} ./dotfiles/ /home/${USER}/

WORKDIR /home/${USER}

VOLUME ["/home/${USER}"]

USER ${USER}

CMD ["/bin/bash", "-ic", "tmux"]
