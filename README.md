# tls-traefik-selfsigned

Traefik 1.7 running in docker using self-signed certs.

Following this guide: https://jimfrenette.com/2018/03/ssl-certificate-authority-for-docker-and-traefik/

### Certs

```sh
mkdir certs
cd certs/

# Generate root cert
openssl genrsa -des3 -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 3650 -out rootCA.cer

mkdir local.docker.whoami.com
cd local.docker.whoami.com/

# Generate self signed server cert
openssl req -new -sha256 -nodes -out server.csr -newkey rsa:2048 -keyout server.key -config <( cat server.csr.cnf )
openssl x509 -req -in server.csr -CA ../rootCA.cer -CAkey ../rootCA.key -CAcreateserial -out server.crt -days 730 -sha256 -extfile v3.ext
```

Note: `rootCA.key` passphrase is `test`

### Hostname

Add to `/etc/hosts`:

```sh
127.0.0.1 local.docker.whoami.com
```

### Docker

```sh
docker-compose up -d
```

or

```sh
docker stack deploy -c docker-compose.yml tls-traefik-selfsigned
```

Visit:
* http://local.docker.whoami.com/
* https://local.docker.whoami.com/
