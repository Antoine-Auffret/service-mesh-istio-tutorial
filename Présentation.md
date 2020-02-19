<h1>Architecture des applications</h1>
<h1>Présentation de la solution ISTIO</h1>
<div style="text-align: justify">
<ol>
<li>I.	Contexte et Limites actuelles</li>
  <p>Aujourd’hui avec l’expansion du cloud, de plus en plus d’entreprises se tournent vers des architecture microservices, abandonnant le développement d’application centralisées. Que cela soit pour faire migrer une architecture monolithique, ou développer une nouvelle application, les réseaux de microservices attirent de plus en plus. Toutefois, malgré leur popularité, ces architectures peuvent être complexes à mettre en place et ne sont pas adaptés à toutes les entreprises.</p>
  
  <p>Une architecture microservices, cherche à séparer les différentes fonctionnalités d’une application généralement d’envergure, afin de mieux contrôler ses différents aspects. Prenons pour exemple une application de vente en ligne : Afin qu’un consommateur achète un article, l’application doit vérifier que celui-ci est en stock. Le service qui gère la base de données doit donc communiquer à la page web l’information. Celle-ci doit par la suite, envoyer l’information au service qui gère le panier de l’utilisateur. L’application peut aussi intégrer un service de recommandation, qui lui aussi devra communiquer avec la base de données. Nous comprenons par cet exemple, que les applications d’envergure, fonctionnent toutes aujourd’hui avec des architectures microservices. Leur fonctionnement serait impossible dans un code regroupé.</p>
  
  <p>Les différents services de ce type d’architectures sont regroupés en instances de conteneurs, et gérer par des réseaux que l’on nomme des Orchestrateurs de conteneurs. Pour rappel un conteneur est une zone d’une machine, allouée à un service. Plusieurs conteneurs occupe différents espaces sur une machine qu’on appellera un pod. Un pod, se différencie des machines virtuelles par la faculté de scinder et de partager son OS entre différents services. Un parc de pod est géré par un Orchestrateur de conteneur, comme Kubernetes par exemple. Ces derniers permettent notamment d’adapter les ressources à savoir les conteneurs, en fonction de la demande d’un service sur le réseau. Kubernetes, automatise donc l’instanciation de nouveaux conteneurs et la gestion du réseau du point de vue des conteneurs de services.</p>
  
  <p>Une architecture microservices possède deux grands avantages :  La gestion est meilleure, en cas de panne ce n’est pas l’ensemble de l’application qui tombe mais seulement le service et ceux qui y sont associés. Et les différents services sont généralement l’œuvre d’équipes métiers différentes qui peuvent donc plus facilement développer de manière autonome leur fonctionnalité.</p>
  
  <p>Mais ce type d’architecture connait aussi ces limites : L’ensemble de ces services est généralement développé de manière autonomes, et l’ensemble des règles de communications avec les autres services qui y sont liés sont fixes et ont été développées dans le code de chaque fonctionnalité. Cela rend extrêmement rigide l’évolution de manière générale, entrainant des erreurs, des bugs et des ralentissements. En effet les règles développées un an auparavant ne sont plus d’actualité aujourd’hui, et force les équipes à modifier le code. De plus, au fil du temps des nouveaux services sont ajoutés, avec de nouvelles règles, et cet ajout peut totalement déséquilibrer la charge sur certaines fonctionnalités. Cela créer des goulots d’étranglement ou les données sont traitées de manière inefficace ralentissant la totalité du système. Pire encore, dans des applications d’une taille colossale ou des services sont régulièrement ajoutés, comment trouver la source d’un problème ? Cela révèle de l’impossible ou de nombreux services appartenant à des corps de métiers différents communiques entre eux. Il est important de noter que si les Orchestrateurs de conteneur comme Kubernetes, gères de manière automatique l’architecture de conteneur, ces derniers ne donnent en aucun cas des informations permettant une vision globale du réseau.</p>
 
  <p>Les architectures microservices peuvent nous le voyons, devenir difficilement gérable sans les bons outils et les bonnes méthodes. Partant en plus du principe que ces applications sont généralement très utilisées et donc très importantes, des ralentissements ou des crashs ferait perdre des sommes considérables d’argents aux compagnies propriétaires. C’est dans ce contexte et face à ce défi qu’entre en jeu le concept de maillage de service.</p>  
  
  
