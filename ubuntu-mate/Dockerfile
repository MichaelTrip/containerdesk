FROM lscr.io/linuxserver/rdesktop:ubuntu-mate
# set version label
ARG BUILD_DATE
# ARG BUILDARCH
ARG TARGETARCH
ARG VERSION
ARG KUBECTL_VERSION=1.32.9
ARG HELM_VERSION=3.15.2
ARG HEADLAMP_VERSION=0.37.0
ARG K9S_VERSION=0.50.16
ARG FREELENS_VERSION=1.6.1
ARG KUBECM_VERSION=0.27.1
ARG KUBECTX_VERSION=0.9.5

LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"
WORKDIR /tmp
# USER root
# Download all packages and install everything in one layer
RUN set -eux; \
  echo "**** Update package lists and install base tools ****"; \
  DEBIAN_FRONTEND=noninteractive apt-get update; \
  DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends curl wget gnupg2 ca-certificates apt-transport-https software-properties-common; \
  echo "**** Determine download arch and download external packages ****"; \
  case "${TARGETARCH}" in \
    amd64) DOWNLOAD_ARCH=linux-amd64 ;; \
    arm64) DOWNLOAD_ARCH=linux-arm64 ;; \
    *) DOWNLOAD_ARCH=linux-amd64 ;; \
  esac; \
  curl -L "https://github.com/freelensapp/freelens/releases/download/v${FREELENS_VERSION}/Freelens-${FREELENS_VERSION}-${DOWNLOAD_ARCH}.deb" -o /tmp/freelens.deb; \
  curl -L "https://github.com/derailed/k9s/releases/download/v${K9S_VERSION}/k9s_Linux_${TARGETARCH}.tar.gz" -o /tmp/k9s.tar.gz; \
  curl -L "https://dl.k8s.io/release/v${KUBECTL_VERSION}/bin/linux/${TARGETARCH}/kubectl" -o /usr/local/bin/kubectl; \
  curl -L "https://github.com/sunny0826/kubecm/releases/download/v${KUBECM_VERSION}/kubecm_v${KUBECM_VERSION}_Linux_x86_64.tar.gz" -o /tmp/kubecm.tar.gz; \
  curl -L "https://github.com/ahmetb/kubectx/releases/download/v${KUBECTX_VERSION}/kubectx" -o /usr/local/bin/kubectx; \
  chmod +x /usr/local/bin/kubectx; \
  chmod +x /usr/local/bin/kubectl; \
  curl -L "https://get.helm.sh/helm-v${HELM_VERSION}-linux-${TARGETARCH}.tar.gz" -o /tmp/helm.tar.gz; \
  curl -L "https://github.com/kubernetes-sigs/headlamp/releases/download/v${HEADLAMP_VERSION}/headlamp_${HEADLAMP_VERSION}-1_${TARGETARCH}.deb" -o /tmp/headlamp.deb; \
  echo "**** Setup VS Code repository and extra PPAs ****"; \
  wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /tmp/packages.microsoft.gpg; \
  mkdir -p /etc/apt/keyrings; \
  cp /tmp/packages.microsoft.gpg /etc/apt/keyrings/; \
  echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list; \
  add-apt-repository ppa:xtradeb/apps -y; \
  echo "**** Apt update and install packages ****"; \
  DEBIAN_FRONTEND=noninteractive apt-get update; \
  mkdir -p /usr/share/man/man1; \
  DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    vim neofetch pluma chromium chromium-sandbox tilix xdg-utils man-db default-jre-headless openjdk-17-jre-headless \
    libreoffice-writer libreoffice-calc libreoffice-impress libreoffice-draw thunderbird gnome-themes-extra \
    gnome-themes-extra-data code thunderbird; \
  echo "**** Install downloaded packages and extract tools ****"; \
  DEBIAN_FRONTEND=noninteractive apt-get install -y /tmp/freelens.deb /tmp/headlamp.deb; \
  tar -C /tmp -zxvf /tmp/k9s.tar.gz; \
  tar -C /tmp -zxvf /tmp/helm.tar.gz; \
  tar -C /tmp -zxvf /tmp/kubecm.tar.gz; \
  mv /tmp/kubecm /usr/local/bin/kubecm; \
  mv /tmp/linux-${TARGETARCH}/helm /usr/local/bin/helm; \
  # when k9s tarball extracts a binary in /tmp or a subdir, move it to /usr/local/bin
  if [ -f /tmp/k9s ]; then mv /tmp/k9s /usr/local/bin/; else mv /tmp/*k9s* /usr/local/bin/ || true; fi; \
  echo "**** Cleanup ****"; \
  apt-get --yes autoremove; apt-get -y autoclean; \
  rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*


COPY /root /

# ports and volumes
EXPOSE 3389
VOLUME /home
VOLUME /config
