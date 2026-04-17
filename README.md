# MicroCRM

Application full-stack de démonstration construite avec **Spring Boot 3** et **Angular 17**.

Le projet permet de gérer des **organisations** et des **contacts** dans une interface web simple. Il a également été enrichi avec une chaîne **CI/CD**, une analyse continue avec **SonarCloud** et une stack **ELK** pour l'observabilité.

## Stack technique

- **Backend** : Java 17, Spring Boot 3, Spring Data REST
- **Frontend** : Angular 17, TypeScript
- **Build & packaging** : Gradle, npm, Docker multi-stage
- **CI/CD** : GitHub Actions
- **Qualité** : SonarCloud, JaCoCo, Karma coverage
- **Observabilité** : Elasticsearch, Logstash, Kibana, Filebeat

## Structure du projet

- `back/` : backend Spring Boot
- `front/` : frontend Angular
- `docker-compose.yml` : lancement de l'application
- `docker-compose-elk.yml` : ajout de la stack ELK
- `elk/` : configuration Filebeat et Logstash
- `.github/workflows/` : pipelines GitHub Actions
- `Dockerfile` : build multi-stage du frontend, du backend et des images finales

## Prérequis

- Docker

> Aucune installation locale de Java ou Node.js n'est nécessaire pour lancer l'application avec Docker.

## Récupération du projet

```bash
git clone https://github.com/gordinir67/Partie7.git
```

## Démarrage rapide

### 1. Lancer l'application avec Docker

```bash
docker compose up --build
```

Applications disponibles :

- Frontend : `http://localhost`
- API : `http://localhost:8080`

### 2. Lancer l'application avec la stack ELK

```bash
docker compose -f docker-compose.yml -f docker-compose-elk.yml up --build
```

Services disponibles :

- Frontend : `http://localhost`
- API : `http://localhost:8080`
- Kibana : `http://localhost:5601`
- Elasticsearch : `http://localhost:9200`

## Build Docker

### Construire l'image backend

```bash
docker build --target back -t microcrm-back:latest .
```

### Construire l'image frontend

```bash
docker build --target front -t microcrm-front:latest .
```

## Tests en local

### Backend

```bash
./gradlew clean test jacocoTestReport
```

### Frontend

```bash
npm ci
npm run test:ci
```

## CI/CD

Le dépôt contient deux workflows GitHub Actions :

- **CI** : build, tests backend, build frontend, tests frontend et analyse SonarCloud
- **CD** : build et publication des images Docker sur **GHCR** après succès de la CI sur la branche `main`

## GHCR

Les images Docker sont publiées sur GitHub Container Registry :

- `ghcr.io/gordinir67/microcrm-back:latest`
- `ghcr.io/gordinir67/microcrm-front:latest`

Des images versionnées par **SHA de commit** sont également publiées par le pipeline CD.

Exemple de récupération :

```bash
docker pull ghcr.io/gordinir67/microcrm-back:latest
docker pull ghcr.io/gordinir67/microcrm-front:latest
```

## Observabilité

La stack ELK permet de centraliser et consulter les logs applicatifs.

Chaîne de traitement :

`conteneurs Docker -> Filebeat -> Logstash -> Elasticsearch -> Kibana`

- **Filebeat** collecte les logs des conteneurs Docker
- **Logstash** les traite et les restructure
- **Elasticsearch** les indexe
- **Kibana** permet leur visualisation

## Commandes utiles

```bash
# Lancer l'application
docker compose up --build

# Lancer l'application avec ELK
docker compose -f docker-compose.yml -f docker-compose-elk.yml up --build

# Arrêter les conteneurs
docker compose down

# Arrêter aussi la stack ELK
docker compose -f docker-compose.yml -f docker-compose-elk.yml down

# Backend : tests + couverture
./gradlew clean test jacocoTestReport

# Backend : build complet
./gradlew clean build --no-daemon

# Frontend : installation + tests CI + couverture
npm ci && npm run test:ci

# Frontend : build Angular
npm ci && npm run build

# Build image backend
docker build --target back -t microcrm-back:latest .

# Build image frontend
docker build --target front -t microcrm-front:latest .
```

## Observabilité et métriques

Le projet s'appuie sur plusieurs sources pour le suivi qualité et l'observabilité :

- **GitHub Actions** : durée des pipelines, taux de succès et fréquence des exécutions
- **SonarCloud** : couverture, duplications, quality gate, code smells et complexité
- **Kibana** : consultation des logs applicatifs centralisés
- **GHCR** : traçabilité des images publiées

## Restauration / rollback

La restauration peut s'appuyer sur :

- un **commit Git stable**
- une **image GHCR versionnée par SHA**

Exemple avec une image versionnée :

```bash
docker pull ghcr.io/gordinir67/microcrm-back:<sha>
docker pull ghcr.io/gordinir67/microcrm-front:<sha>
```
