# Specifications Document

Project: TerraHarbor
Version: 0.1 – July 11, 2025  
Branch: PDG

## 1. Context and Objectives

Implement a remote storage solution for Terraform *state* files compliant with the “HTTP backend” specification.  
The system must offer:

* Persistence, high availability, and resilience of *states*.
* **At-rest** encryption using a key provided by the user, and a local restoration protocol in case of disaster recovery.
* A governance web interface (projects, users, tokens, history).

## 2. Project Scope

### 2.1 HTTP Backend

* Endpoints compliant with the official spec: `GET/PUT/LOCK/UNLOCK/DELETE /state/{project}`.
* Endpoints for authentication: `/register /login`
* Management of *state* versions and metadata.
* Encrypted *state* storage

### 2.2 At-rest Encryption

* AES‑256‑GCM algorithm?
* Symmetric encryption key provided at the time of Terraform *init*.<br>
    -> Symmetric otherwise no one else will be able to access the state
* Derivation/rotation managed on the client side. The server only stores the encrypted blob.

**To be defined**: Where do we store the key? Keystore, like for Android? Decryption of the state when sending, and re-encryption behind with TLS for the path?

### 2.3 Web Interface

* Dashboard listing projects and their last commit.
* JSON visualization of the *state* and diff between versions.
* Restore to a previous version.
* CRUD users, groups and ACL by project.
* Token management (scoped to project or user).

## 3. Detailed Functional Requirements

| Function       | Description                     | Priority |
| -------------- | ------------------------------- | -------- |
| Upload state   | PUT on `/state/{project}`       | Must     |
| Download state | GET on `/state/{project}`       | Must     |
| Lock/Unlock    | POST `/state/{project}/lock`    | Must     |
| Versioning     | List and restore versions       | Must     |
| Encryption     | Server-side encryption          | Must     |
| UI dashboard   | Projects & history view         | Should   |
| RBAC           | Role & group permissions        | Must     |
| Audit log      | Action journal                  | Can      |

## 4. Non-functional Requirements

**These requirements may change**

* **Performance:** Low latency on a `GET`.</br>
    -> Cloud servers hosted in Europe.
* **Scalability:** Handle many simultaneous requests without degradation. </br>
    -> Load balancer, horizontal scaling
* **Security:** OWASP top 10 compliance?.
* **Availability:** 99.9% monthly SLA.
* **Portability:** Containerized deployment (Docker?) <br>
    -> K8s/Fly.io?

### Optional

* **Traceability:** Centralized logs in a JSON file + Prometheus metrics.

## 5. Preliminary Technical Architecture

```
[Terraform Client] ──HTTP──> [Python API] ─┬─> [Encryption Service]
                                 ↑     ├─> [Object Storage]
                                 |     └─> [Data Storage]
                                 |
               [React UI] <──────┘
```

* **API**: Python with FastAPI framework + Auth/JWT ?
* **Encryption**: TLS for packets, AES-256-GCM for at-rest states.
* **Storage**: Azure / AWS Cloud.
* **Metadata DB**: -
* **AuthN**: OAuth2 + JWT

### Optional 

* **Observability**: OpenTelemetry, Prometheus, Grafana

## 6. Technological Choices

| Domain             | Technology                         | Reason                                                         |
| ------------------ | ---------------------------------- | -------------------------------------------------------------- |
| Backend language   | Python                             | No heavy calculatoion required, easy to handle and well-known  |
| API Framework      | FastAPI                            | Really easy to use, handles Oauth2.                            |
| Frontend           | React / Vite / Material UI         | SSR, rich ecosystem                                            |
| CI/CD              | GitHub Actions                     | GitHub integration & marketplace                               |
| IaC                | Terraform                          | Coherence with use case                                        |
| Containers         | Docker                             | Standard                                                       |
| Orchestration      | Docker Compose in VM / K8s         | Simplicity vs scalability                                      |
| Tests              | Unit Testing, Cypress, pytest      | Unit & E2E                                                     |

## 7. Development Process

* **Agile Kanban** method with weekly sprint reviews.
* **Git Flow**: `main`, *feature/*, *release/*.
* Commit convention: **Conventional Commits**.
* Mandatory code review (at least 1 approval) + optionally static analysis (linters).
* **Definition of Done**: passing tests, >80% coverage, updated documentation.

## 8. CI/CD & IaC

1. **Build & test**
   unit testing + linters
2. **Build image**
   `docker build` -> push registry
3. **Security scan**
   Trivy (images)?
4. **Deploy dev**
   Terraform apply + seed DB
5. **Deploy staging** (tag `rc-*`)
   Same steps + E2E tests?
6. **Deploy prod** (tag `v*`)
   Manual approval, DB migration, *blue-green*.

## 9. Environments & Tooling

| Environment  | URL                | Purpose                  |
| ------------ | ------------------ | ------------------------ |
| **Dev**      | `dev.<domain>`     | Continuous integration   |
| **Staging**  | `staging.<domain>` | QA & UAT                 |
| **Prod**     | `app.<domain>`     | Final client             |

* Tools: GitHub Projects (Kanban), Slack/Teams, Figma, Sentry, Grafana Cloud.

## 10. Quality and Testing

* Code coverage 80%; critical threshold 70%.
* Integration tests with MinIO & Postgres in container.
* E2E UI tests on staging.
* Stress tests.

## 11. Security

* Secret storage via GitHub OIDC → HashiCorp Vault.
* Headers: CSP, HSTS.
* Rate limiting: 100 req/min/IP.
* Multi-factor authentication for admin UI (?).

## 12. Risk Management

| Risk                        | Impact          | Prob.     | Mitigation Plans                        |
| -------------------------   | --------------- | ------    | --------------------------------------- |
| Underestimating UI effort   | Delay           | Medium    | Prototype early, allocate dedicated FE  |
| VM Performance              | Latency         | Low       | Bench early, cache CDN                  |
| Encryption key failure      | Data loss       | Low       | Backup KMS + rotation doc               |

## 14. Acceptance Criteria

* All endpoints pass the official Terraform integration test suite.
* Full restore time <30 s for a 10 MiB state.
* UI allows “one-click” rollback.
* No open critical CVEs in the container.

## 15. Documentation & Deliverables

* Terraform integration guide (README).
* Admin UI manual.
* draw.io architecture diagram + source file.
