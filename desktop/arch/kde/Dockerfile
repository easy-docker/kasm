FROM ghcr.io/linuxserver/baseimage-kasmvnc:arch

# set version label
ARG BUILD_DATE
ARG VERSION
LABEL build_version="Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="thelamer"

# title
ENV TITLE="Arch KDE"

# 更新
RUN pacman -Syyu --noconfirm \
    && pacman -Scc --noconfirm \
    && rm -rf /var/cache/pacman/pkg/*

COPY desktop/arch/kde/webtop-logo.png /kclient/public/icon.png

RUN pacman -Sy --noconfirm --needed \
    kwin-x11 plasma-desktop && \
  echo "**** cleanup ****" && \
  rm -rf \
    /config/.cache \
    /tmp/* \
    /var/cache/pacman/pkg/* \
    /var/lib/pacman/sync/*

RUN pacman -Sy --noconfirm --needed \
    dolphin ark dolphin-plugins unarchiver \
    kate \
    konsole && \
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

RUN pacman -Sy --noconfirm --needed \
    chromium && \
  echo "**** application tweaks ****" && \
  sed -i \
    's#^Exec=.*#Exec=/usr/local/bin/wrapped-chromium#g' \
    /usr/share/applications/chromium.desktop && \
  echo "**** cleanup ****" && \
  rm -rf \
    /config/.cache \
    /tmp/* \
    /var/cache/pacman/pkg/* \
    /var/lib/pacman/sync/*

# add local files
COPY desktop/arch/kde/root /
RUN chmod 755 /defaults/startwm.sh && chmod 755 /usr/local/bin/wrapped-chromium
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

# 安装基础包
RUN pacman -Sy --noconfirm \
        git base-devel sudo yay sshpass \
        vim htop iotop iftop xorg-xkill \
        iputils net-tools dnsutils \
        zsh terminator \
    && pacman -Scc --noconfirm \
    && rm -rf /var/cache/pacman/pkg/*

# 安装wps
RUN (mkdir -p /tmp/build && cd /tmp/build \
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

# 安装软件包
RUN pacman -Sy --noconfirm \
        npm maven \
    && npm install -g pnpm \
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

# 修改 /etc/locale.gen
RUN echo 'zh_CN.UTF-8 UTF-8' >> /etc/locale.gen \
    && locale-gen
