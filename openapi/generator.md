# generator

## Generate typescript API client

```text
docker run --rm \
  -v ${PWD}:/local \
  openapitools/openapi-generator-cli generate \
  -i /local/swagger.json \
  -g typescript-node \
  -o /local/out/ \
  --additional-properties supportsES6=true,npmName=my-client
```

