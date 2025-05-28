FROM lscr.io/linuxserver/webtop:arch-kde

LABEL maintainer="Ghostry <ghostry.green@gmail.com>"

RUN echo "Server = https://mirrors.cernet.edu.cn/archlinux/\$repo/os/\$arch" > /etc/pacman.d/mirrorlist && \
  echo "Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
  echo "Server = https://mirrors.ustc.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
  echo "Server = https://mirrors.bfsu.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
  echo "Server = https://mirrors.aliyun.com/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
  echo "Server = https://mirrors.xjtu.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
  echo "Server = https://mirrors.shanghaitech.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
  echo '[archlinuxcn]' >> /etc/pacman.conf && \
  echo 'SigLevel = Never' >> /etc/pacman.conf && \
  echo 'Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch' >> /etc/pacman.conf && \
  echo '' >> /etc/pacman.conf && \
  echo '[arch4edu]' >> /etc/pacman.conf && \
  echo 'SigLevel = Never' >> /etc/pacman.conf && \
  echo 'Server = https://mirrors.tuna.tsinghua.edu.cn/arch4edu/$arch' >> /etc/pacman.conf

RUN pacman -Syyu --noconfirm && \
    pacman -Sy --noconfirm \
    git base-devel sudo yay sshpass \
    vim htop iotop iftop fuse2 \
    iputils net-tools dnsutils opencv openmpi hdf5 vtk abseil-cpp protobuf \
    npm maven bash-completion unarchiver \
    fcitx5 fcitx5-gtk fcitx5-qt \
    fcitx5-configtool fcitx5-config-qt \
    fcitx5-chinese-addons fcitx5-pinyin-zhwiki && \
    mkdir -p /build && chown abc:abc /build && \
    cd /build && git clone https://aur.archlinux.org/google-chrome.git && \
    chown -R abc:abc google-chrome && \
    su - abc -c "cd /build/google-chrome && makepkg -si --noconfirm --needed --skippgpcheck" && \
    rm -rf /build && \
    pacman -Scc --noconfirm && \
    rm -rf /var/cache/pacman/pkg/*
