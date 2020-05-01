# setup drone ci

## pitfalls
- drone needs a public ip or domain name so the git repository webhooks are working
- DRONE_SERVER_PROTO and DRONE_SERVER_HOST are used for the creation of the webhook so the external protocol and public ip/ domain name are needed
- you may want to set DRONE_LOGS_DEBUG=true and DRONE_LOGS_PRETTY=true to true until your server is completly running.
  
## troubleshooting
- build does not start [troubleshooting](https://discourse.drone.io/t/nothing-happens-when-i-push-code-no-builds-or-builds-stuck-in-pending/3424) 
- build stuck in pending [troubleshooting](https://discourse.drone.io/t/builds-are-stuck-in-pending-status/4437)
- other stuff [discourse forum](https://discourse.drone.io/).

### install
Follow the [official install guide](https://docs.drone.io/server/overview/). The following code snippets might be helpful for copypasta, but you might want to read the offical install guide before. If the server is running but the builds do not get triggered check this [trouble shooting guide](https://discourse.drone.io/t/nothing-happens-when-i-push-code-no-builds-or-builds-stuck-in-pending/3424). For asking questions or searching for one use the [discourse forum](https://discourse.drone.io/).

```shell
# 1. generate DRONE_RPC_SECRET with: 
openssl rand -hex 16

# 2. create a bridge network so the contains can address each other with their name
docker network create drone

# 3. start drone master

docker run \
  --volume=/var/lib/drone:/data \
  --env=DRONE_GITHUB_CLIENT_ID=<FROM_GITHUB> \
  --env=DRONE_GITHUB_CLIENT_SECRET=<FROM_GITHUB_SECRET> \
  --env=DRONE_RPC_SECRET=<GENERATED_SECRET_FROM_STEP_1> \
  --env=DRONE_SERVER_HOST=ci.weyrich.dev \
  --env=DRONE_SERVER_PROTO=https \
  --env=DRONE_USER_FILTER=crowdsalat \
  --env=DRONE_USER_CREATE=username:crowdsalat,admin:true \
  --env=DRONE_LOGS_PRETTY=true \
  --env=DRONE_LOGS_COLOR=true \
  --env=DRONE_LOGS_DEBUG=true \
  --publish=180:80 \
  --restart=always \
  --detach=true \
  --name=drone \
  --network drone \
  drone/drone:1

# 4. start drone docker runner:

docker run -d \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e DRONE_RPC_PROTO=HTTP \
  -e DRONE_RPC_HOST=drone:80 \
  -e DRONE_RPC_SECRET=<GENERATED_SECRET_FROM_STEP_1> \
  -e DRONE_RUNNER_CAPACITY=2 \
  -e DRONE_RUNNER_NAME=drone_docker_runner \
  -e DRONE_LOGS_DEBUG=true \
  -p 3000:3000 \
  --restart always \
  --name drone_runner \
  --network drone \
  drone/drone-runner-docker:1
  ```