# https://github.com/orgs/community/discussions/126419

FROM archlinux:latest

RUN pacman -Sy --noconfirm archlinux-keyring && \
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

RUN cd $HOME && sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay-bin.git && cd yay-bin && makepkg -si --noconfirm && cd .. && rm -rf yay-bin
RUN cd $HOME && git clone https://github.com/kolin63/.dotfiles.git
RUN yay -S --noconfirm $(cat $HOME/.dotfiles/yay/codespaces.txt)
RUN rm $HOME/.bashrc && bash $HOME/.dotfiles/symlink.sh
RUN cd $HOME
