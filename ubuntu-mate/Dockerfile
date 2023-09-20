FROM ghcr.io/linuxserver/rdesktop:ubuntu-mate
# set version label
ARG BUILD_DATE
ARG VERSION
LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"

RUN \
  echo "**** install packages ****" && \
  apt update && \
  DEBIAN_FRONTEND=noninteractive apt-get install wget -y && \
  echo "**** adding kubectl stuff ****" && \
  curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg && \
  echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list && \
  wget https://github.com/MuhammedKalkan/OpenLens/releases/download/v6.5.2.286/OpenLens-6.5.2.286.amd64.deb &&\
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
    kubectl \
    code \
    xdg-utils && \
  dpkg -i OpenLens-6.5.2.286.amd64.deb && \
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
