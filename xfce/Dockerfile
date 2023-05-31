FROM docker.io/linuxserver/rdesktop:fedora-xfce

# set version label
ARG BUILD_DATE
ARG VERSION
LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"

RUN \
  echo "**** install packages ****" && \
  rpm --import https://packages.microsoft.com/keys/microsoft.asc && \
  echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo && \
  dnf install -y --setopt=install_weak_deps=False --best \
    chromium \
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
  echo "**** cleanup ****" && \
  dnf autoremove -y && \
  dnf clean all && \
  rm -rf \
    /tmp/*

# Remove this due to errors
RUN dnf -y remove xfce4-power-manager && rm -rf /etc/xdg/autostart/xfce-polkit.desktop && dnf clean all

# add local files
COPY /root /

# ports and volumes
EXPOSE 3389
VOLUME /home
VOLUME /config
