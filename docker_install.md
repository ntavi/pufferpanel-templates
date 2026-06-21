Ubuntu 24 Only

First Step
```
  # Add Docker's official GPG key:
  sudo apt update
  sudo apt install ca-certificates curl
  sudo install -m 0755 -d /etc/apt/keyrings
  sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
  sudo chmod a+r /etc/apt/keyrings/docker.asc
  
  # Add the repository to Apt sources:
  sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
  Types: deb
  URIs: https://download.docker.com/linux/ubuntu
  Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
  Components: stable
  Architectures: $(dpkg --print-architecture)
  Signed-By: /etc/apt/keyrings/docker.asc
  EOF
  
  sudo apt update
```

Second Step
```
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Third Step
```
sudo groupadd docker

sudo usermod -aG docker $USER

docker volume create pufferpanel-config

docker volume create pufferpanel-servers

docker create --name pufferpanel \
    --network=host \
    -v pufferpanel-config:/etc/pufferpanel \
    -v pufferpanel-servers:/var/lib/pufferpanel:z \
    -v /var/run/docker.sock:/var/run/docker.sock \
    --restart=on-failure \
    pufferpanel/pufferpanel:latest

docker start pufferpanel
docker exec -it pufferpanel /pufferpanel/bin/pufferpanel user add
```
