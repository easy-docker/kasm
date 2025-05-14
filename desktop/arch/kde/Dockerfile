FROM lscr.io/linuxserver/webtop:arch-kde

LABEL maintainer="Ghostry <ghostry.green@gmail.com>"
  
RUN pacman -Syyu --noconfirm && \
  pacman -Sy --noconfirm vim htop iotop iftop iputils net-tools dnsutils base-devel

RUN pacman -Sy --noconfirm git fcitx5 fcitx5-gtk fcitx5-qt fcitx5-configtool fcitx5-config-qt fcitx5-chinese-addons \
  fcitx5-pinyin-zhwiki yay

RUN pacman -Sy --noconfirm npm maven bash-completion

RUN yay -S --noconfirm --needed google-chrome
