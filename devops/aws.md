# AWS

Start EC2 instances

```text
aws ec2 start-instances --instance-ids i-04a3559bbecaf4f22 i-0ec0d3dbfd6b2fe21
```

Stop EC2 instances

```text
aws ec2 stop-instances --instance-ids i-04a3559bbecaf4f22 i-0ec0d3dbfd6b2fe21
```

Obtain instance public DNS

```text
aws ec2 describe-instances --instance-ids i-04a3559bbecaf4f22 i-0ec0d3dbfd6b2fe21 | jq '.Reservations[].Instances[].PublicDnsName' --raw-output
```





Simple CLI to work with EC2 instancies

```text
#!/usr/bin/env bash

set -euo pipefail

readonly instances="i-0ec0d3dbfd6b2fe21 i-0821e53ef5e54c513"
readonly sshPrivatekey="$HOME/.ssh/ec2.pem"
readonly ec2User='ubuntu'

function dns(){
  aws ec2 describe-instances --instance-ids $instances | jq '.Reservations[].Instances[].PublicDnsName' --raw-output
}

function start(){
  aws ec2 start-instances --instance-ids $instances
}

function stop(){
  aws ec2 stop-instances --instance-ids $instances
}


function connect(){
  local domain=$1
  ssh -i "$sshPrivatekey" "${ec2User}@${domain}"
}

function help(){
  echo "Usage:
    connect <domain> - Connect to a given EC2 instance
    dns - Get PublicDnsName of all EC2 instances (defined in \$instances)
    start - Start all instances (defined in \$instances)
    stop - Stop all instances (defined in \$instances)
    "
}

function execute(){
  readonly command=${1:-''}
  shift
  case "$command" in
    connect)
      $command "$@"
      ;;
    dns)
      $command
      ;;
    start)
      $command
      ;;
    stop)
      $command
      ;;
    help)
      $command
      ;;    
    *)
      echo "Unrecognized command: '$command'"
      help
      ;;
  esac
}

execute "$@"
```

