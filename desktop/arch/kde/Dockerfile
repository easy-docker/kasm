FROM lscr.io/linuxserver/webtop:arch-kde

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
        nvidia nvidia-utils \
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
