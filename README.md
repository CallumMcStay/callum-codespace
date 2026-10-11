# Kubernetes Basics with Kind

A starter repo for learning the basics of Kubernetes (K8s). You run a small, local Kubernetes cluster using [Kind](https://kind.sigs.k8s.io/) (Kubernetes IN Docker) inside a GitHub Codespace, so you don't need to install anything on your own machine.

## What's in this repo

| File | Purpose |
| --- | --- |
| `.devcontainer/devcontainer.json` | Configures the Codespace: Docker (docker-in-docker), `kubectl`, and `kind` are installed automatically. |
| `cluster.yaml` | Kind cluster config. It defines a cluster called `democluster` with one control-plane node and one worker node. |

## Getting started

### 1. Open the repo in a Codespace

1. On the repo's GitHub page, click **Code** → **Codespaces** → **Create codespace on main**.
2. Wait for the Codespace to build. The first build takes a few minutes because it installs Docker, `kubectl`, and `kind`.

### 2. Check that the tools are installed

In the Codespace terminal, run:

```bash
docker version
kubectl version --client
kind version
```

Each command should print version information without errors.

### 3. Create the cluster

```bash
kind create cluster --config cluster.yaml
```

Kind starts one Docker container for each node in `cluster.yaml`. It also points `kubectl` at the new cluster by setting the context to `kind-democluster`.

### 4. Explore the cluster

```bash
kubectl cluster-info --context kind-democluster
kubectl get nodes
kubectl get pods -A
```

You should see two nodes, `democluster-control-plane` and `democluster-worker`, both with status `Ready`. It can take up to a minute for them to become `Ready`.

Each node is really a Docker container:

```bash
docker ps
```

## Useful commands

```bash
kind get clusters                         # list Kind clusters
kubectl config current-context            # check which cluster kubectl is using
kubectl get all                           # list common resources in the default namespace
kubectl describe node democluster-worker  # detailed info about a node
```

## Cleaning up

Delete the cluster when you're done:

```bash
kind delete cluster --name democluster
```

To save your Codespace hours, stop the Codespace when you're not using it. Go to **Code** → **Codespaces** → **…** → **Stop codespace**, or manage your Codespaces at <https://github.com/codespaces>.

## Troubleshooting

- **`kind: command not found`**: the post-create step may have failed. Check the Codespace creation log, or rebuild the container with **Ctrl/Cmd+Shift+P** → *Codespaces: Rebuild Container*.
- **Cluster isn't working after the Codespace restarts**: Kind clusters don't always survive a stop and restart. Delete the cluster and create it again:
  ```bash
  kind delete cluster --name democluster
  kind create cluster --config cluster.yaml
  ```
- **`kubectl` talks to the wrong cluster**: switch the context with `kubectl config use-context kind-democluster`.

## Further reading

- [Kind Quick Start](https://kind.sigs.k8s.io/docs/user/quick-start/)
- [Kubernetes Basics tutorial](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/quick-reference/)
