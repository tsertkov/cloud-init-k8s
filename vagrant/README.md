# Vagrant vm with single node Kubernetes

```
$ make up
$ eval $(make kubectl-env)
$ make dashboard
$ make copy-token
$ make hello-world
$ watch kubectl get pods
$ make hello-world-proxy
$ make destroy
```
