# Docker - GoAccess + Nginx (Reverse Proxy + Dashboard)

## Description du Projet

Ce projet implémente une solution de monitoring de logs Nginx en utilisant GoAccess, déployée via Docker Compose. L'architecture se compose d'un reverse proxy Nginx et d'un serveur GoAccess, permettant l'analyse en temps réel des logs d'accès.

## Architecture

- **Reverse Proxy (Nginx)**

  - Conteneur basé sur nginx:alpine
  - Point d'entrée pour le trafic
  - Gestion des logs d'accès

- **GoAccess**
  - Conteneur basé sur Debian Bullseye
  - Interface web pour l'analyse des logs
  - Génération de rapports en temps réel
  - Serveur Apache2 pour héberger l'interface

```
docker-goaccess/
├── docker-compose.yml
├── reverse-proxy/
│   ├── Dockerfile
│   └── nginx.conf
├── goaccess/
│   ├── Dockerfile
│   ├── apache.conf
│   └── crontab
├── logs/
└── .env
```

## Fonctionnalités Principales

- Analyse en temps réel des logs Nginx
- Interface web sécurisée avec authentification basic
- Sécurisation via `.htaccess` (Apache) et génération dynamique de `.htpasswd`
- Génération automatique des rapports via crontab
- Healthchecks pour les deux conteneurs
- Gestion optimisée des ressources système
- Volume partagé pour la persistance des logs

## Prérequis

- Docker
- Docker Compose
- Variables d'environnement configurées pour l'authentification:
  - GOACCESS_USER
  - GOACCESS_PASSWORD

## Installation et Déploiement

1. Cloner le repo :
   
```bash
git clone https://github.com/y2-znt/docker-goaccess.git
cd docker-goaccess
```
   
2. Créer un fichier .env avec les credentials :

```bash
GOACCESS_USER=votre_utilisateur
GOACCESS_PASSWORD=votre_mot_de_passe
```

3. Lancer les conteneurs :

```bash
docker compose up --build
```

## Accès aux Services

- Reverse Proxy : http://localhost:8080 - Interface GoAccess : http://localhost:8081
  - Authentification requise avec les credentials définis

## Spécifications Techniques

### Limitations des Ressources

- Mémoire vive : 512Mo par conteneur
- Mémoire maximale : 1Go par conteneur
- Swap désactivé (mem_swappiness: 0)

### Monitoring

- Healthchecks toutes les 30 secondes
- Redémarrage automatique des conteneurs
- Génération des rapports GoAccess toutes les 5 minutes

### Sécurité

- Authentification Basic Auth pour l'accès à GoAccess
- Isolation réseau via un réseau Docker dédié
- Ports non standards utilisés (8080, 50001)

## Points Techniques Notables

- Utilisation exclusive d'images officielles Docker
- Configuration optimisée des conteneurs
- Gestion des logs centralisée
- Architecture modulaire et scalable
- Sécurisation des accès

## Maintenance

Les logs sont conservés dans un volume Docker nommé `nginx_logs` pour assurer la persistance des données. Les rapports sont générés automatiquement et mis à jour régulièrement via cron.
