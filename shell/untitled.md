---
description: All best practices I know about bash
---

# bash

## Overview

{% hint style="info" %}
The bash version: 3.2.57
{% endhint %}

## Booleans

There are no booleans in Bash.

There is a very interesitng discussion about implementing booleans in bash: [https://stackoverflow.com/questions/2953646/how-can-i-declare-and-use-boolean-variables-in-a-shell-script](https://stackoverflow.com/questions/2953646/how-can-i-declare-and-use-boolean-variables-in-a-shell-script)

In the end, I decided to use `true` and `false` strings for "boolean" flags, as they are the most versatile and secure way of implementing booleans:&#x20;

```
[[ "$var" == "false" ]]
[[ "$var" == "true" ]]
```

## Fail on error

Usually I want my script to exit on error. Sounds trivial although it's not. There are number of case that someting can go wrong. The most common are:

* variable is not set
* a given command fails during execution

By default bash does not fail on the first encountered error. Instead it impassively continous exution till the end of the script. A simple yet powerful statement below can save you from many unwanted side effects:

```bash
set -euo pipefail
```

The `set -e `exits on failure of simple command. The` set -o pipefail` exits on first failed command in pipeline Finally &#x20;

{% hint style="info" %}
A trap on `ERR`, if set, is executed before the shell exits.&#x20;
{% endhint %}

{% hint style="info" %}
&#x20;The shell does not exit if the command that fails is part of the test in an`if` statement, part of any command executed in a `&&` or `||` list except the command following the final `&&` or `||`

```bash
set -eo pipefail
[[ 1 > 2 ]] && echo "impossible" || echo "shell exits with 0"
```
{% endhint %}

**Finaly **`set -u`treats unset variables and parameters as an error when performing parameter expansion

{% hint style="warning" %}
Not applicable to special parameters ‘@’ or ‘\*’
{% endhint %}

{% hint style="info" %}
It is possible to use` set -u` and to test variable existence with `-z.`Access variable with default empty value.

```bash
[[ -z "${UNSET_VAR-}" ]] 
```
{% endhint %}



## Cleanup

Sometimes my script creates temporary files that should be removed regardless script exit code. The following code snippet is a good boilerplate:

```bash
function cleanup {
  echo "Remove temporary files"
}

trap cleanup EXIT
```

**Pitfalls**

Try using trap with the below signals.&#x20;

```bash
trap cleanup EXIT ERR
```

What behavior would you expect if some command fails?

## Arrays

Create array that contains a list of files and access its elements

```
touch f1 f2 f3
files=($(ls))
echo $files      # f1
echo ${files[0]} # f1
echo ${files[1]} # f2
echo ${files[@]} # f1 f2 f3
```

## Improve your skills

Use shell linter against your shell scripts and investigate the output.&#x20;

```
brew install shellcheck
```

{% hint style="info" %}
Add[ shellcheck plugin](https://marketplace.visualstudio.com/items?itemName=timonwong.shellcheck) to vscode
{% endhint %}

## Q\&A

What it a command in trap function exits with non-zero status?

```bash
set -e 
function cleanup {
  ls nonexisting_file
}

trap cleanup ERR
ls nonexisting_file
```

From above the `set -e` would suggest that a trap function could be called endlessly. Luckily that's not the case, so you can sleep calm.



## References

* [Set builtin](https://www.gnu.org/software/bash/manual/html\_node/The-Set-Builtin.html#The-Set-Builtin)
* [Parameter substitution](https://tldp.org/LDP/abs/html/parameter-substitution.html)
* [Google shell guideline ](https://github.com/google/styleguide/blob/gh-pages/shellguide.md)

