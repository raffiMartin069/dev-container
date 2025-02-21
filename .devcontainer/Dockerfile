FROM mcr.microsoft.com/devcontainers/python:1-3.12-bullseye AS build

RUN apt-get update -y && \
    apt-get install -y curl gnupg2 ca-certificates lsb-release debian-archive-keyring && \
    curl -fsSL https://nginx.org/keys/nginx_signing.key | gpg --dearmor -o /usr/share/keyrings/nginx-archive-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] http://nginx.org/packages/debian $(lsb_release -cs) nginx" \
    > /etc/apt/sources.list.d/nginx.list && \
    echo -e "Package: *\nPin: origin nginx.org\nPin-Priority: 900\n" > /etc/apt/preferences.d/99nginx && \
    apt-get update -y && \
    apt-get install -y nginx && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
