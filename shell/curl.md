# curl





Print request and response body

```bash
curl --trace-ascii - --request POST \
  --url http://localhost:5555/rbac/ \
  --header 'Content-Type: application/json' \
  --data "$DATA"
```
