# Cahier des Charges

Projet : Backend HTTP Terraform & Interface Web de gestion des states
Version : 0.1 – 11 juillet 2025
branche: PDG

## 1. Contexte et objectifs

Implémenter une solution de stockage distant des fichiers *state* Terraform conforme à la spécification “backend HTTP”.
Le système doit offrir :

* La persistance, la haute disponibilité et la résilience des *states*.
* Le chiffrement **at‑rest** à l’aide d’une clé fournie par l’utilisateur, et un protocole de restauration locale en cas de *disaster recovery*.
* Une interface Web de gouvernance (projets, utilisateurs, jetons, historique).

## 2. Scope du Projet

### 2.1 Backend HTTP

* Endpoints conformes à la spec officielle : `GET/PUT/LOCK/UNLOCK/DELETE /state/{project}`.
* Gestion des versions de *state* et métadonnées.
* Stockage des *states* chiffrés

### 2.2 Chiffrement at‑rest

* Algorithme AES‑256‑GCM ?
* Clé de chiffrement symétrique fournie au moment du *init* Terraform.<br>
    -> Symétrique autrement personne d'autre ne pourra accéder au state
* Dérivation/rotation gérées côté client. Le serveur ne stocke que le blob chiffré.

**à définir**: On stocke où la clef ? Keystore, comme pour android ? Déchiffrement du state lors de l'envoi, et re chiffrement derrière avec TLS pour le trajet ?

### 2.3 Interface Web

* Tableau de bord listant les projets et leur dernier commit.
* Visualisation JSON du *state* et diff entre versions.
* Restauration vers une version antérieure.
* CRUD utilisateurs, groupes et ACL par projet.
* Gestion des tokens (scopés projet ou utilisateur).

## 3. Exigences fonctionnelles détaillées

| Fonction       | Description                    | Priorité |
| -------------- | ------------------------------ | -------- |
| Upload state   | PUT sur `/state/{project}`     | Must     |
| Download state | GET sur `/state/{project}`     | Must     |
| Lock/Unlock    | POST `/state/{project}/lock`   | Must     |
| Versioning     | Liste et restauration versions | Must     |
| Encryption     | Chiffrement serveur‑side       | Must     |
| UI dashboard   | Vue projets & historique       | Should   |
| RBAC           | Droits par rôle & groupe       | Must     |
| Audit log      | Journal des actions            | Can      |

## 4. Exigences non fonctionnelles

**Ces exigences peuvent changer**

* **Performance :** Peu de latence lors d'un `GET`.</br>
    -> Serveurs Clouds herbergés en Europe.
* **Scalabilité :** Tenir beaucoup de requêtes simultanées sans dégradation. </br>
    -> Load balancer, scaling horizontal
* **Sécurité :** conforme OWASP top 10 ?.
* **Disponibilité :** 99,9 % SLA mensuel.
* **Portabilité :** déploiement containerisé (Docker ?) <br>
    -> K8s/Fly.io ?

### Optionnel

* **Traçabilité :** logs centralisés dans un fichier JSON + métriques Prometheus.

## 5. Architecture technique préliminaire

```
[Client Terraform] ──HTTP──> [API Java] ─┬─> [Service chiffrement]
                                       ├─> [Object Storage (S3)]
                                       └─> [PostgreSQL metadata]
                                                  ↑
                                [UI React] <──────┘
```

* **API** : [TODO: Java + frameworks] + middleware Auth/JWT ?
* **Chiffrement** : TLS pour les packets, AES-256-GCM.
* **Stockage** : Cloud Azure / AWS.
* **Metadata DB** : -
* **AuthN** : OAuth2 + JWT (access/refresh) ?

### Optionnel 

* **Observabilité** : OpenTelemetry, Prometheus, Grafana

## 6. Choix technologiques

| Domaine         | Technologie                       | Raison                           |
| --------------- | --------------------------------- | -------------------------------- |
| Langage backend |                                   |                                  |
| Framework API   | Java                              |                                  |
| Frontend        | React / Material UI               | SSR, écosystème riche            |
| CI/CD           | GitHub Actions                    | Intégration GitHub & marketplace |
| IaC             | Terraform                         | Cohérence avec use‑case          |
| Conteneurs      | Docker                            | Standard                         |
| Orchestration   | Docker Compose dans VM / K8s      | Simplicité vs scalabilité        |
| Tests           | Unit Testing, Cypress             | Unit & E2E                       |

## 7. Processus de développement

* Méthode **Agile Kanban** avec sprint review hebdo.
* **Git Flow** : `main`, `develop`, *feature/*, *release/*.
* Convention de commit **Conventional Commits**.
* Revue de code obligatoire (au moins 1 approbation) + éventuellement analyse statique (linters).
* **Definition of Done** : tests verts, couverture >80 %, doc mise à jour.

## 8. CI/CD & IaC

1. **Build & test**
   unit testing  + linters
2. **Build image**
   `docker build` -> push registry
3. **Security scan**
   Trivy (images) ?
4. **Deploy dev**
   Terraform apply + seed DB
5. **Deploy staging** (tag `rc-*`)
   Mêmes steps + tests E2E ?
6. **Deploy prod** (tag `v*`)
   Appro manuelle, migration DB, *blue‑green*.

## 9. Environnements & outillage

| Environnement | URL                | Objectif             |
| ------------- | ------------------ | -------------------- |
| **Dev**       | `dev.<domain>`     | Intégration continue |
| **Staging**   | `staging.<domain>` | QA & UAT             |
| **Prod**      | `app.<domain>`     | Client final         |

* Outils: GitHub Projects (Kanban), Slack/Teams, Figma, Sentry, Grafana Cloud.

## 10. Qualité et tests

* Couverture code 80 %; seuil critique 70 %.
* Tests intégration avec MinIO & Postgres en conteneur.
* Tests E2E UI sur staging.
* Stress tests.

## 11. Sécurité

* Stockage secret via GitHub OIDC → HashiCorp Vault.
* Headers : CSP, HSTS.
* Limitation rate : 100 req/min/IP.
* Auth multi‑facteur pour UI admin.

## 12. Gestion des risques

| Risque                    | Impact        | Prob.  | Plans mitigation                 |
| ------------------------- | ------------- | ------ | -------------------------------- |
| Sous‑estimation effort UI | Délai         | Moyen  | Prototyper tôt, allouer FE dédié |
| Performance S3            | Latence       | Faible | Bench early, cache CDN           |
| Fail clés chiffrement     | Perte données | Faible | Backup KMS + rotation doc        |

## 14. Critères d’acceptation

* Tous endpoints passent la suite d’intégration officielle Terraform.
* Temps de restore complet <30 s pour un state 10 MiB.
* UI permet rollback “en un clic”.
* Aucune CVE critique ouverte dans le container.

## 15. Documentation & livrables

* Guide d’intégration Terraform (README).
* Manuel admin UI.
* Schéma d’archi draw.io + fichier source.
