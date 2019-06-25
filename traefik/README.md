## traefik network with auto let's encrypt


### 1. Edit traefik.toml

1. add a default domain 
1. add a defailt email for let's encrypt

### create the config volume on the host
(or make sure they exist)

```
$ mkdir /srv/www/traefik
$ cp traefik.toml /srv/www/traefik/
```

### create the acme.json on the host and secure
(or make sure they exist and correct permisisons)

```
$ touch /srv/www/traefik/acme.json && chmod 600 /srv/www/traefik/acme.json
```

### create the network

```
$ doccker create network traefik-proxy
```

### deploy the containers

```
$ docker up -d
```
