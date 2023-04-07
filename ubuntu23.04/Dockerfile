FROM ubuntu:23.04
MAINTAINER "Lixin Wei"

# common libs
RUN apt update && \
    apt install -y apt-utils man-db && \ 
    yes | unminimize && \
    apt upgrade -y && \
    apt clean

# sshd
RUN mkdir /run/sshd; \
    apt update && apt install -y openssh-server; \
    sed -Ei 's/^#(PermitRootLogin) .*/\1 yes/' /etc/ssh/sshd_config; \
    sed -Ei 's/^#(AllowAgentForwarding) .*/\1 yes/' /etc/ssh/sshd_config; \
    apt clean;

# entrypoint
RUN { \
    echo '#!/bin/bash -eu'; \
    echo 'ln -fs /usr/share/zoneinfo/${TZ} /etc/localtime'; \
    echo 'echo "root:${ROOT_PASSWORD}" | chpasswd'; \
    echo 'exec "$@"'; \
    } > /usr/local/bin/entry_point.sh; \
    chmod +x /usr/local/bin/entry_point.sh;

ENV TZ Asia/Shanghai

ENV ROOT_PASSWORD root

EXPOSE 22

ENTRYPOINT ["entry_point.sh"]

CMD    ["/usr/sbin/sshd", "-D", "-e"]

# -----------custom libs------------
RUN apt install -y tzdata zsh fzf git tmux vim net-tools curl && \
    apt clean

# zsh
RUN chsh -s /usr/bin/zsh
# oh-my-zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
    git clone --depth=1 https://github.com/mbenford/zsh-tmux-auto-title ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-tmux-auto-title
# oh-my-zsh config
COPY zshrc /root/.zshrc
# vimrc
RUN wget https://raw.githubusercontent.com/amix/vimrc/master/vimrcs/basic.vim -O /root/.vimrc

