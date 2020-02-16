# Service mesh with Istio : tutorial

## 1) Présentation de la technologie

**Service mesh** = Network of microservices

**Istio** = Complete solution to manage a service mesh

**Core features of Istio** = Traffic management, Security, Policies, Observability

**Features Istio** = Load balancing, secure service-to-service authentication, monitoring, traffic control, policy layer, automatic metrics/logs

**Sidecar proxy** = network communication between microservices

**Ingress gateway** = make the application accessible from outside of the Kubernetes cluster

**Platform support** = Spanning Cloud, on-premise, Kubernetes, Mesos, and more

## 2) Objectifs du tutoriel - contexte, description et résultats/connaisses  attendus après  l’exécution

Before you can install Istio, you need a cluster running a compatible version of Kubernetes. Istio 1.4 has been tested with Kubernetes releases 1.13, 1.14, 1.15.

**Google Cloud** = n1-standard-1 (4 nodes)

**Cluster Kubernetes** = 1.15.7-gke.23

**Istio** = 1.4.4

## 3) Description en détail de tous les configurations  et pas à suivre

### Configuration Google Cloud GKE (Google Kubernetes Engine)

Créer un compte Google : https://accounts.google.com/signup

Connecter vous sur le cloud de Google : https://console.cloud.google.com

Activer votre compte cloud gratuitement (300$ de crédits offert ou pendant 1 an)

Créer un nouveau projet

### Créer un nouveau cluster Kubernetes

![](img/createClusterButton.png)

Choisir un modèle 'Cluster standard'

![](img/createClusterStandard1.png)

Nom : istio-demo

Version maître (Kubernetes) : 1.15.7-gke.23

![](img/createClusterStandard2.png)

