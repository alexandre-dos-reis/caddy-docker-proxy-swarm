# caddy-docker-proxy-swarm

This repo shows how you can use caddy-docker-proxy with docker in swarm mode, and be able to connect other services.

## How to

- Create a network using the swarm scope: `docker network create -d overlay caddy`
- Launch caddy as a proxy with the docker stack command: `docker stack deploy -c caddy.yml caddy_stack`
- Launch the whoami service with `docker stack deploy -c whoami.yml whoami_stack`

# Resources

- [Compose file deploy reference](https://docs.docker.com/compose/compose-file/deploy/)
- [Example voting app](https://github.com/dockersamples/example-voting-app/blob/main/docker-stack.yml)
- [Le renouveau de docker compose](https://bearstech.com/societe/blog/le-renouveau-de-docker-compose/)
- [Docker : L'instruction Healthcheck](https://www.grottedubarbu.fr/docker-healthcheck/)
- [caddy docker proxy with swarm](https://github.com/lucaslorentz/caddy-docker-proxy/blob/master/examples/standalone.yaml)  
