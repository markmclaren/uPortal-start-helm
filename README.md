# uPortal-start-helm
Helm chart to install uPortal-demo on Kubernetes.

> :warning: This is a proof of concept - I am not very experienced with Kubernetes yet and this is my first Helm chart - so I'm sure there is plenty of room for improvement.

I've tested this on [Docker for Desktop (Windows 10)](https://www.docker.com/products/docker-desktop) with Kubernetes installed and in [Azure Kubernetes Service](https://azure.microsoft.com/en-gb/services/kubernetes-service/).

I recommend using [Visual Studio Code](https://code.visualstudio.com/) with the Kubernetes extension.

Uses a very slightly modified version of [uPortal-start](https://github.com/markmclaren/uPortal-start/tree/kubernetes_proofofconcept).  The modified version defaults to including support for accessing MariaDB and adds a new Docker image with some addition tools used to debug database and networking in Kubernetes.

The PORTAL_HOME environment variable and config files are set via a Kubernetes ConfigMap.  
Also the Docker image command line is detemined by the Helm chart values.yaml.

## Installation instructions

Currently I have deployed my new Docker uportal-demo-k8s image to [markmclaren/uportal-demo-k8s](https://hub.docker.com/r/markmclaren/uportal-demo-k8s) on DockerHub.  This is because I believe Helm requires that you acquire your Docker image from a repository (this could be a private repository e.g. an instance of ([Azure Container Registry](https://azure.microsoft.com/en-gb/services/container-registry/)).

I am assuming you have a working Kubernetes cluster with [Helm/Tiller](https://helm.sh/docs/install/) installed.

```
git clone https://github.com/markmclaren/uPortal-start-helm
```
Install the MariaDB dependency - this will download a tgz file to the charts directory.
```
cd uportal-demo-k8s
helm dependency update
```
