# Instruction

1. In window `Powershell` or `cmd`

    Set wsl default version: `wsl --set-default-version 2`

    List wsl distro installed with version info: `wsl -l -v`

    List wsl distro available: `wsl -l -o`

    Change existing wsl to version 2: `wsl --set-version Ubuntu-22.04 2`

    Set wsl distro to default: `wsl --setdefault Ubuntu-22.04`

    Launch and connect to wsl distro: `wsl -d Ubuntu-22.04`

    If error, run command:

    ```bash
    dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All
    bcdedit /set hypervisorlaunchtype auto
    ```

1. On `wsl`

    Update available package to new version:

    ```bash
    sudo apt update
    sudo apt upgrade
    ```

    Install docker:

    ```bash
    curl -fsSL <https://get.docker.com> -o get-docker.sh
    sudo sh ./get-docker.sh --dry-run
    ```

    Add docker run command as root user:

    ```bash
    sudo groupadd docker
    sudo usermod -aG docker $USER
    ```

    Add github ssh key:

    ```bash
    ssh-keygen -t ed25519 -C "tnmtoan@yahoo.com"
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_ed25519
    cat ~/.ssh/id_ed25519.pub #copy this result and add to github
    ```

    Install `sdk`

    ```bash
    curl -s "https://get.sdkman.io" | bash
    source "/home/tps/.sdkman/bin/sdkman-init.sh"
    ```
