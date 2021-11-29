# Traefik

Home reverse proxy for all intra service



## Configuration

Before start the docker compose, please change the configuration file with your data

| Parameter          | File                 | Description               |
| ------------------ | -------------------- | ------------------------- |
| DOMAIN             | `.env`               | Custom domain name        |
| CLOUDFLARE_EMAIL   | `docker-compose.yml` | Cloudflare email          |
| CLOUDFLARE_API_KEY | `docker-compose.yml` | Cloudflare api key        |
| email              | `config/traefik.yml` | Let's encrypt email certs |



## Bind9

Bind9 is a local dns server for linux server.

Install

```ba
apt install bind9 bind9utils dnsutils
```

Edit */etc/bind/named.conf.local* and add

```bash
zone "example.ltd" {
	type master;
	file "/etc/bind/example.ltd"
}
```

Create *example.ltd* from localhost template

```bash
cp /etc/bind/db.127 /etc/bind/example.ltd
```

and edit it

```bash
$TTL	604800
@	IN	SOA	example.ltd root.example.ltd (
			      1		; Serial
			 604800		; Refresh
			  86400		; Retry
			2419200		; Expire
			 604800 )	; Negative Cache TTL
;
@			IN	NS		example.ltd.
@			IN	A			192.168.1.40
@			IN	AAAA	::1
test	IN	A			192.168.1.40
db		IN 	A			192.168.1.40
*			IN	A			192.168.1.40
```

*Test* and *db* are example A record. You can remove if you not needed.



## Expose service

Use labels to expose docker service

```yaml
services:
	grafana:
		...
		labels:
      - "traefik.enable=true"
      - "traefik.http.routers.grafana.entrypoints=http"
      - "traefik.http.routers.grafana.rule=Host(`grafana.$DOMAIN`)"
      - "traefik.http.routers.grafana.tls.certresolver=cloudflare"
```
