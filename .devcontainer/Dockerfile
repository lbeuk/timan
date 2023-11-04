FROM mcr.microsoft.com/devcontainers/base:jammy

RUN apt update && apt upgrade -y
RUN apt install -y \
    libwebkit2gtk-4.0-dev \
    build-essential \
    curl \
    wget \
    file \
    libssl-dev \
    libgtk-3-dev \
    libayatana-appindicator3-dev \
    librsvg2-dev

RUN echo "export CARGO_HOME=~/.cargo" >> /home/vscode/.bashrc