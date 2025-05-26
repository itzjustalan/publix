FROM debian:bookworm-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    ca-certificates \
    curl \
    git \
    gnupg \
    sudo \
    tmux \
    unzip \
    && rm -rf /var/lib/apt/lists/*

COPY --from=oven/bun:latest /usr/local/bin/bun /usr/local/bin/bun
COPY --from=oven/bun:latest /usr/local/bin/bunx /usr/local/bin/bunx

ARG USERNAME=devuser
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s /bin/bash \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME
ENV HOME=/home/$USERNAME
WORKDIR /workspaces/publix

EXPOSE 3000 5173

# set alias for the user
RUN echo 'alias ll="ls -lah"' >> "${HOME}/.bashrc"

# Default command to run when the container starts (can be overridden).
# Keeps the container running and provides a shell.
CMD ["tail", "-f", "/dev/null"]

