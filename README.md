# MicroCRM

Application full-stack de démonstration construite avec **Spring Boot 3** et **Angular 17**.

Le projet permet de gérer des **organisations** et des **contacts** dans une interface web simple. Il a aussi été enrichi avec une chaîne **CI/CD**, une analyse **SonarCloud** et une stack **ELK** pour l'observabilité.

## Stack technique

- **Backend** : Java 17, Spring Boot 3, Spring Data REST
- **Frontend** : Angular 17, TypeScript
- **Build & packaging** : Gradle, npm, Docker multi-stage
- **CI/CD** : GitHub Actions
- **Qualité** : SonarCloud, JaCoCo, Karma coverage
- **Logs & monitoring** : Elasticsearch, Logstash, Kibana, Filebeat

```

## Prérequis

- Docker
- Docker Compose

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

### Image backend

```bash
docker build --target standalone -t microcrm-standalone:latest .
```

## CI/CD

Le dépôt contient deux workflows GitHub Actions :

- **CI** : build, tests backend, tests frontend, analyse SonarCloud
- **CD** : build et publication des images Docker sur **GHCR** après succès de la CI sur `main`

## GHCR


Exemple de récupération :

```bash
docker pull ghcr.io/<owner>/microcrm-back:latest
docker pull ghcr.io/<owner>/microcrm-front:latest
```

## Commandes utiles

```bash
# Lancer l'application

docker compose up --build

# Lancer avec ELK

docker compose -f docker-compose.yml -f docker-compose-elk.yml up --build

# Arrêter les conteneurs

docker compose down

# Backend : tests + couverture

cd back && ./gradlew clean test jacocoTestReport

# Frontend : tests CI + couverture

cd front && npm ci && npm run test:ci

# Build image backend

docker build --target back -t microcrm-back:latest .

# Build image frontend

docker build --target front -t microcrm-front:latest .
```

## Observabilité et métriques

Le projet s'appuie sur plusieurs sources pour le suivi qualité et performance :

- **GitHub Actions** : durée des pipelines, taux de succès/échec, fréquence des runs
- **SonarCloud** : coverage, duplications, quality gate, code smells
- **Kibana** : consultation des logs applicatifs centralisés
- **GHCR** : traçabilité des images publiées

## Restauration / rollback

La restauration repose  sur :

- un **commit Git stable**
- ou une **image GHCR versionnée par SHA**

