# Unbound and pi-hole in a docker-compose enviroment.
A combination of Unbound and Pi-hole in a pair of docker containers communicating over a private network - for the Raspberry Pi.  I run this on raspi 3B and 3B+ devices, though I imagine that it would work on a 4B as well.

Basically, I've followed the steps from [Pi-hole Unbound Docker Setup on a Raspberry Pi](https://www.xfelix.com/2020/09/pihole-unbound-docker-setup-on-raspberry-pi/)

### Installation

1. Install docker and docker-compose - this is not necessarily as easy as it seems.
2. Clone this repository.
3. Create the sub-directories `pihole`, `dnsmasq.d`, and `unbound` if it doesn't already exist.
4. Adjust docker-compose.yml, and edit .env to suit your enviroment.
5. You can then stepwise start-up unbound and test it, then pihole and test it as in the url above - I often do `docker-compose up` without putting it into daemon mode so I can see the initial log output.
