# plex_docker #
Do you want to remotely access your Plex Server but since you are running both a VPN/ Torrent client and a Plex server on the same box it's kind of difficult to do? This tutorial will demonstrate how to overcome the said issue with Docker's Macvlan. 


**Problems with Running Plex and Torrent Server (with VPN) together on the same box** : Plex default to the interface it finds first. If you are connected to a VPN server, depending on the VPN provider (some does not support port forwarding), you may not be able to route the traffic to or from your Plex Server. 

Without any further interruption, let us start.

## Installing Docker:

This command will fetch the docker installation script and execute it.


```curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh```


For security purposes, if you want to have a dedicated user for the Plex container, we can run some commands so the user can run docker without sudo privileges. 


```sudo groupadd docker```
```sudo gpasswd -a $USER docker```

To verify that the installation was successful:

```docker run hello-world```

Source: https://medium.freecodecamp.org/the-easy-way-to-set-up-docker-on-a-raspberry-pi-7d24ced073ef

## Creating the Macvlan Network ##

Simply put, a container connected to a Macvlan Network has its own MAC address, making it appear like it is a physical device directly connected to the network. It is like an equivalent of the bridge-networking in virtual machine applications.


```docker network create -d macvlan --subnet=192.168.0.0/24  --gateway=192.168.0.1 --ip-range 192.168.0.240/28 -o parent=eth0 pub_net```

1. 
2. 


