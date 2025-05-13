FROM lscr.io/linuxserver/webtop:arch-kde

LABEL maintainer="Ghostry <ghostry.green@gmail.com>"

RUN echo "Server = https://mirrors.cernet.edu.cn/archlinux/\$repo/os/\$arch" >> /etc/pacman.d/mirrorlist && \
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
  pacman -Sy --noconfirm vim htop iotop iftop iputils net-tools dnsutils
RUN pacman -Sy --noconfirm git fcitx5 fcitx5-gtk fcitx5-qt fcitx5-configtool fcitx5-config-qt fcitx5-chinese-addons \
  fcitx5-pinyin-zhwiki yay
# 在 Dockerfile 中安装并配置 dbus
RUN pacman -Sy --noconfirm npm maven
