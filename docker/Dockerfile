FROM clojure:temurin-24-bookworm

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

#
# Add any editors locally for development within the container.
#

# NeoVIM AppImage.
RUN wget https://github.com/neovim/neovim/releases/download/v0.11.3/nvim-linux-x86_64.appimage && \
    chmod 755 nvim-linux-x86_64.appimage && \
    ./nvim-linux-x86_64.appimage --appimage-extract && \
    cp -a squashfs-root/usr /

# Change ownership of $HOME to $USER.
RUN chown -R $USER:$USER $HOME && chmod 755 $HOME

WORKDIR $HOME
USER $USER

CMD ["sleep", "infinity"]
