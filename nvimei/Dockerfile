ARG BASE_IMAGE=ubuntu:22.04
FROM ${BASE_IMAGE}

LABEL maintainer="mei"
ARG USER_NAME=docker
ARG GROUP_NAME=docker
ARG UID=1000
ARG GID=1000
ARG HOME=/home/${USER_NAME}

ENV DEBIAN_FRONTEND="noninteractive" \
    LC_ALL="C.UTF-8" \
    LANG="C.UTF-8" \
    TZ=Asia/Tokyo 

RUN apt update && apt install --no-install-recommends -y \
    sudo git zip unzip curl wget htop vim tmux make ssh openssh-client fzf locales-all 

RUN apt-get update && apt-get install -qqy npm

RUN rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/*

# nvim
RUN apt-get update -y && apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:neovim-ppa/unstable && \
    apt-get update -y && \
    apt-get install -y neovim


# Add user. Without this, following process is executed as admin. 
RUN groupadd -g ${GID} ${GROUP_NAME} \
    && useradd -ms /bin/sh -u ${UID} -g ${GID} ${USER_NAME}
RUN echo 'Defaults visiblepw' >> /etc/sudoers
RUN echo 'docker ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

RUN chown ${USER_NAME}:${GROUP_NAME} ${HOME}

USER ${USER_NAME}
WORKDIR ${HOME}

RUN git clone --depth=1 https://github.com/mei28/dotfiles.git ~/dotfiles && rm -rf ~/dotfiles/.git
RUN bash ~/dotfiles/setup.sh

RUN nvim +:q