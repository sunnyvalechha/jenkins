# jenkins

vim docker.sh

    Docker Install Ubuntu:-

    apt-get remove docker docker-engine docker.io containerd runc -y
    apt-get update -y
    # Add Docker's official GPG key:
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    # Add the repository to Apt sources:
    echo   "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null   
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
    docker version
    docker --version
    docker run hello-world
    systemctl status docker
    systemctl start docker
    systemctl status docker
    docker run hello-world

Install Docker Compose


    docker search jenkins

    docker pull jenkins/jenkins

    docker images

    
    
