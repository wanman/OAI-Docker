# OAI-SPGW-Docker
Open Air Interface EPC's HSS MME

OpenAirInterface is an implementation of the 3GPP specifications concerning the Evolved Packet Core Networks, that means it contains the implementation of the following network elements: MME, HSS, S-GW, P-GW. 
[More information about Open Air Interface](https://gitlab.eurecom.fr/oai/openair-cn)

This project is aimed to run OpenAirInterface  in Docker Containers

## Minimum Requirements

- Running on Xenial (Cores=2 Mem=8G Root-Disk=30G)
- Ubuntu Xenial(16.04) amd64 | Cores=2 Mem=8G Root-disk=30G
- Linux 4.7.2 low latency Kernel (4.7.1 is also supported)
   Please note that the low latency is a must to initialize GTP Stack

## Install Requirements

The enviroment installation:

- Install Xenial(16.04) 
- Install Linux 4.7.2 low latency Kernel (4.7.1 is also supported)
- Install the latest "docker-ce" release 

Please note that for integrating other EPC Roles, IP end points must be pre-configured
In order to automate this process a Docker bridge is created:
(You can check the bridge has been pre-created with `docker network ls`)

`docker network create --driver=bridge --subnet=172.19.0.0/24 --gateway=172.19.0.1 oainet`

and the following IPs proposed for running the container

- 172.19.0.10 hss.openair4G.eur hss
- 172.19.0.20 epc.openair4G.eur epc
- 172.19.0.30 spgw.openair4G.eur spgw

Default SPGW configuration artifacts are provided in the repo

## Basic Execution

Instructions:
1) Pull the latest image
`docker pull moffzilla/oai-spgw:v01`

2) Execute as follows:
`docker run --ip=172.19.0.30 --net=oainet --expose=1-9000 -p 2152:2152 -p 2123:2123 -t -d --add-host "hss.openair4G.eur hss":172.19.0.10 --add-host "epc.openair4G.eur epc":172.19.0.20 --add-host "spgw.openair4G.eur spgw":172.19.0.30  -h=spgw --privileged=true --name="oai_spgw" --cap-add=ALL -v /dev:/dev -v /lib/modules:/lib/modules moffzilla/oai-spgw:v01`

3) Attach to the running container
`docker exec -it oai_spgw /bin/bash`

4) Verify the S11/S1U|S12|S4 Interfaces 

`netstat -na | grep 2123`
`netstat -na | grep 2152`

and the GTP interfaces are set and enable


root@epc:/openair-cn/SCRIPTS# netstat -na | grep 3868

tcp        0      0 172.19.0.20:60590       172.19.0.10:3868        ESTABLISHED

root@epc:/openair-cn/SCRIPTS# 




