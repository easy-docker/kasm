FROM lscr.io/linuxserver/webtop:arch-kde

LABEL maintainer="Ghostry <ghostry.green@gmail.com>"

# 系统更新及安装必要工具
RUN pacman -Syyu --noconfirm
RUN pacman -Sy --noconfirm \
    git base-devel sudo \
    vim htop iotop iftop \
    iputils net-tools dnsutils \
    npm maven bash-completion \
    fcitx5 fcitx5-gtk fcitx5-qt \
    fcitx5-configtool fcitx5-config-qt \
    fcitx5-chinese-addons fcitx5-pinyin-zhwiki

RUN mkdir -p /build && chown abc:abc /build

RUN cd /build && git clone https://aur.archlinux.org/google-chrome.git && \
    chown -R abc:abc google-chrome && \
    su - abc -c "cd /build/google-chrome && makepkg -si --noconfirm --needed --skippgpcheck"

# 清理构建文件
RUN rm -rf /build && \
    pacman -Scc --noconfirm && \
    rm -rf /var/cache/pacman/pkg/*
