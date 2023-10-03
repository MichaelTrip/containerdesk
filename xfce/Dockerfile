FROM docker.io/linuxserver/rdesktop:fedora-xfce

# set version label
ARG BUILDARCH
ARG TARGETARCH
ARG BUILD_DATE
ARG VERSION
LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"
WORKDIR /tmp

RUN dnf install -y wget
RUN \
    if [ "${TARGETARCH}" == "amd64" ]; then \
        wget -O /tmp/openlens.rpm https://github.com/MuhammedKalkan/OpenLens/releases/download/v6.5.2-366/OpenLens-6.5.2-366.x86_64.rpm; \
    elif [ "${TARGETARCH}" == "arm64" ]; then \
        wget -O /tmp/openlens.rpm https://github.com/MuhammedKalkan/OpenLens/releases/download/v6.5.2-366/OpenLens-6.5.2-366.aarch64.rpm; \
    else \
        echo "Unsupported architecture: ${TARGETARCH}"; \
        exit 1; \
    fi

RUN \
  echo "**** install packages ****" && \
  rpm --import https://packages.microsoft.com/keys/microsoft.asc && \
  echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo && \
  echo "Install and download k9s" && \
  wget -o /tmp/k9s.tar.gz https://github.com/derailed/k9s/releases/download/v0.27.4/k9s_Linux_${TARGETARCH}.tar.gz && \
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
# Remove this due to errors
RUN dnf -y remove xfce4-power-manager && rm -rf /etc/xdg/autostart/xfce-polkit.desktop && dnf clean all


# add local files
COPY /root /


# ports and volumes
EXPOSE 3389
VOLUME /home
VOLUME /config
