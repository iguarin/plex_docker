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

1. "-d" specifies the driver
2. Gateway in this case is my router's IP.
3. Specifying the IP range is imporant as you don't want the IP conflict. In your router, you can specify a range of IPs not to be included in the DHCP range. In this case, I chose IPs 192.168.0.240-254. 
4. "eth0" is the interface connected to the internet.
5. "pub_net" is the name of the network.

## Creating the Plex Container ##

Before we create the container, we have to create the folders that are required for plex: *config, media and transcode". (transcode is optional).

Go to your home and run the following commands:
```mkdir plex``` \n
```cd plex```
```mkdir config```
```mkdir media```
*Note that if you are hosting your media like me on a removable drive, you can skip this command*
```mkdir transcode```



```docker run -d --restart=always --name plex --network=pub_net --ip=192.168.0.12 -e TZ="America/Chicago" -e VERSION=latest -e PUID=1000 -e PGID=1000 -h plex -v ~/plex-config:/config -v /mnt/torrents/completed:/media -v ~/plex-transcode:/transcode plexinc/pms-docker```

1. "-restary=always" allows the container to start after a reboot.
2. "e VERSION=latest" gets the latest version. "-e PUID=1000 -e PGID=1000" are the IDs of the user you want to run the Plex container.
3. 




