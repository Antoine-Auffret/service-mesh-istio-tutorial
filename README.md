# Service mesh with Istio : tutorial

## 1) Présentation de la technologie

**Service mesh** = Network of microservices

**Istio** = Complete solution to manage a service mesh

**Core features of Istio** = Traffic management, Security, Policies, Observability

**Features Istio** = Load balancing, secure service-to-service authentication, monitoring, traffic control, policy layer, automatic metrics/logs

**Sidecar proxy** = network communication between microservices

**Platform support** = Spanning Cloud, on-premise, Kubernetes, Mesos, and more

## 2) Objectifs du tutoriel - contexte, description et résultats/connaisses  attendus après  l’exécution

Before you can install Istio, you need a cluster running a compatible version of Kubernetes. Istio 1.4 has been tested with Kubernetes releases 1.13, 1.14, 1.15.

**Google Cloud** = n1-standard-1 (4 nodes)

**Cluster Kubernetes** = 1.15.7-gke.23

**Istio** = 1.4.4

## 3) Description en détail de tous les configurations  et pas à suivre

### Configuration Google Cloud GKE (Google Kubernetes Engine)

![](img/createClusterButton.png)

Modèle 'Cluster standard' (modifié) 

Version maître : 1.15.7-gke.23

Nombre de nœuds : 4

Type de machine : n1-standard-1

Disponibilité, mise en réseau, sécurité et autres fonctionnalités 
Activer Istio (bêta)

```
gcloud container clusters list
```

```
gcloud container clusters get-credentials CLUSTER_NAME
```

```
kubectl get service -n istio-system
```

```
kubectl get pods -n istio-system
```

```
kubectl label namespace istio-system istio-injection=enabled --overwrite=true
```

Retrieve your credentials for ```kubectl```.
```
gcloud container clusters get-credentials <cluster-name> \
    --zone <zone> \
    --project <project-id>
```

Grant cluster administrator (admin) permissions to the current user. To create the necessary RBAC rules for Istio, the current user requires admin permissions.
```
kubectl create clusterrolebinding cluster-admin-binding \
    --clusterrole=cluster-admin \
    --user=$(gcloud config get-value core/account)
```

### Install Istio

```
curl -L https://istio.io/downloadIstio | sh -
```

```
cd istio-1.4.4
```

```
mv bin/istioctl /usr/local/bin
```

Enabling auto-completion
```
cp tools/istioctl.bash ~/.
```

```
source ~/istioctl.bash
```

Perform pre-check for your cluster and report whether the cluster is ready for Istio installation
```
istioctl verify-install
```

Choose the profile you want to install

<i></i>|default|demo|minimal|sds|remote
---|:---:|:---:|:---:|:---:|:---:
**Corecomponents**|   |   |   |   |   
`istio-citadel`| x | x |   | x | x 
`istio-egressgateway`|   | x |   |   |   
`istio-galley`| x | x |   | x |   
`istio-ingressgateway`| x | x |   | x |   
`istio-nodeagent`|   |   |   | x |   
`istio-pilot`| x | x | x | x |   
`istio-policy`| x | x |   | x |   
`istio-sidecar-injector`| x | x |   | x | x 
`istio-telemetry`| x | x |   | x |   
**Addons**|   |   |   |   |   
`grafana`|   | x |   |   |   
`istio-tracing`|   | x |   |   |   
`kiali`|   | x |   |   |   
`prometheus`| x | x |   | x |   

Install the built-in configuration profile demo to test all the features of Istio
```
istioctl manifest apply --set profile=demo
```

Test dashboard (Web UI)
```
istioctl dashboard
```

Choose one
```
istioctl dashboard grafana
```

## 4) Les code source et scripts élabore. 

## 5) Diapos de votre présentation en PDF.
