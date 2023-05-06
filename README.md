# caddy-docker-proxy-swarm

This repo shows how you can use caddy-docker-proxy with docker in swarm mode, and be able to connect other services.

## How to

- Create a `caddy` network using the swarm scope: `docker network create -d overlay caddy`
- Launch caddy as a proxy with the docker stack command: `docker stack deploy -c caddy.yml caddy_stack`
- Launch whoami service with `docker stack deploy -c whoami.yml whoami_stack`

### Using a service with a custom build image phase

Docker swarm doesn't support the `build` keyword in `compose` file, that's why you need to create a local registry for storing image.

- First create a local registry service: `docker service create --name registry -p 5000:5000 registry:2`
- build your image as usual but respect this pattern: `docker build -t 127.0.0.1:5000/app:latest .`

The image need to respect the following pattern: `[REGISTRY_IP]:[REGISTRY_PORT]/[IMAGE_NAME]:[IMAGE_TAG]`

- Push your image to the local registry: `docker push 127.0.0.1:5000/app:latest`

- In your compose file, reference the image from the registry:

```
version: '3.9'
services:
  app:
    networks:
    - caddy
    image: 127.0.0.1:5000/app:latest
    deploy:
      replicas: 2
      labels: 
        caddy: your-domain-name.app
        caddy.reverse_proxy: "{{ upstreams 8043 }}"
networks:
  caddy:
    external: true
```

- Update your stack with the deploy command: `docker stack deploy -c docker-compose.yml app_stack`




# Resources

- [Compose file deploy reference](https://docs.docker.com/compose/compose-file/deploy/)
- [Example voting app](https://github.com/dockersamples/example-voting-app/blob/main/docker-stack.yml)
- [Le renouveau de docker compose](https://bearstech.com/societe/blog/le-renouveau-de-docker-compose/)
- [Docker : L'instruction Healthcheck](https://www.grottedubarbu.fr/docker-healthcheck/)
- [caddy docker proxy with swarm](https://github.com/lucaslorentz/caddy-docker-proxy/blob/master/examples/standalone.yaml)
- [Baeldung: Pushing a Docker Image to a Self-Hosted Registry](https://www.baeldung.com/ops/docker-push-image-self-hosted-registry)
