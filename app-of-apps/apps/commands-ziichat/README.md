# Auto Discovery

These ApplicationSets are designed to automatically discover and deploy applications that pertain to the business.

## Usage

- **suffix**

  This ApplicationSet is responsible for ensuring that kustomize overlays from repositories suffixed with `-deployment` are applied to their respective environments. These repositories **SHOULD** contain kustomizations within:

  - ./base
  - ./overlays/{environment}

  The following labels **MUST** be added (easily via the Argo CD UI) to any cluster that you want apps deployed to:

  - kubernetes.io/environment

- **pull-request**

  This ApplicationSet is responsible for deploying PR-specific environments. If a pull request is labeled with <https://github.com/gitops-ziichat/argo-config/labels/preview>, a transient environment will be created, and a GitHub Deployment tracked.

## Reference

The following documentation should be reviewed for more information:

- [ApplicationSet](https://argo-cd.readthedocs.io/en/latest/operator-manual/applicationset/)
- [Matrix Generator](https://argo-cd.readthedocs.io/en/latest/operator-manual/applicationset/Generators-Matrix/)
- [SCM Provider Generator](https://argo-cd.readthedocs.io/en/latest/operator-manual/applicationset/Generators-SCM-Provider/)
- [Git Generator](https://argo-cd.readthedocs.io/en/latest/operator-manual/applicationset/Generators-Git/)
- [Pull Request Generator](https://argo-cd.readthedocs.io/en/latest/operator-manual/applicationset/Generators-Pull-Request/)
- [Cluster Generator](https://argo-cd.readthedocs.io/en/latest/operator-manual/applicationset/Generators-Cluster/)
- [Kustomize](https://argo-cd.readthedocs.io/en/latest/user-guide/kustomize/)
- [Go Template](https://argo-cd.readthedocs.io/en/latest/operator-manual/applicationset/GoTemplate/)
- [Sync Options](https://argo-cd.readthedocs.io/en/latest/user-guide/sync-options/)
