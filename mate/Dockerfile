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
  echo "**** mate tweaks ****" && \
  rm -f \
    /etc/xdg/autostart/at-spi-dbus-bus.desktop \
    /etc/xdg/autostart/gnome-keyring-pkcs11.desktop \
    /etc/xdg/autostart/gnome-keyring-secrets.desktop \
    /etc/xdg/autostart/gnome-keyring-ssh.desktop \
    /etc/xdg/autostart/mate-power-manager.desktop \
    /etc/xdg/autostart/mate-screensaver.desktop \
    /etc/xdg/autostart/polkit-mate-authentication-agent-1.desktop && \
  sed -i \
    '/compositing-manager/{n;s/.*/      <default>false<\/default>/}' \
    /usr/share/glib-2.0/schemas/org.mate.marco.gschema.xml && \
    glib-compile-schemas /usr/share/glib-2.0/schemas/ && \
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
