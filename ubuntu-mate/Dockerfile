FROM ghcr.io/linuxserver/rdesktop:ubuntu-mate
# set version label
ARG BUILD_DATE
# ARG BUILDARCH
ARG TARGETARCH
ARG VERSION
ARG KUBECTL_VERSION=1.30.2
ARG SEABIRD_VERSION=0.5.1
ARG K9S_VERSION=0.32.5
LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"
WORKDIR /tmp
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y curl wget
RUN curl -L https://github.com/MuhammedKalkan/OpenLens/releases/download/v6.5.2-366/OpenLens-6.5.2-366.${TARGETARCH}.deb --output /tmp/openlens.deb
RUN curl -L https://github.com/derailed/k9s/releases/download/v${K9S_VERSION}/k9s_Linux_${TARGETARCH}.tar.gz --output /tmp/k9s.tar.gz
RUN curl -l https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/linux/${TARGETARCH}/kubectl --output /usr/local/bin/kubectl && chmod +x /usr/local/bin/kubectl
RUN curl -L https://github.com/getseabird/seabird/releases/download/v${SEABIRD_VERSION}/seabird_linux_${TARGETARCH}.tar.gz --output /tmp/seabird.tar.gz


RUN \
  echo "**** install packages ****" && \
  apt update && \
  DEBIAN_FRONTEND=noninteractive apt-get install wget -y && \
  echo "**** Installing vscode stuff ***" && \
  wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg && \
  cp packages.microsoft.gpg /etc/apt/keyrings && \
  echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list && \
  DEBIAN_FRONTEND=noninteractive apt-get install apt-transport-https -y &&\
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive \
  apt-get install --no-install-recommends -y \
    vim \
    neofetch \
    libreoffice \
    thunderbird \
    pluma \
    tilix \
    terminator \
    code \
    xdg-utils && \
  dpkg -i /tmp/openlens.deb && \
  tar -zxvf /tmp/k9s.tar.gz && \
  tar -zxvf /tmp/seabird.tar.gz && \
  mv /tmp/k9s /usr/local/bin && \
  mv /tmp/seabird /usr/local/bin && \
  echo "**** cleanup ****" && \
  apt-get autoclean && \
  rm -rf \
    /var/lib/apt/lists/* \
    /var/tmp/* \
    /tmp/*

# add local files
COPY /root /

# ports and volumes
EXPOSE 3389
VOLUME /home
VOLUME /config
