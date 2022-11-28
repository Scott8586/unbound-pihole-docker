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

### Alpine 3.13, Raspberry Pi 3/4, and libseccomp2

If you run into the following error:

```
pihole     | s6-svscan: warning: unable to iopause: Operation not permitted
pihole     | s6-svscan: warning: executing into .s6-svscan/crash
pihole     | s6-svscan crashed. Killing everything and exiting.
pihole     | s6-supervise s6-linux-init-shutdownd: fatal: unable to iopause: Operation not permitted
pihole     | s6-linux-init-hpr: fatal: unable to reboot(): Operation not permitted
```

You need to add the backported version of libseccomp2 in order for docker to start the pihole container, see [Fix/Workaround - libseccomp2 and Alpine 3.13 - Installing Raspbian Docker 19.04+ on Raspberry Pi 4 Buster](https://blog.samcater.com/fix-workaround-rpi4-docker-libseccomp2-docker-20/)

In short the commands are:

```
# Get signing keys to verify the new packages, otherwise they will not install
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 04EE7237B7D453EC 648ACFD622F3D138

# Add the Buster backport repository to apt sources.list
echo 'deb http://httpredir.debian.org/debian buster-backports main contrib non-free' | sudo tee -a /etc/apt/sources.list.d/debian-backports.list

sudo apt update
sudo apt install libseccomp2 -t buster-backports
```

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

### Adding to telegraf

You can still pull stats data into telegraf using the inputs.exec tool.  THe format looks like this:

```
[[inputs.exec]]
  commands = ["/bin/sh -c \"docker exec unbound /opt/unbound/sbin/unbound-control stats | grep -v thread0 | sed 's/total\\.//'\""]
  name_override = "unbound"
  data_format = "logfmt"
  data_type = "float"
  interval = "10m"
```

You must also add telegraf to the docker group, like so `sudo usermod -aG docker telegraf`


