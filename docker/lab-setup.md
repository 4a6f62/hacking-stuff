# Setting up a lab in Docker
I'm using Debian/Kali, if you're using a different distro commands might differ.

## It's good hygiene to update your machine before doing stuff
```console
sudo apt update && sudo apt upgrade -y
```

## Install docker
```console
sudo apt install docker.io
sudo apt install podman-docker
```

## Testing docker
```console
sudo docker run hello-world
```

## Creating a network
```console
docker network create lab_network --subnet 192.168.2.0/24
```

## Some config stuff to get the images we want
```console
echo 'unqualified-search-registries=["docker.io"]' | sudo tee -a /etc/containers/registries.conf
```

## Pulling docker images
```console
docker pull kalilinux/kali-rolling
docker pull tleemcjr/metasploitable2
```

## Create Kali (attack box)
```console
docker run -d -it --name kali --network lab_network --ip="192.168.2.2" --hostname kali kalilinux/kali-rolling
```

## Create target box
```console
docker run -d -it --name metasploitable --network lab_network --ip="192.168.2.3" --hostname metasploitable2 tleemcjr/metasploitable2
```

## Enter / Open Kali
```console
docker exec -it kali bash
```

## Installing the basics on Kali
```console
apt update && apt -y install kali-linux-headless
apt upgrade -y
printf '%s\n' "deb https://download.docker.com/linux/debian bullseye stable" | tee /etc/apt/sources.list.d/docker-ce.list
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-ce-archive-keyring.gpg
apt update && apt install -y docker-ce docker-ce-cli containerd.io
```

## Exiting Kali
```console
exit
```
