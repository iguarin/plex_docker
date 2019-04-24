# plex_docker
Do you want to remotely access your Plex Server but since you are running both a VPN/ Torrent client and a Plex server on the same box it's kind of difficult to do? This tutorial will demonstrate how to overcome the said issue with Docker's Macvlan. 


# Problems with Running Plex and Torrent Server (with VPN) together on the same box #: Plex default to the interface it finds first. If you are connected to a VPN server, depending on the VPN provider (some does not support port forwarding), you may not be able to route the traffic to or from your Plex Server. 
