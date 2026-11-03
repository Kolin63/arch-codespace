# https://github.com/orgs/community/discussions/126419

FROM archlinux:latest

RUN pacman-key --init && \
    pacman -Sy --noconfirm archlinux-keyring && \
    pacman -Syu --noconfirm && \
    pacman -S --noconfirm base-devel git sudo

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000
RUN groupadd -g $USER_GID $USERNAME && \
    useradd -m -u $USER_UID -g $USER_GID $USERNAME && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/90-$USERNAME && \
    chmod 0440 /etc/sudoers.d/90-$USERNAME

USER $USERNAME
WORKDIR /workspaces

RUN cd /home/vscode && \
    sudo pacman -S --needed git base-devel && \
    git clone https://aur.archlinux.org/yay-bin.git && \
    cd yay-bin && makepkg -si --noconfirm && cd .. && \
    rm -rf yay-bin && \
    cd /home/vscode && git clone https://github.com/kolin63/.dotfiles.git && \
    yay -S --noconfirm $(cat /home/vscode/.dotfiles/yay/codespaces.txt) && \
    rm /home/vscode/.bashrc && bash /home/vscode/.dotfiles/symlink.sh && \
    cp /workspaces/arch-codespace/lbashrc /home/vscode/.dotfiles/lbashrc
