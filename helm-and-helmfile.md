---
description: Keep it DRY
---

# Helm and Helmfile

## Useful Helm commands

Add chart repo

```text
helm repo add harbor https://helm.goharbor.io
```

Pull chart with a specific version and unpack its content

```text
helm pull harbor/harbor --version "1.5.0"| tar -vxzf harbor-1.5.0.tgz
```

