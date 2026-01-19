# HomeLab

A public overview of my HomeLab and how I built some of the elements.  I have a full near-enterprise network stack at home and over Christmas had to debug a few things.  On my list was to build out a monitoring system, a config backup system like RANCID and an IPAM host.  And thus began my rabbit hole into docker containers.  These appealed because I wanted something 'quick to deploy and configure', and there are a lot of great resources out there now.

I originally started out with just a docker setup on a fresh Ubuntu install when I discovered Portainer - since then I've discovered all kinds of Docker managers but I liked Portainer and stuck with it (the scope was already growing and was out of crontrol.

My existing enviroment:

Portainer host http://Docker01.local:9000/ (9999:9999 and 9000:9000) - 
dropit 3333:3333 - dropit is... - https://github.com/rmarles/HomeLab/tree/main/DropIt
homebox 3100:7745 - homebox is... - https://github.com/rmarles/HomeLab/tree/main/HomeBox
netbox 8100:8000 - netbox is an IPAM solution - https://github.com/rmarles/HomeLab/tree/main/NetBox
picard 5900:5900 and 5800:5800 - picard is .. - https://github.com/rmarles/HomeLab/tree/main/Picard
librenms+oxidized 3300:8000 - librenms is a monitoring system, oxidized pulls config logs similar to rancid, and flows them to librenms + pushes them to github. - https://github.com/rmarles/HomeLab/tree/main/Oxidized-LibreNMS
tandoor 8090:8080 - still working on this. https://github.com/rmarles/HomeLab/tree/main/tandoor
nginx-proxy - reverse proxy to map hostname:ports to friendly names - https://github.com/rmarles/HomeLab/tree/main/nginx-proxy

Upcoming \ Plans
* Implement SSL Certs
* SAML SSO using Authentik or Entra ID - not sure yet.
* Fix Tandoor
* Fix DropIt
* Shlink
* Linkwarden
* Proxmox Lab on Docker03.local
* Control Pane - managing Docker01, 02 and 03 some the same instance

I am considering making a public facing instance for publishing workloads to the internet.  I may move stuff like Tandoor to the edge once I get it working.

Also considering a public facing listserv for my car club.

... That and creating a HamLab on Docker02.local repo and putting the ham related items there

