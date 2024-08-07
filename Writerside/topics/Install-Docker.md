# Install Docker


install docker in linux
```bash 
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

after installation, you can start and enable docker service
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

verifiy docker installation
```bash
docker --version

# you can also run hello-world image to verify installation
docker run hello-world
```

after you have installed docker, you can add your user to docker group to run docker commands without sudo
```bash
newgrp docker
```


if installation is wrong, you can remove docker and reinstall it
```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

sudo rm -rf /var/lib/docker
sudo rm -rf /etc/docker
sudo rm -rf ~/.docker
```
