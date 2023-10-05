ARG BASE_IMAGE=ubuntu:22.04
FROM ${BASE_IMAGE}

LABEL maintainer="mei"
ARG USER_NAME=docker
ARG GROUP_NAME=docker
ARG UID=1000
ARG GID=1000
ARG HOME=/home/${USER_NAME}
ARG PYTHON_VERSION=3.10

ENV DEBIAN_FRONTEND="noninteractive" \
    LC_ALL="C.UTF-8" \
    LANG="C.UTF-8" \
    TZ=Asia/Tokyo \
    PYTHONPATH=${APPLICATION_DIRECTORY} \
    PYTHONUTF8=1

RUN apt update && apt install --no-install-recommends -y \
    sudo git zip unzip curl wget htop vim tmux make ssh openssh-client fzf locales-all build-essential \
    python${PYTHON_VERSION} python3-pip python-is-python3 python${PYTHON_VERSION}-venv


ARG NODE_MAJOR=20
RUN apt-get update && \
    apt-get install -qqy ca-certificates curl gnupg && \
    mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg && \
    echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list && \
    apt-get update && apt-get install nodejs -y



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

RUN python -m venv ~/.venv

RUN nvim +:q
