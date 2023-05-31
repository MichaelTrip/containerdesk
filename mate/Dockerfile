FROM docker.io/linuxserver/rdesktop:fedora-mate

# set version label
ARG BUILD_DATE
ARG VERSION
LABEL build_version="based on Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="Michael Trip"

COPY /root /
RUN \
  echo "**** install packages ****" && \
  rpm --import https://packages.microsoft.com/keys/microsoft.asc && \
  echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo && \
  dnf install -y --setopt=install_weak_deps=False --best \
    terminator \
    tilix \
    chromium \
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
    echo "**** application tweaks ****" && \
    sed -i \
      's#^Exec=.*#Exec=/usr/local/bin/wrapped-chromium#g' \
      /usr/share/applications/chromium-browser.desktop && \
    mv /usr/bin/exo-open /usr/bin/exo-open-real && \
    echo "**** xfce tweaks ****" && \
    rm -f \
      /etc/xdg/autostart/xfce-polkit.desktop && \
    echo "**** cleanup ****" && \
    dnf autoremove -y && \
    dnf clean all && \
    rm -rf \
      /config/.cache \
      /tmp/*


# add local files

# ports and volumes
EXPOSE 3389
VOLUME /home
VOLUME /config
