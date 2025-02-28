FROM mcr.microsoft.com/devcontainers/python:1-3.12-bullseye AS build

# Install dependencies and tools (Removed android-sdk from apt-get)
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install nginx -y 