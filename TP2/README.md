# Étape 1
## 1. Quel est le rôle d’un réseau privé dans une infrastructure Cloud? D’un routeur externe?
- Un réseau privé isole les ressources d'un client de celui des autres clients. Plusieurs machines virtuelles peuvent être sur un même réseau privé, sans que quelqu'un à l'extérieur puisse savoir les détails du réseau à l'interne.
- Un routeur externe relie le réseau privé au réseau public. Il assure le routage entre les deux.
## 2. Pourquoi un fournisseur cloud propose-t-il autant de catégories de ressources différentes?
Pour que la configuration proposée soit en accord avec les besoins et le budget des clients.
## 3. En observant la topologie générée par OpenStack, expliquez le chemin emprunté par un paquet réseau envoyé depuis l’extérieur vers une machine virtuelle sur net-discovery.
Paquet arrive par le réseau externe `vpn-externl` grâce à l'IP flottante publique associée à la VM, qui n'a pas encore été configurée -> routeur `routerdefault` -> VM sur réseau privé `net-discovery`

# Étape 2
## 1. Quelles sont les étapes nécessaires pour pouvoir se connecter en SSH à une nouvelle machine virtuelle OpenStack depuis la machine hôte?
1. Créer un réseau privé relié au routeur externe, un groupe de sécurité qui autorise SSH (sur le port 22), lancer la VM avec une paire de clés.
2. Associer une ip flottante à la VM.
3. Se connecter depuis l'hôte : `ssh -i <clé.pem> ubuntu@<IP_FLOTTANTE>`
## 2. Quel est le rôle de l’adresse IP privée, de l’adresse IP flottante et des Groupes de Sécurité dans cette connexion?
- IP privée : adresse de la VM dans `net-discovery` pour la communication interne
- IP flottante : adresse publique associée à la VM, le point d'entrée depuis l'extérieur.
- Groups de sécurité : pare-fu virtuel qui n'autorise que le trafic permis.
## 3. Pourquoi OpenStack distingue-t-il une adresse IP privée d’une IP flottante publique, plutôt que d’attribuer directement une adresse publique à chaque machine virtuelle?
Parce que pas toutes les VM ont besoin d'être accessibles depuis l'extérieur. Une adresse flottante peut être réattribuée à une autre VM sans changer l'adresse publique d'un service.

# Étape 3
## 1. Pourquoi les Security Groups sont-ils indispensables même lorsqu’une machine virtuelle possède une adresse IP publique?
Même si la VM possède un IP public, ça ne veut pas dire qu'on veut autoriser toutes les connexions. Les SG permettent de contrôler les ports et les types de trafic autorisés.
## 2. Décrivez le chemin parcouru par une requête HTTP envoyée depuis votre ordinateur jusqu’à l’application hello-api exécutée sur votre machine virtuelle.
Notre ordi -> internet -> IP flottante `194.199.106.34` -> `routeurdefault` -> `net-discovery` `192.168.10.0/24` -> filtrage par `sg-tp2` (TCP 5000 autorisé) -> VM `192.168.10.232` -> hello-api sur le port 5000

# Étape 4

## 1. Quelles différences existe-t-il entre une image officielle et un snapshot?
Une image officielle est vierge et générique (il a fallu git clone hello-api), alors qu'un snapshot copie l'état d'une VM (hello-api était déjà dessus)
## 2. Quels avantages apporte le redimensionnement d’une machine virtuelle dans un environnement cloud?
Adapter CPU/RAM aux besoins et ne payer que les ressources nécessaires.
## 3. Dans quels contextes un administrateur système préférera-t-il créer une nouvelle machine à partir d’un snapshot plutôt que de repartir d’une image vierge?
Un admin voudra créer une nouvelle VM à partir d'un snapshot pour cloner rapidement un environnement déjà configuré.