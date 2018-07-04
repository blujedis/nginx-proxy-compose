# Nginx Proxy Compose

Docker compose files for setting up automated Nginx Proxy and SSL for docker containers.

## Install

Either pull down this repository using Git:

```sh
$ git pull https://github.com/blujedis/nginx-proxy-compose.git
```

You can also install into your node modules using NPM if you wish. Note it is not published to NPM.

```sh
npm install blujedis/compose-nginx-proxy
```

## Usage

Ensure you have a network available named "nginx-proxy" see [below](#Network) for instructions. Then run:

```sh
$ docker-compose up
```

OR

```sh
$ docker-compose up -d
```

**NOTE** If you've installed using NPM you'll need to cd to node_modules/compose-alpine-nginx-proxy before running docker-compose or use the <code>-f ./node_modules/nginx-proxy-compose/docker-compose.yml</code> to specify the docker-compose file path.

## Ports

The proxy ports default alternate ports to avoid conflicts. Use additional yaml files to override. See Compose Override below.

- 88:80
- 8443:443

## Compose Override

You can also specify both your compose file along with production overrides depending on your environment. By specifying the compose files to run the latter file will override any keys in the main docker-compose.yml file. This is useful for dev/prod envirments.

```sh
$ docker-compose -f docker-compose.yml -f docker-compose-production.yml
```

## Docker Images

The following images are included in this compose file.

- jwilder/nginx-proxy:alpine (automated Nginx proxy)
- jrcs/letsencrypt-nginx-proxy-companion (automated Letsencrypt SSL)

## Volumes

The following volumes are exposed for use with the proxy and letsencrypt for SSL.

Note "apps" volume would be where you store applications. In a Dockerfile you might
map your container to /apps/myapp for example.

- conf:/etc/nginx/conf.d
- vhost:/etc/nginx/vhost.d
- certs:/etc/nginx/certs:ro
- html:/usr/share/nginx/html

## Network

Requires the network "nginx-proxy" to be available. You can create this network by running:

```sh
$ docker network create --opt com.docker.network.bridge.name=nginx-proxy nginx-proxy
```

