# Argo CD

This deploys Argo CD, the GitOps continuous delivery control plane, into the `argocd` namespace.

See the [official documentation](https://argo-cd.readthedocs.io/en/stable/) and the upstream [install guide](https://argo-cd.readthedocs.io/en/stable/getting_started/).

## Components

- **Namespace**: argocd
- **Installation**: upstream manifest

## Usage

1.  Create the namespace and install Argo CD:

    ```bash
    kubectl apply -f k8s/argocd/namespace.yaml
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

2.  Verify the Argo CD server is running:

    ```bash
    kubectl -n argocd rollout status deploy/argocd-server --timeout=120s
    ```

## Accessing the Argo CD UI

The Argo CD server exposes a web UI on port 443. To forward it locally:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open https://localhost:8080 in your browser.

### Default credentials

- **Username:** admin
- **Password:** stored in the `argocd-initial-admin-secret` Kubernetes secret. Retrieve it with:

  ```bash
  kubectl get secret argocd-initial-admin-secret -n argocd \
    -o jsonpath="{.data.password}" | base64 -d
  ```

> **Note:** For advanced customization, see the upstream manifests or use a Kustomize overlay.
