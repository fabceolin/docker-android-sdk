FROM ubuntu:18.04

LABEL de.mindrunner.android-docker.flavour="ubuntu-standalone"

ENV ANDROID_SDK_HOME /opt/android-sdk-linux
ENV ANDROID_SDK_ROOT /opt/android-sdk-linux
ENV ANDROID_HOME /opt/android-sdk-linux
ENV ANDROID_SDK /opt/android-sdk-linux

ENV DEBIAN_FRONTEND noninteractive

# Install required tools
# Dependencies to execute Android builds

RUN dpkg --add-architecture i386
#RUN dpkg --add-architecture i386 && apt-get update -yqq && apt-get install -y \
#  libc6:i386 \
#  libgcc1:i386 \
#  libncurses5:i386 \
#  libstdc++6:i386 \
#  zlib1g:i386 \
RUN apt-get update -yqq
RUN apt-get install -y \
  bsdtar \
  cmake \
  curl \
  expect \
  file \
  git \
  locales \
  make \
  neovim \
  openjdk-11-jdk \
  openjdk-11-jdk \
  openssh-client \
  python3-pip \
  python3-venv \
  qemu-efi \
  qemu-system \
  unzip \
  vim \
  wget \
  xbase-clients

RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*
RUN localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

ENV LANG en_US.UTF-8

RUN groupadd android && useradd -d /opt/android-sdk-linux -g android -u 1000 android
RUN mkdir /etc/sudoers.d && echo android ALL=\(ALL\) NOPASSWD:ALL > /etc/sudoers.d/android

RUN python3 -m pip install pip --upgrade &&  \
    python3 -m pip install aqtinstall && \
    mkdir /opt/android-sdk-linux && \
    cd /opt/android-sdk-linux/ && \
    aqt install-qt linux desktop 6.3.2 gcc_64 && \
    aqt install-qt linux android 6.3.2 android_x86_64 -m all && \
    aqt install-qt linux android 6.3.2 android_armv7 -m all

COPY tools /opt/tools

COPY licenses /opt/licenses

WORKDIR /opt/android-sdk-linux

RUN /opt/tools/entrypoint.sh built-in

CMD /opt/tools/entrypoint.sh built-in
