# openssl

Decode certificate in PEM format

```
openssl x509 -in certificate.crt -text -noout
```

Download certificate chain from a given host

```
openssl s_client -host example.com -port 443 -prexit -showcerts
```

Download root CA

```
true | openssl s_client -connect example.com:443 2>/dev/null | openssl x509
```
