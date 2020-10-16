# jq

## Examples

### Update package.json 

The package.json needs to be updated with `repository` entry to ensure that `npm publish` uploads artifacts to private github packages.

```text
REPO='myrepo'
VENDOR='myvendor'
TARGET_PACKAGE_JSON='package.json'

jq \
--arg type 'git' \
--arg url ${REPO} \
--arg directory ${VENDOR} \
'. + {"repository": {"type": $type, "url": $url, "directory": $directory}}' \
${TARGET_PACKAGE_JSON} \
| sponge ${TARGET_PACKAGE_JSON}
```

