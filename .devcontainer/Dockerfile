FROM mcr.microsoft.com/vscode/devcontainers/base:ubuntu-22.04

ARG USERNAME=vscode
ARG NOVNC_VERSION=1.2.0
ARG WEBSOCKIFY_VERSION=0.9.0

# install development tools
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    build-essential \
    nasm \
    acpica-tools \
    uuid-dev \
    qemu-system-x86 \
    qemu-utils \
    xauth \
    unzip \
    qemu-system-gui \
    dosfstools \
    git \
    python-is-python3 \
    python3-distutils \
    python3-numpy \
    manpages-dev \
    libc6-dev-i386 \
    mtools \
    clang-format \
    curl \
 && apt-get clean -y \
 && rm -rf /var/lib/apt/lists

# Install noVNC
RUN mkdir -p /usr/local/novnc \
 && curl -k -L -o /tmp/novnc.zip https://github.com/novnc/noVNC/archive/v${NOVNC_VERSION}.zip \
 && unzip /tmp/novnc.zip -d /usr/local/novnc/ \
 && cp /usr/local/novnc/noVNC-${NOVNC_VERSION}/vnc.html /usr/local/novnc/noVNC-${NOVNC_VERSION}/index.html \
 && curl -k -L -o /tmp/websockify.zip https://github.com/novnc/websockify/archive/v${WEBSOCKIFY_VERSION}.zip \
 && unzip /tmp/websockify.zip -d /usr/local/novnc/ \
 && ln -sf /usr/local/novnc/websockify-${WEBSOCKIFY_VERSION} /usr/local/novnc/noVNC-${NOVNC_VERSION}/utils/websockify \
 && rm -rf /tmp/novnc.zip /tmp/websockify.zip

COPY novnc.sh /usr/local/share
RUN chmod +x /usr/local/share/novnc.sh
ENTRYPOINT ["/usr/local/share/novnc.sh"]

USER ${USERNAME}
WORKDIR /home/${USERNAME}

USER root

ENV PATH="/home/${USERNAME}/osbook/devenv:${PATH}"
# ENV DISPLAY=host.docker.internal:0

# override startup command, taken from VSCode Devcontainer logs
CMD ["/bin/sh", "-c", "echo Container started ; trap \"exit 0\" 15; while sleep 1 & wait $!; do :; done"]
