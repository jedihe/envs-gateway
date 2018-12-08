## Local Envs Web Gateway

Very small container with traefik set up to serve multiple local envs.

Usage:

Start the web gateway (clone into 'traefik' dir):

```
git clone git@github.com:jedihe/envs-web-gateway.git traefik
cd traefik
docker-compose up -d
```

Notice that the previous step also created the 'traefik_webgateway' network
(bridge mode). The traefik container is set to be running always (unless it's
explicitly stopped).

Next, update every env docker-compose.yml file to connect the web/app service
to the traefik_webgateway network, but make sure they still can communicate
with any back-end service (e.g. db):


```
...
services:
  web:
    image: php:5.6-apache-jessie
    # Some config...
    networks:
      - web
      - backend

  db:
    image: mysql:5.5
    networks:
      - backend

 # Some more config, services, etc...


# custom networks section
networks:
  web:
    external:
      name: traefik_webgateway
  backend:
```
