FROM ghcr.io/linuxserver/rdesktop:fedora-mate

# set version label
ARG BUILD_DATE
ARG VERSION
ARG TARGETARCH
ARG SEABIRD_VERSION=0.6.0
ARG K9S_VERSION=0.50.9
LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"
WORKDIR /tmp

RUN dnf install -y wget curl
RUN \
      case ${TARGETARCH} in \
            "amd64")  DOWNLOAD_ARCH=linux-amd64  ;; \
            "arm64")  DOWNLOAD_ARCH=linux-arm64  ;; \
        esac \
        && curl -L https://github.com/freelensapp/freelens/releases/download/v1.5.3/Freelens-1.5.3-${DOWNLOAD_ARCH}.rpm --output /tmp/freelens.rpm

RUN curl -L https://github.com/derailed/k9s/releases/download/v${K9S_VERSION}/k9s_Linux_${TARGETARCH}.tar.gz --output /tmp/k9s.tar.gz
RUN curl -L https://github.com/getseabird/seabird/releases/download/v${SEABIRD_VERSION}/seabird_linux_${TARGETARCH}.tar.gz --output /tmp/seabird.tar.gz

RUN echo "**** import Microsoft GPG key ****" \
  && rpm --import https://packages.microsoft.com/keys/microsoft.asc

RUN echo "**** add VS Code repo ****" \
  && echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo

RUN echo "**** install packages ****" \
  && dnf install -y --setopt=install_weak_deps=False --best \
    terminator \
    tilix \
    caja \
    caja-sendto \
    caja-actions \
    caja-schemas \
    kubernetes-client \
    libreoffice \
    pluma \
    neofetch \
    adwaita-gtk2-theme \
    adwaita-icon-theme \
    papirus-icon-theme \
    pop-gtk2-theme \
    pop-gtk3-theme \
    gtksourceview5 \
    libvterm \
    vte291-gtk4 \
    thunderbird \
    firefox \
    code

RUN echo "**** install freelens ****" \
  && dnf localinstall -y /tmp/freelens.rpm

RUN echo "**** extract k9s ****" \
  && tar -zxvf /tmp/k9s.tar.gz

RUN echo "**** extract seabird ****" \
  && tar -zxvf /tmp/seabird.tar.gz

RUN echo "**** move k9s and seabird to /usr/local/bin ****" \
  && mv /tmp/k9s /usr/local/bin \
  && mv /tmp/seabird /usr/local/bin

RUN echo "**** cleanup ****" \
  && dnf autoremove -y \
  && dnf clean all \
  && rm -rf /config/.cache /tmp/*

# add local files
COPY /root /

# ports and volumes
EXPOSE 3389
VOLUME /home
VOLUME /config
