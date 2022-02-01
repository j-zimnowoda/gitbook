# Kubernetes

## General

Show all resources in all namesapces

```
kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found -A
```

## RBAC

Find all (cluster)rolebindigs that grants roles to the unauthenticated users

```
k get clusterrolebinding -ojson | jq '.items[] | select( .subjects != null) | select( .subjects[].name=="system:unauthenticated") | .metadata.name'
```

```
k get rolebinding -A -ojson | jq '.items[] | select( .subjects != null) | select( .subjects[].name=="system:unauthenticated") | .metadata.name'
```

## Kubelet

Explore kubelet logs from last 2 minutes

```
sudo journalctl -u kubelet --since "2 minutes ago"
```

API access

In order to verify if annonymous access to kubelet api is allowed, perform the following command

```
curl -sk https://localhost:10250/pods
```

Check if readonly port is disabled

```
curl -sk https://localhost:10255/metrics
```

## Ingress

For each Ingress print all its backends

```
k -n istio-system get ingress -A -ojsonpath='{range .items[*]}{.metadata.name} {"\t\t\t"} {.spec.rules[*].http.paths[*].backend.serviceName} {"\n"}'
```



## Events

### Inspecting resource status

Show events that has abnormal status

```
kubectl get events -A --field-selector type!=Normal --sort-by='{.lastTimestamp}'
```

List all resources from all namespaces

```
kubectl get all -A --show-kind=true
```

## Namespaces

### Remove namespaces in Terminating state

```
for ns in $(kubectl get ns --field-selector status.phase=Terminating -o jsonpath='{.items[*].metadata.name}'); do  kubectl get ns $ns -ojson | jq '.spec.finalizers = []' | kubectl replace --raw "/api/v1/namespaces/$ns/finalize" -f -; done
```



## Istio

Check version

```

istioctl version --remote
```

```
ns='my-ns'
pod='my-pod'
kubectl exec -n $ns $pod -- curl -X POST http://localhost:15000/logging\?level\=debug
```



### Gateway

For each gateway print value of `istio` selector

```
kubectl get gateway -A -o=jsonpath='{range .items[*]}{.metadata.name}{"\t\t"}{.spec.selector.istio}{"\n"}{end}'
```



## Knative&#x20;



Check version

```
kubectl get namespace knative-serving -o 'go-template={{index .metadata.labels "serving.knative.dev/release"}}'
```

Test routing to internal Knative service

```
k -n istio-system port-forward svc/cluster-local-gateway 8080:80
# Open another console
NS=<namespace-name>
ROUTE=<route-name>
MYHOST=$(k -n $NS get route.serving.knative.dev $ROUTE -o jsonpath='{.status.url}' | sed 's/http:\/\///g')
curl -v -H "Host: $MYHOST" 127.0.0.1:8080
```

## Kubeconfig

Get public certificate for a specific user

```
cat ~/.kube/config | yq eval -j | jq '.users[] | select(.name=="restricted@infra-prod") |  .user["client-certificate-data"]' -r | base64 -d
```

## Minikube

Start minikube with NetworkPolicies enabled

```
minikube start --network-plugin=cni --cni=calico --kubernetes-version=1.20.1
```

Push container images ([read more](https://minikube.sigs.k8s.io/docs/handbook/pushing/#2-push-images-using-cache-command))

```
minikube cache add alpine:latest
```

## Secrets

Decode all values in a given secret

```
name='mysecret'  
k get "$name" -o json | jq -r '.data[] |= @base64d| .data' | yq e -P
```



## Pods

### Security context

#### Seccomp

```
# /etc/lib/kubelet/profiles/audit.json
{
    "defaultAction": "SCMP_ACT_LOG"
}

```



```
# Reference seccomp profile in Pod security context
apiVersion: v1
kind: Pod
metadata:
  name: audit-pod
  labels:
    app: audit-pod
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
```

Inspect logs

```json
grep syscall /var/log
```

## Get pod labels

```
k get pod -A -ojsonpath='{range .items[*]}{.metadata.name} {"\n"}{.metadata.labels} {"\n\n"}'
```

## Service

Get selectors for each service

```
k get svc -A -ojsonpath='{range .items[*]}{.metadata.name} {"\n"}{.spec.selector} {"\n\n"}
```

