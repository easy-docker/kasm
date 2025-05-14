FROM lscr.io/linuxserver/webtop:arch-kde as builder

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

RUN pacman -Syyu --noconfirm

RUN pacman -Sy --noconfirm \
    git base-devel sudo yay \
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
    
FROM scratch
LABEL maintainer="Ghostry <ghostry.green@gmail.com>"

COPY --from=builder / /

ENV PS1=$(whoami)@$(hostname):$(pwd)\$  HOME=/root TERM=xterm S6_CMD_WAIT_FOR_SERVICES_MAXTIME=0 S6_VERBOSITY=1 S6_STAGE2_HOOK=/docker-mods VIRTUAL_ENV=/lsiopy PATH=/lsiopy/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

ENV DISPLAY=:1 PERL5LIB=/usr/local/bin OMP_WAIT_POLICY=PASSIVE GOMP_SPINCOUNT=0 HOME=/config START_DOCKER=true PULSE_RUNTIME_PATH=/defaults NVIDIA_DRIVER_CAPABILITIES=all

EXPOSE 3000/tcp 3001/tcp

VOLUME [/config]

ENTRYPOINT ["/init"]

