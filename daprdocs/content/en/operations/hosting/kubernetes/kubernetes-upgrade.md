---
type: docs
title: "Upgrade Dapr on a Kubernetes cluster"
linkTitle: "Upgrade Dapr"
weight: 30000
description: "Follow these steps to upgrade Dapr on Kubernetes and ensure a smooth upgrade."
---

## Prerequisites

- [Dapr CLI]({{< ref install-dapr-cli.md >}})
- [Helm 3](https://github.com/helm/helm/releases) (if using Helm) 

## Upgrade existing cluster to 1.0.1
There are two ways to upgrade the Dapr control plane on a Kubernetes cluster using either the Dapr CLI or Helm.

### Dapr CLI

The example below shows how to upgrade to version 1.0.1:

  ```bash
  dapr upgrade -k --runtime-version=1.0.1
  ```

You can provide all the available Helm chart configurations using the Dapr CLI.
See [here](https://github.com/dapr/cli#supplying-helm-values) for more info.

### Helm

From version 1.0.0 onwards, upgrading Dapr using Helm is no longer a disruptive action since existing certificate values will automatically be re-used.

1. Upgrade Dapr from 1.0.0 (or newer) to any [NEW VERSION] > v1.0.0:

   ```bash
   helm repo update
   ```
   
   ```bash
   helm upgrade dapr dapr/dapr --version [NEW VERSION] --namespace dapr-system --wait
   ```
   *If you're using a values file, remember to add the `--values` option when running the upgrade command.*

2. Ensure all pods are running:

   ```bash
   kubectl get pods -n dapr-system -w
   
   NAME                                     READY   STATUS    RESTARTS   AGE
   dapr-dashboard-69f5c5c867-mqhg4          1/1     Running   0          42s
   dapr-operator-5cdd6b7f9c-9sl7g           1/1     Running   0          41s
   dapr-placement-server-0                  1/1     Running   0          41s
   dapr-sentry-84565c747b-7bh8h             1/1     Running   0          35s
   dapr-sidecar-injector-68f868668f-6xnbt   1/1     Running   0          41s
   ```

3. Restart your application deployments to update the Dapr runtime:

   ```bash
   kubectl rollout restart deploy/<DEPLOYMENT-NAME>
   ```

4. All done!

## Next steps

- [Dapr on Kubernetes]({{< ref kubernetes-overview.md >}})
- [Dapr production guidelines]({{< ref kubernetes-production.md >}})