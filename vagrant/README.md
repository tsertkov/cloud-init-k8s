# Vagrant managed single node Kubernetes

Dependencies:
```shell
brew cask install virtualbox
brew cask install vagrant
brew install cdrtools
brew install kubectl
```

Bring k8s up, connect to dashboard, deploy demo app, etc.:
```shell
$ export VM=ubuntu
$ make up
$ make kubeconfig
$ eval $(make kubectl-env)
$ eval $(make vagrant-env)
$ make dashboard
$ make copy-token
$ make hello-world && watch kubectl get pods
$ make hello-world-proxy
```

Destroy vm:
```shell
$ make destroy
```
