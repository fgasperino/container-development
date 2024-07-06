FROM clojure:temurin-21-bookworm

ARG USER=
ARG HOME=
ARG UID=

ENV DEBIAN_FRONTEND noninteractive
ENV HOME=$HOME
ENV PATH=$PATH:$HOME:$HOME/bin
ENV GPG_TTY=$(tty)

RUN apt-get clean
RUN apt-get update
RUN apt-get install -y bash sudo locales wget busybox

# Environment customization.
RUN apt-get install -y tmux gcc unzip git ripgrep gpg pinentry-curses
RUN apt-get autoclean

# Add USER to image w/ sudo privs.
RUN useradd -m -d $HOME -u $UID -s /bin/bash $USER
RUN echo "$USER ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USER && chmod 0400 /etc/sudoers.d/$USER

# Create some optional directories for volume mounts.
RUN mkdir -p $HOME/bin
RUN mkdir -p $HOME/.config $HOME/.cache $HOME/.clojure $HOME/.lein

# Add clojure-LSP locally.
RUN wget https://github.com/clojure-lsp/clojure-lsp/releases/download/2024.04.22-11.50.26/clojure-lsp-native-static-linux-amd64.zip -O - | \
    busybox unzip - && \
    chmod 755 clojure-lsp && \
    mv clojure-lsp $HOME/bin/

#
# Add any editors locally for development within the container.
#

# NeoVIM AppImage.
RUN wget https://github.com/neovim/neovim/releases/download/v0.10.0/nvim.appimage && \
    chmod 755 nvim.appimage && \
    ./nvim.appimage --appimage-extract && \
    cp -a squashfs-root/usr /

# Change ownership of $HOME to $USER.
RUN chown -R $USER:$USER $HOME && chmod 755 $HOME

WORKDIR $HOME
USER $USER

CMD ["sleep", "infinity"]
