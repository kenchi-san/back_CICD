# Document CI/CD complet pour BobApp (Backend & Frontend)
## Back

[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=kenchi-san_back_CICD&metric=alert_status)](https://sonarcloud.io/dashboard?id=kenchi-san_back_CICD)

## Front 
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=kenchi-san_front_CICD&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=kenchi-san_front_CICD)

## 1. Présentation globale du workflow CI/CD

Le workflow CI/CD est structuré autour de deux pipelines distincts mais complémentaires, respectivement pour le backend Java Spring Boot et le frontend Angular.

- **Backend** : Build Maven, tests unitaires, analyse qualité SonarQube, puis build & push image Docker.
- **Frontend** : Installation Node, tests unitaires Angular avec couverture, analyse qualité SonarCloud, puis build & push image Docker.

Ces pipelines s’exécutent automatiquement à chaque push sur la branche main ou sur les Pull Requests, garantissant la qualité et la stabilité du code déployé.

## 2. Description détaillée des étapes du workflow

### Backend (Java Spring Boot)

- **Checkout** : Clonage complet du repository pour garantir la pertinence de l’analyse SonarQube.
- **Setup JDK 17** : Environnement Java nécessaire pour la compilation et les tests.
- **Cache Maven & Sonar** : Optimisation du temps de build en stockant les dépendances et paquets.
- **Build & Tests Maven** : Compilation et exécution des tests unitaires avec génération du rapport de couverture JaCoCo.
- **Analyse SonarQube** : Exécution de l’analyse qualité du code, vérification des bugs, vulnérabilités, duplications, et couverture.
- **Docker Build & Push** : En cas de succès, construction de l’image Docker backend et déploiement sur Docker Hub.

### Frontend (Angular)

- **Checkout** : Récupération complète du code source.
- **Setup Node.js 20** : Installation de Node.js pour la gestion des dépendances et tests.
- **Installation npm & Tests** : Installation des packages et exécution des tests unitaires Angular en mode ChromeHeadless avec couverture.
- **Affichage du rapport coverage** : Vérification que le rapport de couverture est généré correctement.
- **Analyse SonarCloud** : Analyse qualité incluant couverture, bugs, duplications, et sécurité.
- **Docker Build & Push** : En cas de succès, build de l’image Docker frontend et push vers Docker Hub.

## 3. KPIs du client

| KPI                     | Backend                         | Frontend                      |
|-------------------------|--------------------------------|-------------------------------|
| Couverture minimale de code | ≥ 30% (actuel 38.8%)           | ≥ 30% (actuel 0%, à améliorer) |
| Nombre d’issues critiques   | 0                              | 0                             |
| Duplication                | ≤ 3%                           | ≤ 3%                          |
| Sécurité (Hotspots)         | ≤ 0 (ou strictement contrôlé)  | ≤ 0 (ou strictement contrôlé) |

Ces KPIs doivent être contrôlés à chaque exécution du pipeline. Les seuils empêchent la mise en production de code dégradant la qualité.

## 4. Analyse des métriques et retours utilisateurs

### Backend

- **Couverture** : 38.8% (sur 45 lignes à couvrir)
- **Issues** : 11 au total (1 fiabilité, 8 maintenabilité, 2 hotspots sécurité)
- **Duplications** : 0% (excellent)
- **Qualité** : Quality Gate passé, mais amélioration possible sur fiabilité et maintenabilité
- **Retours utilisateurs** : Signalement de bugs intermittents sur certaines fonctionnalités critiques, lenteur occasionnelle dans le traitement.

### Frontend

- **Couverture** : Actuellement 0% (trop faible, nécessite amélioration urgente)
- **Issues** : 5 issues ouvertes au total (0 bugs/critiques, 1 fiabilité, 8 maintenabilité)
- **Duplications** : 0%
- **Sécurité** : 0 hotspots détectés
- **Qualité** : Quality Gate passé, mais couverture et fiabilité restent à renforcer
- **Retours utilisateurs** : Bugs d’interface et navigation peu fluide, attentes sur performance et expérience utilisateur.

## 5. Recommandations

### Cas d’utilisation
## Scénario initial :
Au premier lancement, l’application présentait une erreur de communication entre Angular et Java (mauvais routage (endpoint non trouvé)).

## Actions temporaires mises en place :
**Modification dans le frontend Angular :**
```typescript
// Mauvaise pratique mais solution temporaire
private pathService = 'http://localhost:8080/api/joke';
```
au lieu de
```typescript
private pathService = '/api/joke';
```
➡ Permet de pointer directement sur le backend en local.

**Ajout côté backend Java pour résoudre le problème CORS :**
```java
@CrossOrigin(origins = "http://localhost:3000")
```
➡ Autorise les appels provenant du frontend local.

## Fonctionnement actuel :
Une fois l’application lancée, la page **"Bob app"** s’affiche.

**Contenu principal :**
- Un encadré avec le titre statique *"La blague du jour"*
- Une blague générée automatiquement au chargement de la page
- Un bouton **"Vite une autre"** qui recharge aléatoirement une nouvelle blague via appel API

## Améliorations prévues :
- Remplacer l’URL en dur par une configuration via `environment.ts` + proxy Angular
- Configurer un CORS global propre dans Spring Boot
- Ajouter des tests unitaires frontend pour augmenter la couverture

### Backend

- Augmenter la couverture des tests (unitaires et d’intégration) pour dépasser 60-80%.
- Résoudre rapidement les issues de fiabilité et les hotspots de sécurité.
- Ajouter des tests automatisés pour cas d’erreur et scénarios critiques.

### Frontend

- Mettre en place et améliorer les tests unitaires et e2e afin d’atteindre au moins 80% de couverture.
- Corriger les problèmes de navigation et bugs remontés par les utilisateurs.
- Maintenir une qualité stricte via SonarCloud, bloquer les merges si seuils non respectés.

### Global

- Continuer à utiliser les workflows CI/CD pour automatiser la vérification et le déploiement.
- Communiquer les métriques et retours à toute l’équipe pour impulser les améliorations.
- Surveiller les KPIs et ajuster les seuils en fonction des objectifs qualité à moyen terme.
