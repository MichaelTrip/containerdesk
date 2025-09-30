FROM ghcr.io/linuxserver/baseimage-rdesktop:debianbookworm
# set version label
ARG BUILD_DATE
# ARG BUILDARCH
ARG TARGETARCH
ARG VERSION
ARG KUBECTL_VERSION=1.30.2
ARG HELM_VERSION=3.15.2
ARG HEADLAMP_VERSION=0.35.0
ARG K9S_VERSION=0.50.9
ARG FREELENS_VERSION=1.5.3
LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"
WORKDIR /tmp

# Download all packages and install everything in one layer
RUN \
  echo "**** Update package lists and install base tools ****" && \
  DEBIAN_FRONTEND=noninteractive apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y curl wget gnupg2 ca-certificates apt-transport-https && \
  echo "**** Download external packages ****" && \
  case ${TARGETARCH} in \
    "amd64")  DOWNLOAD_ARCH=linux-amd64  ;; \
    "arm64")  DOWNLOAD_ARCH=linux-arm64  ;; \
  esac && \
  curl -L https://github.com/freelensapp/freelens/releases/download/v${FREELENS_VERSION}/Freelens-${FREELENS_VERSION}-${DOWNLOAD_ARCH}.deb -o /tmp/freelens.deb && \
  curl -L https://github.com/derailed/k9s/releases/download/v${K9S_VERSION}/k9s_Linux_${TARGETARCH}.tar.gz -o /tmp/k9s.tar.gz && \
  curl -L https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/linux/${TARGETARCH}/kubectl -o /usr/local/bin/kubectl && \
  chmod +x /usr/local/bin/kubectl && \
  curl -L https://get.helm.sh/helm-v${HELM_VERSION}-linux-${TARGETARCH}.tar.gz -o /tmp/helm.tar.gz && \
  curl -L https://github.com/kubernetes-sigs/headlamp/releases/download/v${HEADLAMP_VERSION}/headlamp_${HEADLAMP_VERSION}-1_${TARGETARCH}.deb -o /tmp/headlamp.deb && \
  echo "**** Setup VS Code repository ****" && \
  wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg && \
  cp packages.microsoft.gpg /etc/apt/keyrings && \
  echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list && \
  apt-get update && \
  echo "**** Install all packages ****" && \
  mkdir -p /usr/share/man/man1 && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y \
    vim \
    neofetch \
    firefox-esr \
    pluma \
    tilix \
    xdg-utils \
    mate-desktop-environment \
    man-db \
    default-jre-headless \
    openjdk-17-jre-headless \
    libreoffice-writer \
    libreoffice-calc \
    libreoffice-impress \
    libreoffice-draw \
    thunderbird \
    gnome-themes-extra \
    gnome-themes-extra-data \
    code && \
  echo "**** Install downloaded packages and extract tools ****" && \
  apt-get install -y /tmp/freelens.deb && \
  apt-get install -y /tmp/headlamp.deb && \
  tar -zxvf /tmp/k9s.tar.gz && \
  tar -zxvf /tmp/helm.tar.gz && \
  mv linux-${TARGETARCH}/helm /usr/local/bin/helm && \
  mv /tmp/k9s /usr/local/bin && \
  echo "**** MATE desktop tweaks ****" && \
  rm -f \
    /etc/xdg/autostart/mate-power-manager.desktop \
    /etc/xdg/autostart/mate-screensaver.desktop && \
  echo "**** Cleanup ****" && \
  apt-get autoclean && \
  rm -rf \
    /var/lib/apt/lists/* \
    /var/tmp/* \
    /tmp/*

COPY /root /

# ports and volumes
EXPOSE 3389
VOLUME /home
VOLUME /config
