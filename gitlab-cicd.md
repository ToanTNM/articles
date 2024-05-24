# Gitlab CI/CD

## Install

  ```sh
  # Download the binary for your system
  sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
  
  # Give it permission to execute
  sudo chmod +x /usr/local/bin/gitlab-runner
  
  # Create a GitLab Runner user
  sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
  
  # Install and run as a service
  sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
  sudo gitlab-runner start
  ```

or
  ```sh
  curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash

  sudo apt-get install gitlab-runner
  
  systemctl status gitlab-runner
  ```

## Uninstall

  ```sh
  # Stop and remove the service
  sudo gitlab-runner stop
  sudo gitlab-runner uninstall
  sudo systemctl daemon-reload
  
  # Remove gitlab-runner files and config
  sudo rm -rf /usr/local/bin/gitlab-runner
  sudo userdel gitlab-runner
  sudo rm -rf /home/gitlab-runner/
  ```

or

  ```sh
  sudo apt-get remove gitlab-runner
  ```

## Command

  ```sh
  # Register runner
  gitlab-runner register -n -u https://gitlab.com -t <token> --executor shell

  # Unregister runner
  gitlab-runner unregister -t <token>
  
  # View gitlab runner service status
  sudo gitlab-runner status
  
  # View registered runner 
  gitlab-runner list

  # Run registered runner 
  gitlab-runner run

  # Verify and delete unused runner
  gitlab-runner verify --delete

  # Remove all runner
  gitlab-runner unregister --all-runners
  ```
