## Environment Definitions
### Development (Ontwikkel)
Local development and initial security analysis. Researchers run the vulnerable module here, reproduce findings, and develop patches.
### Configuration
- No required reviewers
- No branch restrictions
- No wait timer
- All team members can deploy freely

Speed of iteration is t he priority at this stage. The environment is local and not externally exposed, so strict gate controls add friction without security benefit.

---
### Test
Automated validation. The CI pipeline (GitHub Actions) deploys here to run unit tests, integration tests, and static analysis on every push
### Configuration
- No required reviewers
- No branch restrictions
- Automated deployments via GitHub Actions

CI must be able to deploy without manual approval to keep the feedback loop fast. The environment uses isolated test data and has no external exposure.

---
### Acceptance
Demonstration and validation of findings and patches. Used to present audit results to assessors and verify that patches resolve vulnerabilities without breaking functionality.
### Configuration
- Required reviewers: 1 team member must approve before deployment
- Cannot be self-reviewed
- Deployment restricted to `main` branch only

Acceptance is a shared, stable environment. Requiring a reviewer ensure only validated work reaches this stage, preventing half-finished patches from being demonstrated to assessors. Branch restriction to `main` enforces that only merged, reviewed code is deployed here.

----
### Production
The final reference environment representing a compliant, hardened deployment of the module. Serves as evidence of the end state after audit findings have been resolved.
### Configuration
- Required reviewers: 1 team member must approve before deployment
- Cannot be self-reviewed
- Deployment restricted to `main` branch only
- Wait timer: 5 minutes before deployment proceeds
- Hardened Docker Compose configuration

Production represents the highest-risk environment. The wait timer introduces a forced pause, giving reviewers time to catch last-minute issues before deployment proceeds. Combined with required reviewers no self-review and branch restrictions. this satifies NEN-7510 A.8.3 (access control) and A.8.5 (authentication) by ensuring no single person can unilaterally deploy to production.

## Docker Compose Structure
Each environment has a corresponding Docker Compose override file. The base `docker-compose.yml` defines the shared service configuration (OpenMRS backend, MariaDB, nginx gateway). Environment-specific overrides are layered on top and only define what differs from the base.
- `docker-compose.yml` - Shared Base
- `docker-compose.dev.yml` - debug port exposed on port 5005
- `docker-compose.test.yml` - gateway ports removed, backend exposed directly on 8080
- `docker-compose.acc.yml` - schema management disabled, module web admin on for demo uploads
- `docker-compose.prod.yml` - module web admin off, no credential defaults, resource limits applied

Run a specific environment with:
```bash
docker compose -f docker-compose.yml -f docker-compose.<env>.yml up -d
```

Production requires all credential environment variables to be set explicitly. There are no fallback default, the stack will fail to start if any are missing. This is intentional and prevents accidental deployment with weak credentials.
```bash
OMRS_DB_USER=user 
OMRS_DB_PASSWORD=password 
OMRS_DB_ROOT_PASSWORD=rootpassword

docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

## NEN-7510 Alignment

| **Environment** | **Protection Measure**                 | **NEN-7510 Control**      |
| --------------- | -------------------------------------- | ------------------------- |
| Acceptance      | Required reviewer before deployment    | A.8.3 Toegangsbeveiliging |
| Production      | Required reviewer + wait timer         | A.8.3 Toegangsbeveiliging |
| Production      | `main` branch only                     | A.8.5 Authenticatie       |
| Production      | No default credentials, no debug       | A.8.1.5 Logging           |
| All             | Environment secrets isolated per stage | A.8.3 Toegangsbeveiliging |

## Summary

| **Environment** | **Reviewers**     | **Branch Restriction** | **Wait Timer** |
| --------------- | ----------------- | ---------------------- | -------------- |
| Development     | None              | None                   | None           |
| Test            | None              | None                   | None           |
| Acceptance      | 1 Unique Required | `main` only            | None           |
| Production      | 1 Unique Required | `main` only            | 5 Minutes      |
