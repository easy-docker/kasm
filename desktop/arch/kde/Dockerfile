FROM lscr.io/linuxserver/webtop:arch-kde

LABEL maintainer="Ghostry <ghostry.green@gmail.com>"

# 设置 Arch Linux 镜像源
RUN echo "Server = https://mirrors.aliyun.com/archlinux/\$repo/os/\$arch" > /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.ustc.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.bfsu.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.cernet.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.xjtu.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
    echo "Server = https://mirrors.shanghaitech.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist

# 启用 multilib 仓库
RUN sed -i '/^#$$multilib$$/{N;s/^#//;s/\n#/\n/}' /etc/pacman.conf

# 添加 archlinuxcn 和 arch4edu 仓库
RUN echo '[archlinuxcn]' >> /etc/pacman.conf && \
    echo 'SigLevel = Never' >> /etc/pacman.conf && \
    echo 'Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch' >> /etc/pacman.conf && \
    echo '' >> /etc/pacman.conf && \
    echo '[arch4edu]' >> /etc/pacman.conf && \
    echo 'SigLevel = Never' >> /etc/pacman.conf && \
    echo 'Server = https://mirrors.tuna.tsinghua.edu.cn/arch4edu/$arch' >> /etc/pacman.conf

# 安装软件包（包括 Steam 及其依赖项）
RUN pacman -Syyu --noconfirm && \
    pacman -Sy --noconfirm \
        git base-devel sudo yay sshpass \
        vim htop iotop iftop fuse2 \
        iputils net-tools dnsutils \
        npm maven bash-completion unarchiver \
        steam lib32-mesa lib32-vulkan-radeon lib32-vulkan-intel \
    && pacman -Scc --noconfirm \
    && rm -rf /var/cache/pacman/pkg/*
