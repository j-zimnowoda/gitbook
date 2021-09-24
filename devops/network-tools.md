# Network tools

Httperf

Generate https traffic on port 433 with SSL handshake.

```text
url=mydomian.local
httperf --num-conns 10000 --rate 10 --timeout 3 --port 443 --ssl --server $url
```

