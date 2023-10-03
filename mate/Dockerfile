FROM docker.io/linuxserver/rdesktop:fedora-mate

# set version label
ARG BUILD_DATE
ARG VERSION
ARG BUILDARCH
ARG TARGETARCH

LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"
WORKDIR /tmp

RUN dnf install -y wget && \
      case ${TARGETARCH} in \
            "amd64")  DOWNLOAD_ARCH=x86_64  ;; \
            "arm64")  DOWNLOAD_ARCH=aarch64  ;; \
        esac \
        && wget -O /tmp/openlens.rpm https://github.com/MuhammedKalkan/OpenLens/releases/download/v6.5.2-366/OpenLens-6.5.2-366.${DOWNLOAD_ARCH}.rpm



RUN \
  echo "**** install packages ****" && \
  rpm --import https://packages.microsoft.com/keys/microsoft.asc && \
  echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo && \
  wget -o /tmp/k9s.tar.gz https://github.com/derailed/k9s/releases/download/v0.27.4/k9s_Linux_$BUILDARCH.tar.gz && \
  dnf install -y --setopt=install_weak_deps=False --best \
    terminator \
    tilix \
    kubernetes-client \
    libreoffice \
    pluma \
    neofetch \
    adwaita-gtk2-theme \
    adwaita-icon-theme \
    papirus-icon-theme \
    thunderbird \
    firefox \
    code && \
  dnf localinstall -y /tmp/openlens.rpm && \
  tar -zxvf /tmp/k9s.tar.gz && \
  mv /tmp/k9s /usr/local/bin && \
  echo "**** cleanup ****" && \
  dnf autoremove -y && \
  dnf clean all && \
  rm -rf \
    /config/.cache \
    /tmp/*

# add local files
COPY /root /

# ports and volumes
EXPOSE 3389
VOLUME /home
VOLUME /config
