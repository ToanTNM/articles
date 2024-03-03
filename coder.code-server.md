
# Setup environment

## Install tools

### New: Install by modify Dockerfile

Dockerfile - v1.91 - 2024-03-01 22:30

  ```Dockerfile
  FROM ubuntu
  
  # User pass from local.username in `main.tf`
  ARG USER=coder 
  ARG NODE_VERSION=21
  
  RUN apt-get update \
      && apt-get install -y \
      curl \
      git \
      sudo \
      vim \
      wget \
      && rm -rf /var/lib/apt/lists/*
  
  # Install docker
  RUN curl -sSL https://get.docker.com/ | sh
  
  RUN useradd --groups sudo --shell /bin/bash ${USER} \
      && sudo usermod -aG docker ${USER} \
      && echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
      && chmod 0440 /etc/sudoers.d/${USER}
  USER ${USER}
  WORKDIR /home/${USER}
  
  RUN touch ~/.bashrc
  
  # Install nvm
  RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
  
  # Install nodejs and tools
  RUN bash -c 'source $HOME/.nvm/nvm.sh \
      && nvm install $NODE_VERSION \
      && npm install -g pnpm'
  
  # Install starship
  RUN curl -sS https://starship.rs/install.sh | sudo sh -s -- -y
  RUN echo 'eval "$(starship init bash)"' >> ~/.bashrc
  ```

---
### Old: Manual install following below

####  starship

  ```sh
  curl -sS https://starship.rs/install.sh > starship.sh
  sudo sh starship.sh

  // move starship to ~/bin
  mkdir ~/bin
  mv /usr/local/bin/starship ~/bin
  vi ~/.bashrc

  // add this line to ~/.bashrc
  PATH="/home/$USER/bin:$PATH"
  eval "$(starship init bash)"
  ```
  
#### Install `nvm`, `nodejs`, `pnpm`

  ```sh
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
  nvm -v
  // if nvm not found, add to ~/.bashrc
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
  [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

  // install nodejs
  nvm install 21

  // install pnpm
  npm install -g pnpm
  // or
  wget -qO- https://get.pnpm.io/install.sh | ENV="$HOME/.bashrc" SHELL="$(which bash)" bash -
  ```

`.bashrc` after config

  ```sh
  if [ -d "$HOME/bin" ] ; then
    PATH="$PATH:$HOME/bin"
  fi
  
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
  [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
  
  eval "$(starship init bash)"
  
  # pnpm
  export PNPM_HOME="/home/toantran/.local/share/pnpm"
  case ":$PATH:" in
    *":$PNPM_HOME:"*) ;;
    *) export PATH="$PNPM_HOME:$PATH" ;;
  esac
  # pnpm end
  ```

---

## Install `code-server` extension

### Install extension

  ```sh
  // Launch VS Code Quick Open (Ctrl+P)
  code-server --install-extension dbaeumer.vscode-eslint
  code-server --install-extension humao.rest-client

  // Launch VS Code Quick Open (Ctrl+P)
  ext install dbaeumer.vscode-eslint humao.rest-client
  ```

### Install downloaded `vsix` extension

  ```sh
  // Download and copy extension to server
  scp D:\Downloads\liviuschera.noctis-10.43.1.vsix tpsc@tps-dev-srv.go9.me:~/liviuschera.noctis-10.43.1.vsix
  ```

  ```sh
  // ssh to server and copy extension to docker host
  docker cp liviuschera.noctis-10.43.1.vsix coder-toantran-dev:/home/toantran/data
  ```

  ```sh
  // Download and copy extension to server
  code-server --install-extension /data/liviuschera.noctis-10.43.1.vsix

  // Or Launch VS Code Quick Open (Ctrl+P)
  ext install /data/liviuschera.noctis-10.43.1.vsix
  ```

## Config settings

Import `toan` profile or do it manual: 

Open `Preferences: Open Remote Settings (JSON)`, copy below to `settings.json`

  ```json
  {
    "editor.tokenColorCustomizations": {
      "comments": "#15ac31"
    },
    "editor.semanticTokenColorCustomizations": {
      "enabled": true
    },
    "workbench.colorCustomizations": {
      "[Noctis Azureus]": {
        "activityBar.foreground": "#49ace9"
      }
    },
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.tabSize": 2,
    "editor.formatOnType": true,
    "editor.formatOnPaste": true,
    "editor.formatOnSave": true
  }
  ```

## Add git ssh key

  ```sh
  ssh-keygen -t rsa -b 4096 -C "name"
  // copy content in `.ssh/id_rsa.pub` to github or gitlab
  ```
