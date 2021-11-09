# openssl

Decode certificate in PEM format

```
openssl x509 -in certificate.crt -text -noout
```

Download certificate chain from a given host

```
openssl s_client -host example.com -port 443 -prexit -showcerts
```
