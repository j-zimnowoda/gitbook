---
description: All best practices I know about bash
---

# bash

## Fail on error

Usually, I wanted my script to exit on error. Sounds trivial although it's not. There are number of case that someting can go wrong. The most common are:

* variable is not set
* a given command fails during execution

By default bash does not fail on the first encountered error. Instead it impassively continous exution till the end of the script. A simple yet powerful statement below can save you from many unwanted side effects:

```bash
set -euo pipefail
```

The `set -e` exits on failure of simple command. The `set -o pipefail` exits on first failed command in pipeline Finally  

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

**Finaly** `set -u`treats unset variables and parameters as an error when performing parameter expansion

{% hint style="warning" %}
Not applicable to special parameters ‘@’ or ‘\*’
{% endhint %}

{% hint style="info" %}
It is possible to use `set -u` and to test variable existence with `-z.`Access variable with default. empty value.

```text
[[ -z "${UNSET_VAR-}" ]] 
```
{% endhint %}



## Cleanup

Sometimes my script creates temporary files that should be removed regardless script exit code. The following code snippet is a good boilerplate:

```text
function cleanup {
  echo "Remove temporary files"
}

trap cleanup EXIT
```

### Pitfalls

Try using trap with the below signals What behavior would you expect?

```text
trap cleanup EXIT ERR
```

## Q&A

Can trap function perfrom endless loop if it fails and `set -e ?`

```text
set -e 
function cleanup {
  local exitcode=$?
  ls nonexisting_file
  return $exitcode;
}

trap cleanup EXIT ERR
```

No, it will be performed only once.



## References

* [Set builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html#The-Set-Builtin)
* [Parameter substitution](https://tldp.org/LDP/abs/html/parameter-substitution.html)



