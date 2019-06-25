## traefik network with auto let's encrypt


### 1. Edit traefik.toml

1. add a default domain 
1. add a default email for let's encrypt

### 2. Create the config volume on the host
(or make sure they exist)

```
$ mkdir /srv/www/traefik
$ cp traefik.toml /srv/www/traefik/
```

### 3. Create the acme.json on the host and secure
(or make sure they exist and correct permisisons)

```
$ touch /srv/www/traefik/acme.json && chmod 600 /srv/www/traefik/acme.json
```

### 4. Create the network

```
$ docker network create traefik-proxy
```

### 5. Deploy the containers

```
$ docker up -d
```
