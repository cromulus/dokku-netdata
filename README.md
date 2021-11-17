# Deploy Netdata on your Dokku instance

## Create the application

```sh
dokku apps:create netdata
dokku config:set netdata DO_NOT_TRACK=1 PGID=999

dokku docker-options:add netdata deploy,run "--cap-add=SYS_PTRACE"
dokku docker-options:add netdata deploy,run "--security-opt apparmor=unconfined"

dokku storage:mount netdata /etc/group:/host/etc/group:ro
dokku storage:mount netdata /etc/os-release:/host/etc/os-release:ro
dokku storage:mount netdata /etc/passwd:/host/etc/passwd:ro
dokku storage:mount netdata /proc:/host/proc:ro
dokku storage:mount netdata /sys:/host/sys:ro
dokku storage:mount netdata /var/lib/dokku/data/storage/netdata:/etc/netdata
dokku storage:mount netdata /var/run/docker.sock:/var/run/docker.sock:ro
```

## Deploy

1. Clone this repo.

2. Set up your Dokku server as a remote.

```bash
git remote add dokku dokku@example.com:netdata
```

3. Push it to Dokku

```bash
git push dokku main
```

## Set proxy, HTTPS and Auth

Proxy:

```sh
dokku proxy:ports-set netdata http:80:19999
```

SSL:

```sh
# Install letsencrypt plugin
dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git

# Set certificate contact email
dokku config:set --no-restart netdata DOKKU_LETSENCRYPT_EMAIL=you@example.com

# Generate certificate
dokku letsencrypt:enable netdata
```

Auth:

```sh
dokku http-auth:on netdata <username> <password>
```
