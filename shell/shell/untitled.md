---
description: All best practices I know about bash
---

# bash

## Fail on error

In most cases I wanted my script to exit on error. Sounds trivial although it's not.

```bash
set -euo pipefail
```

The `set -eo pipefail` ensures to exit immediately if with first command that fails, including those in pipeline.

{% hint style="info" %}
A trap on `ERR`, if set, is executed before the shell exits. 
{% endhint %}

{% hint style="info" %}
 The shell does not exit if the command that fails is part of the test in an`if` statement, part of any command executed in a `&&` or `||` list except the command following the final `&&` or `||`

```text
set -euo pipefail
[[ 1 > 2 ]] && echo "impossible" || echo "shell exits with 0"
```
{% endhint %}

The `set -u`treats unset variables and parameters as an error when performing parameter expansion

{% hint style="warning" %}
Not applicable to special parameters ‘@’ or ‘\*’
{% endhint %}

{% hint style="info" %}
It can happen that there are some variable that may not be set and you are testing against variable existence:

```text
[[ -z "${UNSET_VAR}" ]] 
```

The above code would cause shell to exit if`set -u`is used. Instead acess variable with default value:

```text
[[ -z "${UNSET_VAR}" ]]
```
{% endhint %}

## Cleanup

{% hint style="info" %}
```text
function cleanup {
  local exitcode=$?
  echo "Cleanup"
  return $exitcode;
}

trap cleanup ERR EXIT
```
{% endhint %}



## References

* [Set builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html#The-Set-Builtin)
* [Parameter substitution](https://tldp.org/LDP/abs/html/parameter-substitution.html)



