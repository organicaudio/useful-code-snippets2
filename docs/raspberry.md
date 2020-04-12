
# raspberry pi

## installation

- use [Etcher](https://www.balena.io/etcher/) to create the image
- add a ssh file to the root of the boot partition to activate ssh like explained [here](https://hackernoon.com/raspberry-pi-headless-install-462ccabd75d0)
- start raspberry pi (and connect to lan wire so you can start right away from the remote computer)
- use ssh to connect to rp
- use `sudo raspi-config` to configure the rp

## power off and restart

```
# shutdown alias
sudo poweroff

# restart alias
sudo reboot

# shutdown
sudo shutdown -h now

# restart
sudo shutdown -r now
```

## install docker on rp

```shell
# install
curl -sSL https://get.docker.com | sh


# add use pi to docker group
sudo usermod -aG docker pi

# reboot
sudo reboot

# test setup
docker run hello-world

# might not  work than use (https://stackoverflow.com/questions/52233182/docker-run-does-not-display-any-output)
docker run hypriot/armhf-hello-world
```

## docker images for rp

In order to run on a rp a docker image need to be compiled for arm processor architecture. You can find a list of supported cpu architectures for docker [here](https://github.com/docker-library/official-images#architectures-other-than-amd64). To find out which architecture your rp has [see](https://de.wikipedia.org/wiki/Raspberry_Pi#Hardware). E.g. a rp 2 mod B uses a ARMv7 architecture.

- [open-jdk](https://hub.docker.com/r/balenalib/raspberry-pi-openjdk)