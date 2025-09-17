FROM ghcr.io/linuxserver/rdesktop:ubuntu-xfce
# set version label
ARG BUILD_DATE
# ARG BUILDARCH
ARG TARGETARCH
ARG VERSION
ARG KUBECTL_VERSION=1.30.2
ARG HELM_VERSION=3.15.2
ARG SEABIRD_VERSION=0.6.0
ARG K9S_VERSION=0.50.9
ARG FREELENS_VERSION=1.5.3
LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"
WORKDIR /tmp
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y curl wget
RUN \
  case ${TARGETARCH} in \
    "amd64")  DOWNLOAD_ARCH=linux-amd64  ;; \
    "arm64")  DOWNLOAD_ARCH=linux-arm64  ;; \
  esac \
  && curl -L https://github.com/freelensapp/freelens/releases/download/v${FREELENS_VERSION}/Freelens-${FREELENS_VERSION}-${DOWNLOAD_ARCH}.deb --output /tmp/freelens.deb
RUN curl -L https://github.com/derailed/k9s/releases/download/v${K9S_VERSION}/k9s_Linux_${TARGETARCH}.tar.gz --output /tmp/k9s.tar.gz
RUN curl -L https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/linux/${TARGETARCH}/kubectl --output /usr/local/bin/kubectl && chmod +x /usr/local/bin/kubectl
RUN curl -L https://get.helm.sh/helm-v${HELM_VERSION}-linux-${TARGETARCH}.tar.gz --output /tmp/helm.tar.gz
RUN curl -L https://github.com/getseabird/seabird/releases/download/v${SEABIRD_VERSION}/seabird_linux_${TARGETARCH}.tar.gz --output /tmp/seabird.tar.gz


RUN echo "**** install packages ****"
RUN apt update
RUN DEBIAN_FRONTEND=noninteractive apt-get install wget -y
RUN echo "**** Installing vscode stuff ***"
RUN wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
RUN cp packages.microsoft.gpg /etc/apt/keyrings
RUN echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list
RUN DEBIAN_FRONTEND=noninteractive apt-get install apt-transport-https -y
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
  vim \
  neofetch \
  libreoffice \
  thunderbird \
  pluma \
  tilix \
  terminator \
  code \
#  xfce4-themes
  xdg-utils
RUN apt-get install -y /tmp/freelens.deb
RUN tar -zxvf /tmp/k9s.tar.gz
RUN tar -zxvf /tmp/seabird.tar.gz
RUN tar -zxvf /tmp/helm.tar.gz
RUN mv linux-${TARGETARCH}/helm /usr/local/bin/helm
RUN mv /tmp/k9s /usr/local/bin
RUN mv /tmp/seabird /usr/local/bin
RUN echo "**** cleanup ****"
RUN apt-get autoclean
RUN rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*

COPY /root /

# ports and volumes
EXPOSE 3389
VOLUME /home
VOLUME /config
