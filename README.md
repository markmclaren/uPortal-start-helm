# uPortal-start-helm
Helm chart to install uPortal-demo on Kubernetes (Proof of concept)

I've tested this on [Docker for Desktop (Windows 10)](https://www.docker.com/products/docker-desktop) and in [Azure Kubernetes Service](https://azure.microsoft.com/en-gb/services/kubernetes-service/).

I recommend using [Visual Studio Code](https://code.visualstudio.com/) with the Kubernetes extension.

Uses a very slightly modified version of [uPortal-start](https://github.com/markmclaren/uPortal-start/tree/kubernetes_proofofconcept).  The modified version defaults to including support for accessing MariaDB and adds a new Docker image with some addition tools used to debug database and networking in Kubernetes.

The PORTAL_HOME environment variable and config files are set via a Kubernetes ConfigMap.  
Also the Docker image command line is detemined by the Helm chart values.yaml.

> This is my first Helm chart - so I'm sure there is room for improvement.
