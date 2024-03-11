# VS code notes
---

## Create dev environment with devcontainer

- Dockerfile

```Dockerfile
FROM ubuntu

ENV DEBIAN_FRONTEND=noninteractive
ARG USER=admin
ARG NODE_VERSION=21

RUN apt-get update && \
    apt-get install -y \
    htop \
    lsof \
    curl \
    git \
    sudo \
    vim \
    software-properties-common \
    wget && \
    rm -rf /var/lib/apt/lists/*

# Install docker
RUN curl -sSL https://get.docker.com/ | sh

RUN apt-add-repository -y ppa:fish-shell/release-3 \
    && apt-get install -y fish

RUN useradd --groups sudo --shell /bin/bash ${USER} \
    && sudo usermod -aG docker ${USER} \
    && echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
    && chmod 0440 /etc/sudoers.d/${USER}
USER ${USER}
WORKDIR /home/${USER}

ENV SHELL /usr/bin/fish
ENV LANG=C.UTF-8 LANGUAGE=C.UTF-8 LC_ALL=C.UTF-8
SHELL ["fish", "--command"]

RUN curl -sSL https://get.oh-my.fish | fish -C 'set argv --noninteractive'

# Install starship
RUN curl -sS https://starship.rs/install.sh | sudo sh -s -- -y
RUN echo 'starship init fish | source' >> ~/.config/fish/config.fish

# Install nvm
RUN omf install nvm \
    && curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# Install nodejs and tools
RUN npm install -g pnpm

# Install code-server
RUN curl -Lk 'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64' --output vscode_cli.tar.gz \
    && pwd && ls . \
    && sudo tar -xf vscode_cli.tar.gz -C /usr/bin \
    && rm vscode_cli.tar.gz

```

- devcontainer.json

```json
{
  "name": "Ubuntu",
  // Or use a Dockerfile or Docker Compose file. More info: https://containers.dev/guide/dockerfile
  "runArgs": ["--name", "${localEnv:USER}_devcontainer"],
  "mounts": [
    {
      "source": "/var/run/docker.sock",
      "target": "/var/run/docker.sock",
      "type": "bind"
    }
  ],
  "build": {
    "dockerfile": "Dockerfile",
    "args": {
      "USER": "dev",
      "NODE_VERSION": "21"
    }
  },
  "remoteUser": "dev",

  // change workspaces directory, default is /workspaces. If not, comment 2 below lines
  "workspaceFolder": "/home/dev/workspaces",
  "workspaceMount": "source=${localWorkspaceFolder},target=${containerWorkspaceFolder},type=bind,consistency=cached",

  // Configure tool-specific properties.
  "customizations": {
    "vscode": {
      "extensions": [
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "ms-azuretools.vscode-docker",
        "ms-vscode-remote.remote-containers",
        "ms-vscode-remote.remote-ssh",
        "ms-vscode-remote.remote-ssh-edit",
        "ms-vscode.remote-explorer",
        "liviuschera.noctis",
        "bradlc.vscode-tailwindcss",
        "rangav.vscode-thunder-client",
        "tabnine.tabnine-vscode"
      ],
      "settings": {
        "workbench.colorTheme": "Noctis Azureus",
        "editor.wordWrap": "on",
        "editor.fontLigatures": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode",
        "editor.tabSize": 2,
        "editor.formatOnType": true,
        "editor.formatOnPaste": true,
        "editor.formatOnSave": true,
        "editor.semanticTokenColorCustomizations": {
          "enabled": true
        },
        "files.autoSave": "off",
        "git.enableSmartCommit": true,
        "git.confirmSync": false,
        "git.autofetch": true,
        "javascript.updateImportsOnFileMove.enabled": "always",
        "terminal.integrated.fontFamily": "Consolas, 'Courier New', monospace, 'Symbols Nerd Font', 'Symbols Nerd Font Mono'",
        "terminal.integrated.defaultProfile.linux": "fish",
        "tabnine.experimentalAutoImports": true
      }
    }
  }
}
```

## Terminal font and default terminal

  ```json
  {
    # install font at https://www.nerdfonts.com/font-downloads
    "terminal.integrated.fontFamily": "Consolas, 'Courier New', monospace, 'Symbols Nerd Font', 'Symbols Nerd Font Mono'",
    "terminal.integrated.fontSize": 14,
    "terminal.integrated.defaultProfile.linux": "fish"
  }
  ```
