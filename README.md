# Unbound and pi-hole in a docker-compose enviroment.
A combination of Unbound and Pi-hole in a pair of docker containers communicating over a private network - for the Raspberry Pi.  I run this on raspi 3B and 3B+ devices, though I imagine that it would work on a 4B as well.

Basically, I've followed the steps from [Pi-hole Unbound Docker Setup on a Raspberry Pi](https://www.xfelix.com/2020/09/pihole-unbound-docker-setup-on-raspberry-pi/)

### Installation

1. Install docker and docker-compose - this is not necessarily as easy as it seems.
2. Clone this repository.
3. Create the sub-directories `pihole`, `dnsmasq.d`, and `unbound` if it doesn't already exist.
4. Adjust docker-compose.yml, and edit .env to suit your enviroment.
5. You can then stepwise start-up unbound and test it, then pihole and test it as in the url above - I often do `docker-compose up` without putting it into daemon mode so I can see the initial log output.

If you have not added yourself to the docker group with `sudo usermod -aG docker ${USER}`, you may need to add `sudo` to the docker command below.

### Testing Unbound

1. `docker-compose up -d unbound`
2. `dig www.google.com @172.20.0.7 -p 5053`
3. `dig www.google.com @127.0.0.1 -p 5053`

### Testing pihole

1. `docker-compose up -d pihole`
2. `dig www.google.com @172.20.0.6 -p 53`
3. `dig www.google.com @127.0.0.1 -p 53`
4. Also access http://<raspi_ipaddr>/admin/index.php to configure pihole blocklist, etc.

### Troubleshooting

`docker ps`

`docker inspect <container name, such as pihole or unbound>`

`docker logs pihole`

### Maintenance and Update

`docker pull pihole/pihole:latest`

`docker pull mvance/unbound-rpi:latest`

`docker stop pihole`

`docker stop unbound`

`docker rm pihole`

`docker rm unbound`

`docker-compose up -d unbound`

`docker-compose up -d pihole`

