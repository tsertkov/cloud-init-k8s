# AWS cloud-init user-data

user-data.yaml for Ubuntu Xenial provisioning single node Kubernetes cluster.

Includes:
- superuser with admin priveleges
- dashboard addon
- fixes internet in pods

## Testing

```
$ kubectl proxy

// copy superuser access token to clipboard (macOS)
$ kubectl get secret "$(kubectl get serviceAccount superuser -n kube-system -o jsonpath='{.secrets[0].name}')" -n kube-system -o jsonpath='{.data.token}' | base64 -D | pbcopy
$ open http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

// deploy hello-world app and proxy it to http://localhost:8080/
$ kubectl run node-hello --image=gcr.io/google-samples/node-hello:1.0 --port=8080
$ kubectl port-forward "$(kubectl get pods -l run=node-hello -o jsonpath='{.items[0].metadata.name}')" 8080:8080
```
