# AWS

## Troubleshoot access denined and unauthorized errors

```
userArn='arn:aws:iam::<redacted>'
startTime=$(date -d '1 hour ago' --iso-8601=seconds)
endTime=$(date --iso-8601=seconds)
( echo "Time,Identity ARN,Event ID,Service,Action,Error,Message";
  aws cloudtrail lookup-events --start-time "$startTime" --end-time "$endTime" --query "Events[*].CloudTrailEvent" --output text \
    | jq -r ". | select(.userIdentity.arn == \"${userArn}\" and .eventType == \"AwsApiCall\" and .errorCode != null
    and (.errorCode | ascii_downcase | (contains(\"accessdenied\") or contains(\"unauthorized\"))))
    | [.eventTime, .userIdentity.arn, .eventID, .eventSource, .eventName, .errorCode, .errorMessage] | @csv"
) | column -t -s'",'

```

Above command produces table like below

```
Time                  Identity ARN             Event ID                              Service            Action        Error         Message
2021-11-17T10:30:33Z  arn:aws:iam::<redacted>  c752dda3-edff-46d3-973b-a192e1364ac7  iam.amazonaws.com  CreatePolicy  AccessDenied  User: arn:aws:iam::<redacted> is not authorized to perform: iam:CreatePolicy on resource: policy <redacted>
2021-11-17T10:30:33Z  arn:aws:iam::<redacted>  fabc4c04-559c-4948-b636-236e3545ac0e  iam.amazonaws.com  CreateRole    AccessDenied  User: arn:aws:iam::<redacted> is not authorized to perform: iam:CreateRole on resource: arn:aws:iam::<redacted>
2021-11-17T10:20:57Z  arn:aws:iam::<redacted>  11936636-70ac-49d2-bac9-b26e0aaa26cc  iam.amazonaws.com  CreatePolicy  AccessDenied  User: arn:aws:iam::<redacted> is not authorized to perform: iam:CreatePolicy on resource: policy <redacted>
2021-11-17T10:20:57Z  arn:aws:iam::<redacted>  95760bea-dec4-475f-9c69-8d82e9770bfa  iam.amazonaws.com  CreateRole    AccessDenied  User: arn:aws:iam::<redacted> is not authorized to perform: iam:CreateRole on resource: arn:aws:iam::<redacted>

```

{% embed url="https://aws.amazon.com/premiumsupport/knowledge-center/troubleshoot-iam-permission-errors" %}

Start EC2 instances

```
aws ec2 start-instances --instance-ids i-04a3559bbecaf4f22 i-0ec0d3dbfd6b2fe21
```

Stop EC2 instances

```
aws ec2 stop-instances --instance-ids i-04a3559bbecaf4f22 i-0ec0d3dbfd6b2fe21
```

Obtain instance public DNS

```
aws ec2 describe-instances --instance-ids i-04a3559bbecaf4f22 i-0ec0d3dbfd6b2fe21 | jq '.Reservations[].Instances[].PublicDnsName' --raw-output
```





## Simple CLI to work with EC2 instances

```
#!/usr/bin/env bash

set -exuo pipefail

readonly instances="i-0ec0d3dbfd6b2fe21 i-0821e53ef5e54c513"
readonly sshPrivatekey="$HOME/.ssh/ec2.pem"
readonly ec2User='ubuntu'

function dns(){
  aws ec2 describe-instances --instance-ids $instances | jq '.Reservations[].Instances[] | .PublicDnsName,.Tags' --raw-output
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
    help - Print help
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
