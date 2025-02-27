FROM mcr.microsoft.com/devcontainers/python:1-3.12-bullseye AS build

# Set environment variables
ENV ANDROID_SDK_ROOT=/usr/lib/android-sdk
ENV FLUTTER_SDK_ROOT=/usr/lib/flutter
ENV FLUTTER_SDK_VERSION=3.27.1-stable

# Include Flutter and Android tools in PATH
ENV PATH="${PATH}:${FLUTTER_SDK_ROOT}/bin:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools"

# Enable noninteractive installation
ENV DEBIAN_FRONTEND=noninteractive

# Remove any potential dpkg locks before installation
RUN rm -rf /var/lib/dpkg/lock /var/lib/dpkg/lock-frontend

# Install dependencies and tools (Removed android-sdk from apt-get)
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        nginx \
        watchman \
        openjdk-17-jdk \
        wget \
        unzip \
        zip \
        curl \
        git-all \
        xz-utils \
        libglu1-mesa \
        libc6 \
        libstdc++6 \
        lib32z1 \
        libbz2-1.0 \
        clang \
        cmake \
        ninja-build \
        libgtk-3-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && apt-get autoremove -y && apt-get autoclean


# Add Flutter to the safe directory in Git
RUN git config --global --add safe.directory /usr/lib/flutter

# Configure Git user name and email (Modify these values)
RUN git config --global user.name "Your Name" \
    && git config --global user.email "mail@example.com"

# Download and install Android Command Line Tools
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    wget -O commandlinetools.zip "https://dl.google.com/android/repository/commandlinetools-linux-8512546_latest.zip" && \
    unzip commandlinetools.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools && \
    mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest && \
    rm commandlinetools.zip

# Accept Android SDK licenses and install necessary components
RUN yes | ${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin/sdkmanager --licenses || true && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin/sdkmanager \
    "platform-tools" \
    "platforms;android-34" \
    "build-tools;33.0.0" \
    "build-tools;29.0.3"

# Install Flutter SDK
RUN mkdir -p ${FLUTTER_SDK_ROOT} && \
    cd ${FLUTTER_SDK_ROOT} && \
    curl -OL https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_${FLUTTER_SDK_VERSION}.tar.xz && \
    tar -xf flutter_linux_${FLUTTER_SDK_VERSION}.tar.xz -C /usr/lib/ && \
    rm -rf flutter_linux_${FLUTTER_SDK_VERSION}.tar.xz

# Disable Flutter telemetry
RUN flutter --disable-analytics

# Set environment variables for Android Studio and Chrome
ENV ANDROID_SDK_ROOT="/mnt/c/Android-sdk/Sdk/Sdk"
ENV ANDROID_HOME="/mnt/c/Android-sdk/Sdk/Sdk"
ENV ANDROID_STUDIO_PATH="/mnt/c/Android/Android Studio"
ENV CHROME_EXECUTABLE="/mnt/c/Google/Application/chrome.exe"
ENV JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"

RUN flutter config --android-studio-dir="$ANDROID_STUDIO_PATH"

# Default shell to bash
SHELL ["/bin/bash", "-c"]