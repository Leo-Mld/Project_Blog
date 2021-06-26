#GOLANG

Vous devez développer un plugin Telegraf qui enregistre des valeurs données par powertop (https://wiki.archlinux.org/index.php/powertop) sur l’infrastructure docker hébergeant le blog (cf partie Docker) dans une DB InfluxDB. Afin d’y suivre les graphes de consommation électrique des machines.(https://www.influxdata.com/training/write-your-own-telegraf-plugin/)

Votre projet Golang sera testé et déployé en image Docker via GITLAB CI

#DOCKER

En plus du projet Golang, il vous sera demandés d’installer une infrastructure Web hébergeant un Blog (libre choix de la solution, mais obligation d’une BDD déportée). Tous les services devront être containérisés dans leur propre environnement en respectant :

•	Un LoadBalancing est obligatoire 
•	Vous devez créer vos propres images Docker via dockerfile (toutes récupérations d’image existante est éliminatoire) 
•	Les images seront rajoutées à un DOCKER REGISTRY PRIVÉE. 
•	Le Docker registry devra également gérer l’authentification et les contrôles d’accès des utilisateurs via un « authentification server ». Vous aurez le choix de référencer vos utilisateurs via des ACL, ou MongoDB, ou LDAP, ou un fichier YAML (partie supplémentaire pour les quadrinômes)
Votre projet Blog sera testé et déployé en image Docker via GITLAB CI

#CI/CD

Vous utiliserez Gitlab-CI pour automatiser toutes les taches de déploiement mais également les tests du bon fonctionnement des applications.

Nous vous proposons un découpage en trois stages : 

•	Build, 
•	Test 
•	Deploy.
La partie « Test » vérifiera que vos projets GO et BLOG sont fonctionnels. 

Le déploiement se fera sur 3 containers en load Balancing avec l’outils de votre choix. (Pour les quadrinômes, il se fera sur 3 containers en load Balancing et sur 3 autres containers en Failover )



#LE LIVRABLE

Pour permettre de consulter les sources et le fonctionnement de votre projet, vous aurez à fournir
•	Une machine virtuelle comprenant votre usine logicielle.
•	Un rendu type Blog ou Vlog (dans votre infrastructure Web hébergeant un Blog) expliquant comment utiliser le projet et comment le tester. N’oubliez pas de présenter des captures d’écran (ou des captures vidéo) pour faciliter les explications et de faire preuve de vulgarisation.
•	Un répertoire qui contient les sources de votre développement Golang
La date limite de livraison est le 29 Avril 2021





#Projet pro en quadrinôme : 

Le projet BLOG a été réalisé avec les membres de GlobalNet comprenant TORDJMAN Joël (Partie Golang), BISSIERES Nicolas (partie Docker), MOLLARD Léo (partie Gitlab) et WINTZ Enzo (partie Docker).

PREREQUIS : 

Merci de rajouter les éléments suivants dans le fichier host de la VM si ce n'est pas déjà le cas : 

---
172.25.0.3      www.wordpressblogavril.org
172.25.0.6      www.wordpressblogavril.org
172.25.0.7      www.wordpressblogavril.org
172.25.0.11     admin_blog.org
---

Dans les différents répertoire, il est important de relever que pour utiliser Gitlab,

Il sera nécessaire de se référencer au pdf tuto.pdf qui se trouve dans le dossiers Docs :

Au sein de ce dossier Docs se trouvera un résumer des différentes parties et des différentes technologies utilisées.

On retrouvera dans le répertoire principale (à la racine) tout les éléments permettant les actions suivantes : 

- Création, configuration et déploiement de l'infra avec Gitlab (voire gitlab-ci.yml)

- Création, configuration du registre servant à accueillir nos images (voire repo.yml) et dossier registry 
et les deux scripts en son sein

- Mise en place des différentes tasks permettant le déploiement maitrisé de chaque éléments au sein d'une 
CI-CD fait par Gitlab

- Création de nos images des serveurs web (voire dossier apache), des serveurs de bases de données (voire 
dossier mysql qui contiendra également la configuration de hyperdb permettant la haute disponibilité de 
la base de données de notre Wordpress).

- Mise en place du Load Balancing (voire dossier Traeffik)

- Mise en place de notre container contenant des variables sécurisées (voire dossier vault_conf)

- Mise en place d'une base de données InfluxDB ainsi que la mise en place de Telegraf afin d'avoir un 
aperçu de la consommation éléctrique de la VM (voire dossier golang)

- Possibilité de purger l'infrastructure via Gitlab avec le 
déploiement de la task purge-env.yml se trouvant dans le dossier ci-playbook

Arborescence du projet : 

.
├── apache
│   ├── host1
│   │   ├── docker-entrypoint.sh
│   │   ├── Dockerfile
│   │   ├── hyperdb.sh
│   │   └── wordpress.conf
│   ├── host2
│   │   ├── docker-entrypoint.sh
│   │   ├── Dockerfile
│   │   └── wordpress.conf
│   ├── host3
│   │   ├── docker-entrypoint.sh
│   │   ├── Dockerfile
│   │   └── wordpress.conf
│   ├── wordpress
│   └── wordpress_conf
│       ├── docker.txt
│       ├── gitlab.txt
│       ├── go.txt
│       └── info.txt
├── build_repo
│   ├── build_before_push.yml
│   └── purge_tmp_container.sh
├── ci-playbook
│   ├── build-registry.yml
│   ├── build-traefik.yml
│   ├── certificat-ssl.yml
│   ├── check-power.yml
│   ├── deploy-apache.yml
│   ├── deploy-bdd.yml
│   ├── deploy-failover.yml
│   ├── deploy-images.yml
│   ├── deploy-influxdb.yml
│   ├── deploy-mysql.yml
│   ├── deploy-traefik.yml
│   ├── deploy-vault.yml
│   ├── purge-containers.yml
│   ├── purge-env.yml
│   ├── push-images.yml
│   └── workspace.yml
├── copy_rootkey.sh
├── docker-compose.yml
├── drois.sh
├── golang
│   ├── build.sh
│   ├── influx
│   │   ├── build.sh
│   │   ├── config.sh
│   │   ├── Dockerfile
│   │   ├── influx
│   │   │   ├── config.sh
│   │   │   └── Dockerfile
│   │   ├── influx.yml
│   │   ├── powertop.csv
│   │   └── powertop.go
│   ├── influx.yml
│   ├── powertop.csv
│   └── powertop.go
├── mysql
│   ├── config_DB.sh
│   ├── Dockerfile
│   ├── mysql_host1
│   │   ├── config_DB.sh
│   │   └── Dockerfile
│   ├── mysql_host2
│   │   ├── adduser.sh
│   │   └── Dockerfile
│   └── mysql_host3
│       ├── adduser.sh
│       └── Dockerfile
├── README.txt
├── registry
│   ├── confcert.sh
│   └── config_repo.sh
├── repo.yml
├── runner.sh
├── traefik
│   ├── conf
│   │   ├── traefik_dynamic.toml
│   │   └── traefik.toml
│   ├── password
│   │   └── password.txt
│   └── traefik.yml
├── tuto.pdf
├── tuto.sh
└── vault_conf
    ├── scripts_vars
    │   ├── ADD_vault_vars_apache.sh
    │   ├── ADD_vault_vars_mysql2.sh
    │   └── ADD_vault_vars_mysql.sh
    ├── vault
    │   ├── config
    │   │   ├── vault-config.json
    │   │   └── VAULT_conf.sh
    │   └── Dockerfile
    └── vault-compose.yml
