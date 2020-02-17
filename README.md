# tls-traefik-selfsigned

Traefik 1.7 running in docker using self-signed certs.

Following this guide: https://jimfrenette.com/2018/03/ssl-certificate-authority-for-docker-and-traefik/

### Certs

```
openssl genrsa -des3 -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 3650 -out rootCA.cer

openssl req -new -sha256 -nodes -out server.csr -newkey rsa:2048 -keyout server.key -config <( cat server.csr.cnf )
openssl x509 -req -in server.csr -CA ../rootCA.cer -CAkey ../rootCA.key -CAcreateserial -out server.crt -days 730 -sha256 -extfile v3.ext
```

* `rootCA.key` passphrase is `test`

### Hostname

Add to `/etc/hosts`:

```
127.0.0.1 local.docker.whoami.com
```

### Docker

```
docker network create webgateway

cd ./traefik
docker-compose up -d

cd ./whoami
docker-compose up -d
```
