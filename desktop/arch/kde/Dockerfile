FROM ghcr.io/linuxserver/baseimage-kasmvnc:arch

# set version label
ARG BUILD_DATE
ARG VERSION
LABEL build_version="Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="thelamer"

# title
ENV TITLE="Arch KDE"

RUN \
  echo "**** add icon ****" && \
  curl -o \
    /kclient/public/icon.png \
    https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/webtop-logo.png && \
  echo "**** install packages ****" && \
  pacman -Sy --noconfirm --needed \
    chromium \
    dolphin \
    kate \
    konsole \
    plasma-desktop && \
  echo "**** application tweaks ****" && \
  sed -i \
    's#^Exec=.*#Exec=/usr/local/bin/wrapped-chromium#g' \
    /usr/share/applications/chromium.desktop && \
  echo "**** kde tweaks ****" && \
  sed -i \
    's/applications:org.kde.discover.desktop,/applications:org.kde.konsole.desktop,/g' \
    /usr/share/plasma/plasmoids/org.kde.plasma.taskmanager/contents/config/main.xml && \
  echo "**** cleanup ****" && \
  rm -rf \
    /config/.cache \
    /tmp/* \
    /var/cache/pacman/pkg/* \
    /var/lib/pacman/sync/*

# add local files
COPY root /

# ports and volumes
EXPOSE 3000

VOLUME /config
# https://github.com/linuxserver/docker-webtop/blob/cb209db0cbc921ab1c5d19795a14d87749a40695/Dockerfile 因上游镜像不再支持kasm,从上有拷贝代码到此.
LABEL maintainer="Ghostry <ghostry.green@gmail.com>"

# 启用 multilib 仓库
RUN echo '[multilib]' >> /etc/pacman.conf && \
    echo 'Include = /etc/pacman.d/mirrorlist' >> /etc/pacman.conf

# 添加 archlinuxcn 和 arch4edu 仓库
RUN echo '[archlinuxcn]' >> /etc/pacman.conf && \
    echo 'SigLevel = Never' >> /etc/pacman.conf && \
    echo 'Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch' >> /etc/pacman.conf && \
    echo '' >> /etc/pacman.conf && \
    echo '[arch4edu]' >> /etc/pacman.conf && \
    echo 'SigLevel = Never' >> /etc/pacman.conf && \
    echo 'Server = https://mirrors.tuna.tsinghua.edu.cn/arch4edu/$arch' >> /etc/pacman.conf

# 同步包数据库并安装软件包
RUN pacman -Syyu --noconfirm \
    && pacman -Sy --noconfirm \
        git base-devel sudo yay sshpass \
        vim htop iotop iftop fuse2 \
        iputils net-tools dnsutils \
        npm maven bash-completion unarchiver \
    && (mkdir -p /tmp/build && cd /tmp/build \
        && git clone https://aur.archlinux.org/wps-office-cn.git \
        && chown -R abc:abc wps-office-cn \
        && cd wps-office-cn \
        && sudo -u abc makepkg -si --noconfirm) \
    && (mkdir -p /tmp/build && cd /tmp/build \
        && git clone https://aur.archlinux.org/ttf-wps-fonts.git \
        && chown -R abc:abc ttf-wps-fonts \
        && cd ttf-wps-fonts \
        && sudo -u abc makepkg -si --noconfirm) \
    && rm -rf /tmp/build \
    && pacman -Scc --noconfirm \
    && rm -rf /var/cache/pacman/pkg/*

# 设置 Arch Linux 镜像源
RUN echo "Server = https://mirrors.aliyun.com/archlinux/\$repo/os/\$arch" > /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.ustc.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.bfsu.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.cernet.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.xjtu.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.shanghaitech.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist
