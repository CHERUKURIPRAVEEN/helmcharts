# Helmcharts
### What is helm charts
* Helm is a package manager
* Helm is a tool which allows to manage Kubernetes based applications by providing a standard approach for defining how we install, upgrade and uninstall applications
* It is a package manager for Kubernetes and the best way to find, share, and use software built for Kubernetes
* Allows developers to easily package, configure, and deploy applications and services onto Kubernetes clusters through Helm Charts
* Charts are files that are written in YAML and packaged in a particular directory structure which can be versioned within any source control systems. These charts describe a set of Kubernetes resources which makes up the entire application
* Helm is an official Kubernetes project and is part of CNCFS
#### Helm Can:
- [x] Install software
- [x] Automatically install software dependencies
- [x] Upgrade software
- [x] Configure software deployments
- [x] Fetch software packages from repositories called Charts repository

**Helm packages**
```helm
helm install my-release bitnami/nginx
helm upgrade my-release bitnami/nginx
```

#### What makes Helm so popular?
* Offers a simple way to package all application resources into one single chart and advertises what you can configure
* Hiding the complexity of running multiple YAML files to deploy the application. Instead we run a single command that install all YAML files at once
* One click install/upgrade/rollback applications
* Easy management of application releases
* Same chart can be used for different environment like dev, staging and prod Reusable charts from helm repositories

#### Without Helm
* Write and modify large set of Kubernetes manifests
* Do this every time a release is needed
* No packaging of files
* Rollbacks are possible at the YAML level. If an application has to be rolled back, all its
* dependent YAML files/resources are to be rolled back. Since no version is maintained for all resources, rollbacks are not easy
* No concept of application rollback since no application versioning is done in the cluster Figure out your own sharing: Git offline etc
* Tweak resources by hand

#### helm2 Architecture

#### helm3 Architecture

#### Helm Common terminologies
* Helm client/helm
1. Helm Client is a command line tool for developing Charts, managing repositories and releases, and interfacing with the Helm Library

* Tiller
1. Tiller is the Helm server.
2. It interacts directly with the Kubernetes API server to install, upgrade, query, and remove Kubernetes resources. It is installed in the Kubernetes cluster and runs as a pod. Tiller is removed in Helm3.

* Chart
1. It contains all of the resource definitions necessary to run an application, tool, or service inside of a Kubernetes cluster. 2. A chart is basically a package of pre-configured Kubernetes resources.

* Release
1. An instance of a chart running in a Kubernetes cluster.
2. Same chart can be deployed multiple times in multiple environments.

* Repository
1. Place where charts reside and can be shared with others.

#### Helm3 - What changed from Helm2
* Tiller removed in Helm3
1. When Helm 2 was developed, Kubernetes did not had role-based access control (RBAC) So, Helm had to take care of authorizing who and what actions a user can perform in the cluster
2. With Kubernetes 1.6, RBAC is enabled by default, so there is no need for Helm to do the same job which can be done natively by Kubernetes and that's why in Helm 3 tiller is removed completely

* Three-way strategic merge patch
1. In Helm 2 a patch would be generate by comparing the old manifest against the new manifest
2. If someone manually changed the deployment, the helm has no info regarding that and rollbacks would give erroneous results
3. In Helm 3, the patch is generated using the old manifest, the live state of the cluster, and the new manifest

* Secrets as the default storage driver
1. Helm 2 used Config Maps to store release information. In Helm 3, Secrets are used instead as the default storage driver

* JSON Schema Chart Validation
1. A JSON Schema validation can now be forced upon chart values
2. With this functionality, we can ensure that values provided by the user follow the schema created by the chart maintainer
Ex: Port number can be made mandatory and without which the chart cannot be installed

* Release name is now required
1. In Helm 2, if no name was provided, a random release name would be generated. Helm 3 will throw an error if no name is provided with helm install
2. Helm 2 charts are compatible with Helm 3, although the chart structure might differ

#### Helm: Three-way strategic merge patch
* Helm 3 uses three way strategic merge patch.
* It means if any helm operation is to be performed like version upgrade, it compares the most recent manifest chart(v1) against the proposed chart manifest(v2) and also the live state of the cluster.
* So, any manual changes made to the cluster using kubectl (for example kubectl edit), will also be considered and changes are retained in the final release.

#### Insatlling the Helm
* From the Binary Releases
Every release of Helm provides binary releases for a variety of OSes. These binary versions can be manually downloaded and installed.
1. Download your [desired version](https://github.com/helm/helm/releases)
2. Unpack it (tar -zxvf helm-v3.0.0-linux-amd64.tar.gz)
3. Find the helm binary in the unpacked directory, and move it to its desired destination (mv linux-amd64/helm /usr/local/bin/helm)
4. From there, you should be able to run the client and add the stable repo: helm help.

* From Homebrew (macOS)
Members of the Helm community have contributed a Helm formula build to Homebrew. This formula is generally up to date.
```
brew install helm
```

* From Apt (Debian/Ubuntu)
Members of the Helm community have contributed a Helm package for Apt. This package is generally up to date.

```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

* From dnf/yum (fedora)
Since Fedora 35, helm is available on the official repository. You can install helm with invoking:
```
sudo dnf install helm
```
ref: helm install official page link [here](https://helm.sh/docs/intro/install/)
