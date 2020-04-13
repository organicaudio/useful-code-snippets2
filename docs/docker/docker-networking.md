
## network types

Copied from [network driver summary](https://docs.docker.com/network/):

- User-defined bridge networks are best when you need multiple containers to communicate on the same Docker host.
- Host networks are best when the network stack should not be isolated from the Docker host, but you want other aspects of the container to be isolated.
- Overlay networks are best when you need containers running on different Docker hosts to communicate, or when multiple applications work together using swarm services.
- Macvlan networks are best when you are migrating from a VM setup or need your containers to look like physical hosts on your network, each with a unique MAC address.
- Third-party network plugins allow you to integrate Docker with specialized network stacks.

## user-defined bridge

[official doc](https://docs.docker.com/network/bridge/)

A user-defined bridge allow that two containers in this network can access each other via their container name (dns resolution). Container without a explicit network are in the defualt network bridge. The default bridge have no dns server so the containers need to address each other via their ip address. You can get the ip address of a container via `docker instpect <containerName>`.

```shell
# create a new bridge network
docker network create --driver bridge <bridgeName>

# list networks
docker network ls

# start container a contgainer in the network 
docker run --network=<bridgeName> <imageName>

# add a running container to a network
docker network connect <bridgeName> <runningContainerName>
```