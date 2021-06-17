# Kubernetes

## Inpsecting resource status

Show events that has abnormal status

```text
kubectl get events -A --field-selector type!=Normal --sort-by='{.lastTimestamp}'
```

List all resources from all namespaces

```text
kubectl get all -A --show-kind=true
```

## Istio

Check version

```text
istioctl version --remote
```

## Knative 

Check version

```text
kubectl get namespace knative-serving -o 'go-template={{index .metadata.labels "serving.knative.dev/release"}}'
```

Test routing to internal Knative service

```text
k -n istio-system port-forward svc/cluster-local-gateway 8080:80
# Open another console
NS=<namespace-name>
ROUTE=<route-name>
MYHOST=$(k -n $NS get route.serving.knative.dev $ROUTE -o jsonpath='{.status.url}' | sed 's/http:\/\///g')
curl -v -H "Host: $MYHOST" 127.0.0.1:8080
```

## Remove namespaces in Terminating state

```text
for ns in $(kubectl get ns --field-selector status.phase=Terminating -o jsonpath='{.items[*].metadata.name}'); do  kubectl get ns $ns -ojson | jq '.spec.finalizers = []' | kubectl replace --raw "/api/v1/namespaces/$ns/finalize" -f -; done
```

## 

