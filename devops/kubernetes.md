# Kubernetes

## General

Show all resources in all namesapces

```
kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found -A
```

## RBAC

Find all cluster rolebindig that grants roles to unauthenticated users

```
k get clusterrolebinding -ojsonpath='{.items[?(@.subjects[].name=="system:unauthenticated")].metadata.name}'
```

```
k get rolebinding -ojsonpath='{.items[?(@.subjects[].name=="system:unauthenticated")].metadata.name}'
```

## Kubelet

Explore kubelet logs from last 2 minutes

```
sudo journalctl -u kubelet --since "2 minutes ago"
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

### Gateway

For each gateway print value of `istio` selector

```
kubectl get gateway -A -o=jsonpath='{range .items[*]}{.metadata.name}{"\t\t"}{.spec.selector.istio}{"\n"}{end}'
```

## Knative 



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

##
