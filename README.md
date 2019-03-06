# uPortal-start-helm
[Helm chart](https://helm.sh/) to install uPortal-demo on Kubernetes.

> :warning: This is a proof of concept - this is my first Helm chart - I'm sure there is plenty of room for improvement.

In the first instance MariaDB was arbitrarily chosen as something supported by uPortal and numerous cloud providers.  Also it was my intention not to stray too far from the spirit of [uPortal-start](https://github.com/Jasig/uPortal-start/).

I've tested this on [Docker for Desktop (Windows 10)](https://www.docker.com/products/docker-desktop) with Kubernetes installed locally and on [Azure Kubernetes Service](https://azure.microsoft.com/en-gb/services/kubernetes-service/) in the cloud. 

Note: I needed to increase the default memory available to a Docker image in Windows 10 (Settings > Advanced > Memory Slider up to 4096 MB).

I recommend using [Visual Studio Code](https://code.visualstudio.com/) with the [Kubernetes extension](https://code.visualstudio.com/docs/azure/kubernetes).

This chart uses a very slightly modified version of [uPortal-start](https://github.com/markmclaren/uPortal-start/tree/kubernetes_proofofconcept).  The modified version defaults to including support for accessing MariaDB and adds a new Docker image with some additional tools used to debug database and networking in Kubernetes (mysql-client, bind-tools and nmap).

The *PORTAL_HOME* environment variable and config files (global.properties, uPortal.properties, notification.properties) are set via a Kubernetes ConfigMap.  The uPortal instance is currently configured not to use CAS - because the version of CAS in uPortal-demo does not support external configuration (later versions do).

Also the Docker image command line is determined by the Helm chart values.yaml.

## Installation instructions

Currently I have deployed my new Docker uportal-demo-k8s image to [markmclaren/uportal-demo-k8s](https://hub.docker.com/r/markmclaren/uportal-demo-k8s) on DockerHub.  This is because I believe Helm requires that you acquire your Docker image from a repository (this could be a private repository e.g. an instance of [Azure Container Registry](https://azure.microsoft.com/en-gb/services/container-registry/)).

#### Prerequisites.

I am assuming you have a working Kubernetes cluster with [Helm/Tiller](https://helm.sh/docs/install/) installed.  I am also assuming you have a working Ingress controller installed (I use the [NGINX Ingress Controller]( https://kubernetes.github.io/ingress-nginx/deploy/) be aware it is a two stage install.)  This is necessary so that an externally accessible URL is available.

### Installation

```
git clone https://github.com/markmclaren/uPortal-start-helm
```
Install the MariaDB dependency - this will download a **tgz** file to the charts directory.
```
cd uportal-demo-k8s
helm dependency update
```

Make sure you have your Kubernetes context set correctly (you can use VSCode, [kubectx](https://github.com/ahmetb/kubectx) or [kubectxwin](https://github.com/thomasliddledba/kubectxwin) to do this).

Then whilst inside the **uportal-demo-k8s** directory you should be able to deploy uPortal-start with a supporting MariaDB database by doing:

```
helm install .
```

If not specified Helm will automatically generate a release name (e.g. animal based like *[ugly-monkey](https://github.com/helm/helm/issues/4089)*)

You can monitor the start up process using VSCode.  After a little while you should hopefully be able to access your externally accessible IP address by running:

```
kubectl get svc
```

The EXTERNAL-IP of the LoadBalancer should give you the accessible URL.  Tomcat should be accessible at:

http://\<EXTERNAL-IP>/

and uPortal accessible at:

http://\<EXTERNAL-IP>/uPortal

## Using Helm to manage deployment

You can use Helm to list deployed releases using:

```
helm ls
```

You can check the status of the deployment with:

```
helm status <release-name>
```

You can then undeploy the release using:

```
helm delete <release-name>
```

### Troubleshooting

VSCode/Kubernetes integration has some really nice features for troubleshooting.

* Is the ideal way to monitor the start up process of your helm chart.
* You can also easily access and follow logs.
* You can create a terminal on the running machines.

### Work in progress

This is a proof of concept, I have more to consider in the future:

* Using [Helm secrets](https://github.com/futuresimple/helm-secrets) for encrypting the values in the ConfigMap
* HTTPS access, maybe using [kube-lego](https://github.com/jetstack/kube-lego)
* Consider Kubernetes specific deployment options - e.g. [a sidecar full of WARs](https://github.com/kubernetes/examples/tree/master/staging/javaweb-tomcat-sidecar)
* Consider making it production ready topics, scaling, session affinity, monitoring etc.
