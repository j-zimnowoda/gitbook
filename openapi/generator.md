# API client generation

## Generate typescript API client

Generate typescript API client based on spec OpenApi specification: `api.json`

```text
docker run --rm \
  -v ${PWD}:/local \
  openapitools/openapi-generator-cli generate \
  -i /local/api.json \
  -g typescript-node \
  -o /local/out/ \
  --additional-properties supportsES6=true,npmName=my-client
```

