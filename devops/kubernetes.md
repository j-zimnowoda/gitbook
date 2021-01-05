# Kubernetes



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



