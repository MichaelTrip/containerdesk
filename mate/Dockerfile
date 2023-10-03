FROM docker.io/linuxserver/rdesktop:fedora-mate

# set version label
ARG BUILD_DATE
ARG VERSION
LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"
WORKDIR /tmp
RUN \
  echo "**** install packages ****" && \
  rpm --import https://packages.microsoft.com/keys/microsoft.asc && \
  echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo && \
  dnf install -y wget && \
  echo "Install and download OpenLens" && \
  if [ "$BUILDARCH" = "amd64" ]; then wget -O https://github.com/MuhammedKalkan/OpenLens/releases/download/v6.5.2-366/OpenLens-6.5.2-366.x86_64.rpm; elif [ "$BUILDARCH" = "arm64" ]; then wget -O https://github.com/MuhammedKalkan/OpenLens/releases/download/v6.5.2-366/OpenLens-6.5.2-366.aarch64.rpm; fi && \
  echo "Install and download k9s" && \
  wget https://github.com/derailed/k9s/releases/download/v0.27.4/k9s_Linux_$BUILDARCH.tar.gz && \
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
  if [ "$BUILDARCH" = "amd64" ]; dnf localinstall -y /tmp/OpenLens-6.5.2-366.aarch64.rpm; elif [ "$BUILDARCH" = "arm64" ]; then dnf localinstall -y /tmp/OpenLens-6.5.2-366.aarch64.rpm; fi && \
  tar -zxvf /tmp/k9s_Linux_$BUILDARCH.tar.gz && \
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
