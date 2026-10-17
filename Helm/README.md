# Helm Notes – Installation, Repositories & Charts

## 1. What is Helm?

- **Helm is a package manager for Kubernetes**, similar to `apt` in Ubuntu. 
- It helps us install, upgrade, and uninstall applications on a Kubernetes cluster.

- **a. Concepts of Repository in Helm:** A centralized location that stores and distributes Helm charts.
- When we do `apt install nginx` apt connects to Default Repository, (https://archive.ubuntu.com). All the packages like nginx, java are the part of this repo. similar we need to have a Helm Repository, which has all the charts.
  
- **b. Concepts of Chart in Helm:** A bundle or package of an application.
- **Examples:**
- Bundle of Prometheus is called as Prometheus chart
- Bundle of Grafana is called as Grafana chart
- so within the repo. which ever chart we have we can only install those charts.

- **c. Concepts of Release in Helm:** Release is the deployed instance of our chart.



## 2. Install Helm on Ubuntu

Follow the [official Helm installation guide](https://helm.sh/docs/intro/install/).

After installation, verify Helm and Kubernetes connectivity:

```bash
helm version
kubectl get nodes
```

## 3. Add a Helm Repository

- Helm downloads charts from configured chart sources.

Bitnami is an example of a chart publisher.

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm repo list
```

- `helm repo add` — Adds a chart repository.
- `helm repo update` — Refreshes local repository metadata.
- `helm repo list` — Lists configured repositories.

> **Note:** Check the publisher's current documentation to confirm repository URLs and chart availability.

## 4. Search for Charts

```bash
helm search repo bitnami
helm search repo bitnami | grep -i nginx
```

Example chart reference: `bitnami/nginx`

Here, `bitnami` is the repository name and `nginx` is the chart name.

If a chart is not in your configured repositories, search [Artifact Hub](https://artifacthub.io/) or the application's official documentation.

## 5. Install an Application Using Helm

**Syntax:**

```bash
helm install <release-name> <repository>/<chart-name>
```

**Example:**

```bash
helm install nginxv1 bitnami/nginx
```

Verify the installation:

```bash
helm list
helm status nginxv1
kubectl get pods
```

In this example:

- `nginxv1` — Release name.
- `bitnami/nginx` — Chart reference.
- `kubectl get pods` — Checks the Kubernetes pods.

## 6. Install Charts from Other Sources

Not every application is available in Bitnami. For example, Argo CD and AWS Load Balancer Controller have their own installation instructions and chart sources.

Search for charts:

```bash
helm search hub argocd
helm search hub aws-load-balancer-controller
```

Then follow the official documentation to configure the correct chart source and install the application.

- [Argo CD installation](https://argo-cd.readthedocs.io/en/stable/operator-manual/installation/)
- [AWS Load Balancer Controller installation](https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html)

## 7. Upgrade and Uninstall a Release

Upgrade an existing release (use the correct chart reference and desired configuration):

```bash
helm upgrade nginxv1 <repository>/<chart-name>
```

List releases:

```bash
helm list
```

Uninstall a release:

```bash
helm uninstall nginxv1
```

## 8. Create Your Own Helm Chart

Helm lets us package our own applications into reusable charts and deploy them on Kubernetes.

Create a chart:

```bash
helm create myapp
```

Important files:

- `Chart.yaml` — Chart name, version, and metadata.
- `values.yaml` — Default application configuration.
- `templates/` — Kubernetes resource templates, such as Deployments and Services.

Validate and deploy:

```bash
helm lint myapp
helm template myapp ./myapp
helm install myapp-release ./myapp
```

Package the chart:

```bash
helm package myapp
```

**Workflow:** Create chart → Configure → Validate → Package → Publish → Deploy.

## Quick Revision

| Command | Purpose |
|---|---|
| `helm repo add NAME URL` | Add a repository |
| `helm repo update` | Refresh repository metadata |
| `helm search repo KEYWORD` | Search configured repositories |
| `helm install RELEASE CHART` | Install an application |
| `helm list` | List releases |
| `helm upgrade RELEASE CHART` | Upgrade a release |
| `helm uninstall RELEASE` | Uninstall a release |
| `helm create CHART` | Create a chart |

**Remember:** Repository = chart source; Chart = application package; Release = deployed instance.


