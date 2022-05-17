# Raspberry-pi setup

This is a documentation on how i setup my raspberry-pi (mentioned RPi hereafter)

## 1 - Install Raspberry Pi OS Lite (64 bit)

Download and install [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to your laptop/pc

open Raspberry Pi Imager and select Raspberry Pi OS Lite (64 bit)
select your drive (i.e SD card) 
select the settings icon and enable SSH 
set user name and password

click WRITE button

once the process is completed, insert the SD card to your RPi and power it in 

> to get the IP of the RPi from your router

## 2 - set a static IP
login to the RPi

```
sudo nano /etc/dhcpcd.conf
```

at the end of the file add the following 


```
interface eth0
        static ip_address=192.168.0.5/24 # < change the static IP as per your desire
        static routers=192.168.0.1 # this is router IP
        static domain_name_servers=1.1.1.1 1.0.0.1
```

save and reboot

## 3 - Install docker

```
#!/bin/bash

function error {
  echo -e "\\e[91m$1\\e[39m"
  exit 1
}

function check_internet() {
  printf "Checking if you are online..."
  wget -q --spider http://github.com
  if [ $? -eq 0 ]; then
    echo "Online. Continuing."
  else
    error "Offline. Go connect to the internet then run the script again."
  fi
}

check_internet

curl -sSL https://get.docker.com | sh || error "Failed to install Docker."
sudo usermod -aG docker pi
sudo usermod -aG docker $USER || error "Failed to add user to the Docker usergroup."
echo "Remember to logoff/reboot for the changes to take effect."
```

## 4 - Install Docker-Compose

Installing python3 and pip3 to run the installer scripts

```
sudo apt-get install libffi-dev libssl-dev
sudo apt install python3-dev
sudo apt-get install -y python3 python3-pip
```

Install docker-compose

```
sudo pip3 install docker-compose
```
### Enable Docker to start your containers on boot

```
sudo systemctl enable docker
```

## 5 - Install portainer

```
sudo docker pull portainer/portainer-ce:latest
```

```
sudo docker run -d -p 9000:9000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

## 6 - Install git

```
sudo apt install git -y
```

Clone repo 

