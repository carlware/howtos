# Self Signed Certificate


## Method 1
1. Create the Server Private Key
```shell
openssl genrsa -out server.key 2048
```
2. Create Certificate Signing Request Configuration
```shell
cat > csr.conf <<EOF
[ req ]
default_bits = 2048
prompt = no
default_md = sha256
req_extensions = req_ext
distinguished_name = dn

[ dn ]
C = US
ST = California
L = San Fransisco
O = DataStores
OU = DataStores Dev
CN = development.com

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = *.development.com
DNS.2 = development.com
DNS.3 = localhost.development.com
DNS.4 = host.docker.internal
IP.1 = 127.0.0.1

EOF
```
3. Generate Certificate Signing Request (CSR) Using Server Private Key
```shell
openssl req -new -key server.key -out server.csr -config csr.conf
```
4. Create a external file
```shell
cat > cert.conf <<EOF

authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = development.com
DNS.2 = *.development.com
DNS.3 = localhost.development.com
DNS.4 = host.docker.internal

EOF
```
5. Generate SSL certificate With self signed CA
```shell
openssl x509 -req \
    -in server.csr \
    -CA rootCA.crt -CAkey rootCA.key \
    -CAcreateserial -out server.crt \
    -days 365 \
    -sha256 -extfile cert.conf
```

## Method 2

1. Generating a self-signed certificate config files
```shell
cat > cert.conf <<EOF

[req]
default_bits  = 2048
distinguished_name = req_distinguished_name
req_extensions = req_ext
x509_extensions = v3_req
prompt = no

[req_distinguished_name]
countryName = XX
stateOrProvinceName = N/A
localityName = N/A
organizationName = Self-signed certificate
commonName = 120.0.0.1: Self-signed certificate

[req_ext]
subjectAltName = @alt_names

[v3_req]
subjectAltName = @alt_names

[alt_names]
DNS.3 = example.com
DNS.4 = host.docker.internal
IP.1 = 127.0.0.1
EOF
```
2. Generating a self-signed certificate with OpenSSL
```shell
openssl req -x509 -nodes -days 730 -newkey rsa:2048 -keyout key.key -out cert.crt -config cert.cnf
```
3. Inspect certificate
```shell
openssl x509 -in cert.pem -text -noout
```

### References
1. https://devopscube.com/create-self-signed-certificates-openssl/
2. https://medium.com/@antelle/how-to-generate-a-self-signed-ssl-certificate-for-an-ip-address-f0dd8dddf754
3. https://superuser.com/questions/1073986/self-signed-certificate-with-openssl-for-server-at-home-and-no-domain-name
