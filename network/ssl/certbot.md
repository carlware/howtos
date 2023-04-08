# Certbot

## using dns
```shell
// create cert (dns method) add many sub domains as needed
sudo certbot certonly --manual --preferred-challenges dns

// regenerate cert
sudo certbot delete 
sudo certbot certonly --manual --preferred-challenges dns

```