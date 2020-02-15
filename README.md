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

### Configuration Google Cloud GKE (Google Kubernetes Engine)

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

## 3) Description en détail de tous les configurations  et pas à suivre

## 4) Les code source et scripts élabore. 

## 5) Diapos de votre présentation en PDF.
