# Run & Test & Clean

## Preparing

```shell
./.script/kind-with-registry.sh
export MEMCACHED_IMAGE="memcached:1.6.23-alpine3.19"
```

## Docker

The script provided at [this link](https://kind.sigs.k8s.io/docs/user/local-registry/).

```shell
# Docker Hub
make docker-build docker-push IMG=smhmayboudi/memcached-operator:v0.0.1
make deploy IMG=smhmayboudi/memcached-operator:v0.0.1

# Local Registry
make docker-build docker-push IMG=localhost:5001/smhmayboudi/memcached-operator:v0.0.1
make deploy IMG=kind-registry:5000/smhmayboudi/memcached-operator:v0.0.1

kubectl apply -f config/samples/

make undeploy
```

## OLM

```shell
operator-sdk olm install

# Docker Hub
make bundle IMG=smhmayboudi/memcached-operator:v0.0.1
make bundle-build bundle-push BUNDLE_IMG=smhmayboudi/memcached-operator-bundle:v0.0.1
operator-sdk run bundle smhmayboudi/memcached-operator-bundle:v0.0.1

# Local Registry
make bundle IMG=localhost:5001/smhmayboudi/memcached-operator:v0.0.1
make bundle-build bundle-push BUNDLE_IMG=localhost:5001/smhmayboudi/memcached-operator-bundle:v0.0.1
operator-sdk run bundle kind-registry:5000/smhmayboudi/memcached-operator-bundle:v0.0.1

kubectl apply -f config/samples/

operator-sdk cleanup custom-kubernetes-controller
```

## Direct

```shell
# Docker Hub
make deploy IMG="smhmayboudi/memcached-operator:v0.0.1"

# Local Registry
make deploy IMG="kind-registry:5000/smhmayboudi/memcached-operator:v0.0.1"

kubectl apply -f config/samples/

make undeploy
```

## locally

```shell
make install run

kubectl apply -f config/samples/

make uninstall
```

## Check the Deployments

```shell
kubectl get deployments
kubectl logs deployments/dummy1
kubectl describe deployments/dummy1
```

# Bundle

```shell
make bundle
make bundle-build
```

# Shell

```shell
# If shell completion is not already enabled in your environment you will need
# to enable it. You can execute the following once:
$ echo "autoload -U compinit; compinit" >> ~/.zshrc

# To load completions for each session, execute once:
$ operator-sdk completion zsh > "${fpath[1]}/_operator-sdk"

# You will need to start a new shell for this setup to take effect.
```
