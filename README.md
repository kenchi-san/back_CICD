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


