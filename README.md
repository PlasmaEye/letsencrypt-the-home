# letsencrypt-the-home
Simple setup of LetsEncrypt certificates for any services running out of your home behind an Nginx proxy. Based off the excellent [Acme.sh script](https://github.com/Neilpang/acme.sh).

## Assumptions
1. This is being run on a Raspberry Pi or a Unix OS with a username `pi`
1. [DuckDNS](https://www.duckdns.org) is being used as a Dynamic DNS provider for your home network

## The process

```bash
sudo apt update && sudo apt upgrade -y && sudo apt install nginx -y
curl https://get.acme.sh | sh
export DuckDNS_Token="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
acme.sh --issue --dns dns_duckdns -d '*.[YOUR DOMAIN].duckdns.org' 
mkdir nginx
touch nginx/key.pem
touch nginx/cert.pem
acme.sh --install-cert -d *.plasmaeye.duckdns.org --key-file /home/pi/nginx/key.pem --fullchain-file /home/pi/nginx/cert.pem --reloadcmd "service nginx reload"
```
- add `ssl_certificate /home/pi/nginx/cert.pem` to nginx conf
- add `ssl_certificate_key /home/pi/nginx/key.pem` to nginx conf
