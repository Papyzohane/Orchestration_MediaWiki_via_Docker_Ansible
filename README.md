🏗️ Infrastructure as Code : Déploiement de MediaWiki via Docker et Ansible

🛠️ Une Architecture Fondée sur la Conteneurisation

Le cœur de ce projet repose sur une isolation stricte des services grâce à la technologie Docker. Plutôt que de déployer l'ensemble de la pile applicative sur une machine unique, nous avons fait le choix d'une architecture modulaire où chaque composant évolue dans son propre conteneur. Ce réseau de conteneurs, orchestré sous Ubuntu 24.04, est interconnecté par un réseau bridge dédié, ce qui garantit une communication fluide tout en restant hermétique aux accès extérieurs non autorisés.

Pour assurer une gestion efficace, l'infrastructure est segmentée comme suit :

    Node Manager : Le conteneur maître qui pilote les déploiements et centralise la logique Ansible.

    Serveur Web (http1) : Une instance dédiée à Apache 2 et PHP pour l'affichage du Wiki.

    Base de Données (bdd1) : Un conteneur MariaDB isolé pour la persistance des données.

    Portainer : Une interface graphique permettant de monitorer visuellement le cycle de vie de chaque nœud.

🔧 L'Automatisation au Service de la Reproductibilité

Le déploiement et la configuration de ces conteneurs ne sont pas réalisés manuellement, mais pilotés intégralement par Ansible. Grâce à l'utilisation de rôles structurés, nous automatisons l'installation de la pile logicielle complète. Cette approche par Infrastructure as Code permet de reconstruire l'intégralité du wiki en quelques minutes, garantissant que l'environnement de production est identique à celui de développement.

La sécurité est intégrée par défaut dans ce processus d'automatisation :

    Flux SSH : Sécurisation des accès par clés ECDSA injectées automatiquement.

    Ansible Vault : Chiffrement des secrets (mots de passe SQL, clés privées) via l'algorithme AES256.

    Idempotence : Utilisation de handlers pour ne redémarrer les services qu'en cas de modification réelle de la configuration.

🚀 Intégration Applicative et Personnalisation

Une fois l'infrastructure réseau opérationnelle, le projet assure la finalisation de MediaWiki en injectant dynamiquement les fichiers de configuration et en gérant les permissions du système de fichiers. L'aspect novateur de cette solution réside également dans sa capacité d'extension via le développement d'un module Python personnalisé nommé count_page. Ce module s'intègre directement aux playbooks pour extraire des statistiques précises depuis la base de données, prouvant que l'infrastructure peut être aussi intelligente que robuste.

Pour lancer le déploiement, il suffit d'exécuter les playbooks dans l'ordre suivant après avoir activé votre environnement virtuel :

    install-apache.yml pour préparer le serveur web.

    install-mariadb.yml pour le serveur de données.

    install-mediawiki.yml pour lier les services et finaliser l'application.
