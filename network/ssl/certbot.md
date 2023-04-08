# Certbot

## using dns
```shell
// create cert (dns method) add many sub domains as needed
sudo certbot certonly --manual --preferred-challenges dns

// regenerate cert
sudo certbot delete 
sudo certbot certonly --manual --preferred-challenges dns

// pem files can be ranamed to key and cer
mv fullchain.pem example.com.crt
mv privkey.pem example.com.key

```