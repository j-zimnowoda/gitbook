---
description: Working with YAML files
---

# yq

## Overview

The YAML format is becoming the standard for describing any kind of configuration parameters. A common uses case is to extract a given parameter or set of parameters from a YAML file In this article I am going to present some examples that I am often using at my work.

{% hint style="info" %}
The yq verion: 3.4.0
{% endhint %}

## Examples

### Get all keys under clouds attribute

**Input**:

```yaml
clouds:
    aws: {}
    amazon: {}
    digitalOcean: {}
    google: {}
    ovh: {}
```

**Command**:

```bash
yq r -j $filePath clouds | jq -r '.|keys[]'
```

**Output**:

```text
amazon
aws
digitalOcean
google
ovh
```

### Remove each filed called `required`

**Input**:

```yaml
definitions:
  address:
    type: object
    properties:
      street_address:
        type: string
      city:
        type: string
      state:
        type: string
    required:
    - street_address
    - city
    - state
type: object
properties:
  billing_address:
    "$ref": "#/definitions/address"
  shipping_address:
    allOf:
    - "$ref": "#/definitions/address"
    - properties:
        type:
          enum:
          - residential
          - business
      required:
      - type
```

**Command**:

```bash
yq d test.yaml '**.required.'
```

**Output:**

```yaml
definitions:
  address:
    type: object
    properties:
      street_address:
        type: string
      city:
        type: string
      state:
        type: string
type: object
properties:
  billing_address:
    "$ref": "#/definitions/address"
  shipping_address:
    allOf:
      - "$ref": "#/definitions/address"
      - properties:
          type:
            enum:
              - residential
              - business
```

## References:

* [https://mikefarah.gitbook.io/yq/](https://mikefarah.gitbook.io/yq/)







