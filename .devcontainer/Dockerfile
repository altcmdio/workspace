ARG VARIANT=dev-14-bullseye
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:${VARIANT}

# install packages required to install docker
RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive \
    apt-get install -y --no-install-recommends \
      httpie \
      inetutils-ping \
      lsb-release \
  && rm -rf /var/lib/apt/lists/*

# install docker cli and docker compose (as a plugin)
RUN apt-get update \
  && curl -fsSL https://download.docker.com/linux/$(lsb_release -is | \
    tr '[:upper:]' '[:lower:]')/gpg | \
    gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg \
  && echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian  $(lsb_release -cs) stable" | \
    tee /etc/apt/sources.list.d/docker.list > /dev/null \
  && apt-get update \
  && DEBIAN_FRONTEND=noninteractive \
    apt-get install -y --no-install-recommends \
      docker-ce \
      docker-ce-cli \
      docker-compose-plugin \
      containerd.io \
  && rm -rf /var/lib/apt/lists/* \
  && usermod -aG docker node

# install optional-extra packages
RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive \
    apt-get install -y --no-install-recommends \
      httpie \
      inetutils-ping \
      jq \
      vim \
  && rm -rf /var/lib/apt/lists/*

# set zsh as the default shell for the node user
RUN chsh -s /bin/zsh node

# switch to node user
USER node

# install and enable zsh plugins for node user
RUN PLUGINS=${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins \
  && cd "${PLUGINS}" \
  && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git \
  && git clone https://github.com/zsh-users/zsh-autosuggestions \
  && git clone https://github.com/MichaelAquilina/zsh-you-should-use.git you-should-use \
  && sed -i "s/^plugins=(git)/plugins=(git docker yarn you-should-use zsh-autosuggestions zsh-syntax-highlighting)/" ${HOME}/.zshrc

# customise $PROMPT for the node user
RUN echo "export PROMPT='%F{33}λ %m%f %F{208}($(arch))%f %F{33}%(5~|../%4~|%~)/%f $(git_prompt_info)%{$reset_color%}'" >> ${HOME}/.zshrc

# un-install global npm modules inherited from the parent image
RUN npm un -g eslint

# install additional global npm modules
RUN npm i -g \
  eslint \
  nodemon \
  tiged \
  prettier

# Stop the annoying message when you start a shell
ENV IGNORE_NOTICE=true

# start in the /workspace directory by default
WORKDIR /workspace
