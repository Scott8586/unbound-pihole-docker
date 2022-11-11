# unbound-pihole-docker
A combination of Unbound and Pi-hole in a docker container for the raspberry pi

Basically, I've followed the steps from [Pi-hole Unbound Docker Setup on a Raspberry Pi](https://www.xfelix.com/2020/09/pihole-unbound-docker-setup-on-raspberry-pi/)

### Installation

1. Install docker and docker-compose - this is not necessarily as easy as it seems.
2. Clone this repository.
3. Create the sub-directories `pihole`, `dnsmasq.d`, and `unbound` if it doesn't already exist.
4. Adjust docker-compose.yml, and edit .env to suit your enviroment.
5. You can then stepwise start-up unbound and test it, then pihole and test it as in the url above - I often do `docker-compose up` without putting it into daemon mode so I can see the initial log output.