Nombre de nœuds : 4 (nécessaire pour tester toutes les fonctionnalités d'Istio)

Type de machine : n1-standard-1

> Il est possible d'activer Istio à la création du cluster dans l'onglet "Disponibilité, mise en réseau, sécurité et autres fonctionnalités" puis "Activer Istio (bêta)". Pour ce tutoriel nous allons installer Istio à la main pour comprendre son fonctionnement.

Appuyer sur bouton "Créer"

> Le cluster peut mettre plusieurs minutes à se créer

![](img/clusterList.png)

### Vérification de l'installation et configuration du cluster Kubernetes

Pour accéder à la console (shell) cliquer sur l'icone en haut à doite

![](img/cloudShellIcon.png)

Vérifier l'état du cluster dans la console
```
$ gcloud container clusters list
NAME        LOCATION       MASTER_VERSION  MASTER_IP     MACHINE_TYPE   NODE_VERSION   NUM_NODES  STATUS
istio-demo  us-central1-a  1.15.7-gke.23   35.238.31.56  n1-standard-1  1.15.7-gke.23  4          RUNNING
```

Obtener les informations d'identification de votre cluster afin de pouvoir interagir avec lui avec la commande `kubectl`.
Préciser la zone (LOCATION) de votre cluster, ici `us-central1-a`.
```
$ gcloud container clusters get-credentials istio-demo --zone us-central1-a
Fetching cluster endpoint and auth data.
kubeconfig entry generated for istio-demo.
```

Activer l'auto-completion pour la commande `kubectl`
```
$ kubectl completion bash > ~/kubectl.bash
```
```
$ source ~/kubectl.bash
```

Vérifier la version de Kubernetes
```
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.9", GitCommit:"2e808b7cb054ee242b68e62455323aa783991f03", GitTreeState:"clean", BuildDate:"2020-01-18T23:33:14Z", GoVersion:"go1.12.12", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"15+", GitVersion:"v1.15.7-gke.23", GitCommit:"06e05fd0390a51ea009245a90363f9161b6f2389", GitTreeState:"clean", BuildDate:"2020-01-17T23:10:45Z", GoVersion:"go1.12.12b4", Compiler:"gc", Platform:"linux/amd64"}
```

Accordez des autorisations d'administrateur de cluster (admin) à l'utilisateur actuel. Pour créer les règles nécessaires pour Istio, l'utilisateur actuel nécessite des autorisations d'administrateur.
```
$ kubectl create clusterrolebinding cluster-admin-binding \
    --clusterrole=cluster-admin \
    --user=$(gcloud config get-value core/account)
Your active configuration is: [cloudshell-26194]
clusterrolebinding.rbac.authorization.k8s.io/cluster-admin-binding created
```

### Installation d'Istio

Télécharger Istio dans le répertoire home du cluster
```
$ curl -L https://istio.io/downloadIstio | sh -
```

Ajouter la commande `istioctl` à la variable d'environnement PATH (en remplaçant a_y_auffret par votre nom)
```
$ export PATH="$PATH:/home/a_y_auffret/istio-1.4.4/bin"
```

Activer l'auto-completion pour la commande `istioctl`
```
$ cp ~/istio-1.4.4/tools/istioctl.bash ~/.
```
```
$ source ~/istioctl.bash
```

Lancer la vérification de pré-installation d'Istio pour savoir si le cluster est prêt à installer Istio
```
$ istioctl verify-install
Install Pre-Check passed! The cluster is ready for Istio installation.
```

Plusieurs profils d'installation d'Istio sont disponibles (voir le tableau ci-dessous).
Pour ce tutoriel, nous allons installer le profil "demo" car celui-ci permet de tester toutes les fonctionnalités d'Istio

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

Installer le profil "demo"
```
$ istioctl manifest apply --set profile=demo
```

Vérifier l'installation en vous assurant que les services Kubernetes suivants sont déployés et vérifiez qu'ils ont tous un CLUSTER-IP approprié, à l'exception du service `jaeger-agent`
```
$ kubectl get service -n istio-system
NAME                     TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)                                                                                                                      AGE
grafana                  ClusterIP      10.0.12.82    <none>          3000/TCP                                                                                                                     2m9s
istio-citadel            ClusterIP      10.0.12.52    <none>          8060/TCP,15014/TCP                                                                                                           2m11s
istio-egressgateway      ClusterIP      10.0.11.249   <none>          80/TCP,443/TCP,15443/TCP                                                                                                     2m10s
istio-galley             ClusterIP      10.0.15.206   <none>          443/TCP,15014/TCP,9901/TCP,15019/TCP                                                                                         2m11s
istio-ingressgateway     LoadBalancer   10.0.13.178   35.222.49.120   15020:31102/TCP,80:30629/TCP,443:31990/TCP,15029:31705/TCP,15030:32305/TCP,15031:32618/TCP,15032:32128/TCP,15443:31641/TCP   2m10s
istio-pilot              ClusterIP      10.0.3.93     <none>          15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                       2m11s
istio-policy             ClusterIP      10.0.0.9      <none>          9091/TCP,15004/TCP,15014/TCP                                                                                                 2m11s
istio-sidecar-injector   ClusterIP      10.0.13.92    <none>          443/TCP                                                                                                                      2m10s
istio-telemetry          ClusterIP      10.0.6.143    <none>          9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                       2m6s
jaeger-agent             ClusterIP      None          <none>          5775/UDP,6831/UDP,6832/UDP                                                                                                   2m17s
jaeger-collector         ClusterIP      10.0.9.110    <none>          14267/TCP,14268/TCP,14250/TCP                                                                                                2m17s
jaeger-query             ClusterIP      10.0.12.140   <none>          16686/TCP                                                                                                                    2m16s
kiali                    ClusterIP      10.0.8.83     <none>          20001/TCP                                                                                                                    2m11s
prometheus               ClusterIP      10.0.3.108    <none>          9090/TCP                                                                                                                     2m16s
tracing                  ClusterIP      10.0.1.253    <none>          80/TCP                                                                                                                       2m16s
zipkin                   ClusterIP      10.0.2.15     <none>          9411/TCP                                                                                                                     2m16s
```

Assurez-vous également que les modules Kubernetes correspondants sont déployés et ont un STATUS "Running"
```
$ kubectl get pods -n istio-system
NAME                                      READY   STATUS    RESTARTS   AGE
grafana-6c8f45499-wkfjc                   1/1     Running   0          5m31s
istio-citadel-db8578cc4-vn7xt             1/1     Running   0          5m33s
istio-egressgateway-f9c7b6669-pl4xl       1/1     Running   0          5m36s
istio-galley-786ff7f89b-wvqgk             1/1     Running   0          5m33s
istio-ingressgateway-5654f8bf65-fnv2g     1/1     Running   0          5m37s
istio-pilot-c565b478c-vm7gd               1/1     Running   0          5m34s
istio-policy-7d5f97776c-bbxpv             1/1     Running   2          5m33s
istio-sidecar-injector-85577d99c6-glzmd   1/1     Running   0          5m32s
istio-telemetry-55dd49cf85-mzwl8          1/1     Running   3          5m33s
istio-tracing-78548677bc-gcgfk            1/1     Running   0          5m38s
kiali-fb5f485fb-87qhs                     1/1     Running   0          5m32s
prometheus-685585888b-8dhsb               1/1     Running   0          5m37s
```

Lorsque vous déployez votre application à l'aide de `kubectl apply`, l'injecteur sidecar d'Istio injectera automatiquement les conteneurs Envoy dans les pods d'applications s'ils sont démarrés dans des namespace étiquetés avec istio-injection=enabled
```
$ kubectl label namespace default istio-injection=enabled
namespace/default labeled
```
```
$ kubectl get namespace -L istio-injection
NAME              STATUS   AGE    ISTIO-INJECTION
default           Active   127m   enabled
istio-system      Active   32m    disabled
kube-node-lease   Active   127m
kube-public       Active   127m
kube-system       Active   127m
```

Vous pouvez lancer un dashboard (Web UI) comme grafana pour tester son fonctionnement sur le cluster en cliquant sur le lien dans un navigateur
```
$ istioctl dashboard grafana
http://localhost:40939
```

### Installation d'une application

## 4) Les code source et scripts élabore. 

## 5) Diapos de votre présentation en PDF.
