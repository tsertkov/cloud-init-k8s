# Vagrant managed single node Kubernetes

Install dependencies:
```
brew cask install virtualbox
brew cask install vagrant
brew install cdrtools
brew install kubectl
```

Bring k8s up, connect to dashboard, deploy demo app:
```
$ make up
$ eval $(make kubectl-env)
$ make dashboard
$ make copy-token
$ make hello-world && watch kubectl get pods 
$ make hello-world-proxy
```

Destroy vm:
```
$ make destroy
```
