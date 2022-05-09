# Open5Gs Docker
## Docker implementation of open5gs, with EPC, 5G SA and User Plane separated


Various directories for a docker implementation of Open5Gs modules separated according to the scenarios for my thesis "Agile 5G OnBoarding".

## Docker compose

- For EPC deployment
- For 5G SA deployment
- Isolated 4G User Plane Server
- Isolated 5G User Plane Server
- Complete deployment with UERANSIM, IMS and srsLTE (credits to @herlesupreeth)


## Installation

Requires [docker-ce](https://docs.docker.com/install/linux/docker-ce/ubuntu) and [docker-compose](https://docs.docker.com/compose) .

After cloning the repository choose which of the deployments you wish to do and you may delete the unwanted folders: 

For EPC:
```sh
cd open5gs_docker/epc
docker build --no-cache --force-rm -t docker_open5gs .
```

For 5G SA:
```sh
cd open5gs_docker/5G_sa
docker build --no-cache --force-rm -t docker_open5gs .
```

For 4G/5G NSA User Plane:
```sh
cd open5gs_docker/up_4g
docker build --no-cache --force-rm -t docker_open5gs .
```

For 5G User Plane:
```sh
cd open5gs_docker/up_5g
docker build --no-cache --force-rm -t docker_open5gs .
```

Now you may build and run using docker compose 

```sh
cd ..
set -a
source .env
docker-compose build --no-cache
docker-compose up
```

## Configuration

This deployment comes with some configurations already set, such as the MCC/MNC, Test_Network and advertised ports in the docker compose. But you will still need to change the following:

| Variable | Explanation |
| ------ | ------ |
| DOCKER_HOST_IP | This is the IP address of the host running your docker setup |
| SGWU_ADVERTISE_IP | Change this to value of DOCKER_HOST_IP set above, this will need to be done in EPC and 4G User Plane |
| UPF_ADVERTISE_IP | Change this to value of DOCKER_HOST_IP set above, this will need to be done in 5G SA and 5G User Plane |


## UE subnets

To define the UE subnet please change the `${subnet}` in the following line of the `upf/upf_init.sh`:

```sh
python3 /mnt/upf/tun_if.py --tun_ifname ogstun --ipv4_range ${subnet} --ipv6_range 2001:230:cafe::/48
```

> Subnet example: 10.47.0.0/16



#### Credits

Credits to the developers of [Open5Gs](https://github.com/open5gs) and to @herlesupreeth for the amazing work on the [docker deployment](https://github.com/herlesupreeth/docker_open5gs).