<li>II.	Présentation des maillages de services</li>

  <p>La technologie des service mesh ou maillages de services, est apparue ces dernières années pour répondre aux problèmes naissant des architectures de microservices de plus en plus volumineuses. Cette technologie, cherche à améliorer la gestion de ces imposants réseaux en en contrôlant la transmission des données. Ce contrôle va chercher à être effectué entre les différents services, afin d’optimiser leurs échanges en enlevant cette responsabilité de traitement des données présent dans le code de chaque fonctionnalité, pour la déplacer. Les services mesh, ont pour objectif d’être aisément mis en place sans avoir besoin de modifier le code des services du réseau, ou très peu. Nous l’aurons compris, un maillage de service est une méthode de gestion d’infrastructure plus vaste utilisant les microservices, et dont le but est d’en réduire la complexité en apportant une vision d’ensemble afin de gérer au mieux l’architecture.</p>
  
  <p>Pour approfondir son fonctionnement, un maillage de service est l’ajout d’une couche applicative dédié au traitement au transport et à la gestion en règle générale des données entre les différents services souvent dépendants les uns des autres. L’ajout d’une couche dédié permet comme expliqué plus haut, de déplacer cette responsabilité de traitement propre à chaque service, afin de pouvoir plus facilement modifier les règles de transmission de données.</p>
  
  <p>L’ajout de cette couche dédiée se fait par l’ajout de proxy communément appelé « sidecar ». Ce diminutif, se rapporte au fait que pour chaque service présent, un proxy de type sidecar y sera rattaché.</p>
  
  
  
  <p>Un proxy « sidecar », fonctionne à côté d’un service et non dans celui-ci. Ce type de proxy est la base des maillages de services. En effet, chaque proxy va contrôler l’ensemble des données qui transitent entre leur service et le reste du réseau. Permettant d’y ajouter un ensemble de règles assouplissant drastiquement leur évolution et modification.</p>
  
  <p>L’ajout de ces proxys, permet de récolter des données sur l’ensemble du réseau qui sont ensuite envoyées et traitées afin d’avoir une vision d’ensemble sur le fonctionnement de l’architecture. Cette vision, est rendue possible par différentes fonctionnalités propres au service mesh, aboutissant à des graphiques, et un ensemble de statistiques variés sur l’état du réseau et des échanges. Il devient alors simple, de voir, le temps de transition des données, le temps de traitement, les erreurs potentiels et remonter plus facilement aux différentes sources de problèmes.</p>
  
  <p>Avec l’évolution constante d’architectures conséquentes, la vision globale détaillée et statistique qu’apporte un maillage de service, deviens indispensable. Autre le fait d’éviter les crash, bugs et ralentissements important, cette vision permet notamment d’améliorer la qualité du système, en permettant l’optimisation du réseau, assurant l’efficacité et la fiabilité en continu.</p>
  
  <p>Maintenant que nous avons décrit en généralisant le rôle d’un maillage de service, nous allons voir les apports et le fonctionnement des maillages de services dont notamment :</p>
  
* La découverte de service
* L’équilibrage
* Le chiffrement
* L’observabilité
* La traçabilité
* L’authentification
* La prise en charge du modèle de disjoncteur
* La mise en place de tests
* Les déploiement canaris
* La limitation du débit

<li>III.	Fonctionnement et approfondissement</li>

* La découverte de service
<p>Lorsqu’une instance de service souhaite interagir avec l’instance d’un autre service, celle-ci doit chercher et « découvrir » sur le réseau, une instance disponible et fonctionnelle répondant à son besoin. Pour ce faire, l’instance effectue généralement une recherche DNS à l’infrastructure d’orchestration, qui conserve une liste des services disponibles en fonction de leur type. Cette architecture d’orchestration est par exemple Kubernetes.</p>

* L'équilibrage
<p>L’équilibrage de charge est un ajout qu’apporte une la majorité des Framework d’orchestrations de conteneur, tel Kubernetes au niveau de la couche réseau de transport (4). Les maillages de services quant à eux, affine cette idée de loadBalancer au niveau de la couche d’application (7), avec de meilleurs algorithmes et une meilleure gestion des trafics à fort volume de données.</p>

* Le chiffrement
<p>Le maillage de service assure le chiffrement des données entre les services, permettant de soulager ses derniers de cette charge. Un maillage de service va également chercher à optimiser les échanges en utilisant en priorité les connexions déjà existantes. L’implémentation la plus rependue est l’utilisation d’infrastructure PKI.</p>

* L'observabilité
<p>L’observabilité correspond à la capacité de voir en temps réel le trafic entre les différentes instances de manière détaillée. C’est l’un des principaux apports d’un service mesh, permettant une vision globale de l’état du réseau. Cette observabilité est rendue possible par l’utilisation de proxy sidecars qui filtre toutes données entrante ou sortante d’un service. Ces données sont de différents types :
  -	Des métriques correspondantes à la latence, aux erreurs et à la saturation du réseau.
  -	Des logs d’accès, relatifs à toutes le demandes entre les services. (Sources/destination) Cela apporte de la précision sur les communications en cas d’erreur ou de latence par exemple.
  -	Des traces distribuées ou « Distributed Tracing », qui collecte différentes informations pour chaque couche de communications traversé par les requêtes entre services.
</p>

* L'authentification
<p>Les maillages de service permettent une authentification hors service, n’autorisant ainsi que l’envoi de requêtes valides, faisant ainsi gagner du temps de traitement aux différentes instances.</p>


