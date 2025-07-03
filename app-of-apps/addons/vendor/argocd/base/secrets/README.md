# Secrets

There are a few secrets that need to be created in order to allow Argo CD to manage itself and the rest of the applications.

## GitHub

### Repositories

- <https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#repositories>

If application manifests are located in private repository then repository credentials have to be configured. This is also used by the Image Updater to update the image tags in the manifests.

```sh
kubectl create -n argocd secret generic repo-creds \
  --from-literal=type=git \
  --from-literal=url=https://github.com/gitops-ziichat \
  --from-literal=username=$GITHUB_ACTOR \
  --from-literal=password=github_pat_XXX

kubectl label -n argocd secret repo-creds "argocd.argoproj.io/secret-type=repo-creds"
```

### ApplicationSet

- <https://argo-cd.readthedocs.io/en/stable/operator-manual/applicationset/Generators-Pull-Request/#github>

Argo CD will be polling repositories for pull requests that need preview environments. We need to create a secret to allow Argo CD to authenticate with GitHub to access private repos and bypass rate limits for public repos:

```sh
kubectl create -n argocd secret generic github-token \
  --from-literal=token="github_pat_XXX"
```

TODO: Once the cluster is stable and accessible to the internet, we can leverage [webhooks to trigger Application generation](https://argo-cd.readthedocs.io/en/stable/operator-manual/applicationset/Generators-Pull-Request/#webhook-configuration).

### Notifications

- <https://argo-cd.readthedocs.io/en/stable/operator-manual/notifications/#getting-started>

We also need to create a secret to allow Argo CD notifications to call the GitHub API (which updates deployment statuses)

```sh
kubectl delete -n argocd secret argocd-notifications-secret
kubectl create -n argocd secret generic argocd-notifications-secret \
  --from-literal=github-token="github_pat_XXX"
```

### Image Updater

- <https://argocd-image-updater.readthedocs.io/en/stable/basics/authentication/>

```sh
kubectl create -n argocd secret docker-registry github-registry \
  --docker-username $GITHUB_ACTOR \
  --docker-password github_pat_XXX \
  --docker-server "https://ghcr.io"
```
