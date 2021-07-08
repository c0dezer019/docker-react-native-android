FROM ubuntu:21.10

LABEL maintainer="c0dezer019"
LABEL description="Provides a base environment to run react-native environment for Android."

# MIT License

# Copyright (c) Facebook, Inc. and its affiliates.

# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.

# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

# This is an updated version of the Docker container found at https://github.com/facebook/react-native
# in the ContainerShip directory.

# build arguments
ARG ANDROID_EMULATOR=emulator-linux_x64-7453540.zip
ARG CMDLINE_TOOLS=commandlinetools-linux-7006259_latest.zip
ARG PLATFORM_TOOLS=platform-tools_r31.0.2-linux.zip
ARG SDKTOOLS=sdk-tools-linux-4333796.zip
ARG TOOLS_VERSION=30.0.3
ARG BUCK_VERSION=v2021.01.12.01
ARG NDK_VERSION=android-ndk-r22b-linux-x86_64.zip
ARG NODE_VERSION=14.17.1
ARG WATCHMAN_VERSION=4.9.0

# build time args. Pass '--build-args key=value' to set when running the build command.
ARG user=user
ARG build_version=30

# default env variables
ENV ADB_INSTALL_TIMEOUT=10
ENV ANDROID_HOME=/opt/android/sdk
ENV ANDROID_SDK_HOME=${ANDROID_HOME}
ENV ANDROID_NDK=/opt/ndk/android-ndk-r$NDK_VERSION
ENV DEBIAN_FRONTEND=noninteractive
ENV DOCKER_BUILDKIT=0
ENV COMPOSE_DOCKER_CLI_BUILD=0
ENV PATH=${PATH}:/opt/buck/bin/
ENV PATH=${PATH}:${ANDROID_NDK}

# only run if user is provided.
RUN useradd -ms /bin/bash ${user}

# system dependencies
RUN apt-get update && \ 
    apt-get install \
    ant \
    autoconf \
    automake \
    curl \
    g++ \
    gcc \
    git \
    libqt5widgets5 \
    lib32z1 \
    lib32stdc++6 \
    make \
    maven \
    npm \
    openjdk-8* \
    python-dev \
    python3-dev \
    qml-module-qtquick-controls \
    qtdeclarative5-dev \
    unzip \
    apt-utils \
    gpgv2 \
    -y

RUN ln -fs /usr/share/zoneinfo/America/Chicago /etc/localtime && \
    dpkg-reconfigure --frontend noninteractive tzdata

# configure npm
RUN npm config set spin=false && \
    npm config set progress=false

# android stuff
RUN mkdir -p /opt/android/sdk

WORKDIR /opt/android/sdk

RUN curl --silent https://dl.google.com/android/repository/${SDKTOOLS} > android.zip && \
    unzip -q android.zip && \
    rm android.zip

ENV PATH=/opt/android/sdk/tools/bin:$PATH

RUN curl --silent https://dl.google.com/android/repository/${CMDLINE_TOOLS} > cmdline-tools.zip && \
    unzip -q cmdline-tools.zip && \
    rm cmdline-tools.zip

ENV PATH=/opt/android/sdk/cmdline-tools/latest/bin:$PATH

RUN mkdir -p /opt/android/sdk/.android && touch /opt/android/sdk/.android/repositories.cfg

# ndk
RUN mkdir /opt/ndk
WORKDIR /opt/ndk

RUN curl --silent https://dl.google.com/android/repository/${NDK_VERSION} > ndk.zip && \
    unzip -q ndk.zip && \
    rm ndk.zip

WORKDIR /

# adding android SDK tools
RUN yes | sdkmanager --licenses && yes | sdkmanager "system-images;android-18;google_apis;armeabi-v7a" \
    "platforms;android-$build_version" \
    "platform-tools" \
    "emulator" \
    "build-tools;$TOOLS_VERSION" \
    "add-ons;addon-google_apis-google-23" \
    "extras;android;m2repository"

# clean up
RUN rm -rf /opt/android/system-images/android-19/default/x86