<li>IV.	La solution ISTIO</li>

  <p>Istio est la solution de maillage de service la plus réputée actuellement. Elle a été développée avec le soutien d’IBM, de google cloud de Lyft et de la communauté open-source dont Read Hat, et a vu le jour en 2017. Parmi les nombreux avantages qu’apporte l’utilisation d’un service mesh, Istio recherche majoritairement à en promulguer quatre principaux.</p>
  
  <p>À savoir : La gestion du trafic, l’observabilité, la sécurité et la mise en place de politiques en matière de gestion de l’information.</p>
  
  <p>De plus, l’un des principaux avantages de la solution, est la faculté de mettre en place le maillage de service, sans avoir (ou très peu mais rarement) à modifier le code des services de l’architecture. Cela permet un déploiement simple et rapide, malgré la difficulté et la taille des architectures concernées. </p>
  
  <p>Lors de l’implémentation de la solution Istio, l’architecture se divise en deux grandes parties.</p>
  

  
  ## Le plan de données
  <p>La première correspond au plan de données qui va être composé de l’ensemble des proxy sidecars, rattachés aux instances de services. On appelle également Mixer, le composant chargé de vérifier que les politiques sont respectées sur l’ensemble des communications, et de collecter des données sur la télémétrie. Le mixer est une couche supplémentaire de vérification des politiques, et de collecte de données par lequel chaque proxy communique avec la seconde partie de l’’architecture, le plan de contrôle des données.</p>
  
  <p>En ce qui concerne, l’application de et la vérification des politiques, chaque proxy possède une mise en cache locale, ce qui lui permet d’effectuer de manière autonome une grande partie des vérifications sur les données qu’il communique. Le mixer, permet quant à lui, permet d’isoler le reste de l’architecture Istio des différentes implémentations individuelles et des infrastructures de backend.</p>
  
  <p>L’ensemble des proxy sidecars, sont issus de la solution « Envoy » qui est une nécessité au fonctionnement d’Istio. Conçu chez Lyft, Envoy est un proxy distribué C++ haute performance conçu pour gérer un service unique.</p>
  
  <p>Les proxy Envoy permettent :</p>
* Une autonomie et un fonctionnement optimal qu’importe le type et le langage du service.
* Une prise en charge des communication http/2 et gRPC pour les connexions entrantes et sortantes.
* Un système de loadbalancer avancé intégré, prenant en charge, les tentatives de connexion automatiques, la limitation du débit globale et l’équilibrage de zones.
* L’utilisation d’API robustes pour gérer de manière dynamique la configuration du proxy.
* Une observabilité approfondie et une vision claire des échanges qui transitent.
* Le concept d’injection automatique
* La découverte de services
* Le système de disjoncteur

  
  ## Le plan de contrôle
  <p>Le plan de contrôle correspond à l’ensemble des différents composants et technologies, permettant la configuration des règles, la gestion et le contrôle de l’architecture, ainsi que l’apport d’une vision globale du trafic. C’est dans ce plan que la politique est définie et relayée ensuite aux proxys de la couche de données.</p>
  
  <p>Pilot est la solution qui gère la couche de contrôle permettant aux différents proxy Sidecar d’effectuer la recherche et la découverte de nouveaux services. Pilot, permet d’effectuer un routage intelligent en définissant des règles spécifiques que pourra traiter la solution Envoy, de chaque proxy. La solution Pilot s’occupe donc d’optimiser le routage entre les proxys.</p>
  
  <p><Citadel, est la solution de sécurité d’Istio, qui intègre l’authentification forte entre les différents services en traitant automatiquement des identités et des informations d’authentifications. Citadel permet notamment la sécurisation des échanges par la mise en place de configuration adoptant différents chiffrements./p>
  
  <p>Galley, est le composant de validation, de traitement et de distribution des configurations d’Istio. Son objectif est d’isoler le reste des composants Istio de la plateforme de configuration sous-jacente, exemple Kubernetes.</p>
  
  <p>Istio est une solution qui se base sur une application d’orchestration de conteneur, dont les dernières versions supportent Kubernetes qui est la plus connue et Nomad fonctionnant avec Consul. (Pour prendre l’exemple de Kubernetes, une application d’orchestration de conteneur, permet de gérer un réseau ou un parc de multiples services. Son rôle est de d’automatiser les déploiements de nouvelles instances via les conteneurs, mais aussi la mise à l’échelle en fonctions des besoins du réseau, et la gestion de applications et service des manières générales par le biais des conteneurs. Une solution comme Kubernetes est donc obligatoire au fonctionnement d’Istio, et permet d’adapter les ressources du serveur correspondants au conteneur de services, en fonction des besoins de l’architecture. L’un des principaux objectifs d’Istio reste toutefois de fonctionner qu’importe l’application d’orchestration de conteneur, et malgré le fait que Kubernetes soit aujourd’hui la plus connues, elle n’est pas une obligation, à l’inverse de la solution Envoy qui gère les proxy sidecars.</p>
  
</ol>
</div>
