# Git Strategy

By following this Git strategy, the Monitoring App and Adapters will be independently manageable, ensuring long-term flexibility and scalability.

## Overview

1. **Separate Repositories for Monitoring App and Adapters**:

   - **Monitoring App**: One repository to manage Prometheus, Grafana, and Alertmanager configurations and deployment scripts.

   - **Adapters**: Each adapter (e.g., `localhost`, `docker`, `hetzner`, `azure`) gets its own repository.

   Ensures each component is independently versioned, deployed, and updated.

2. (optional) centralized Integration Repository:

   - A separate "meta-repository" can be created to manage the integration of all components. It includes submodules or references to specific adapter versions.

---

## Repository Structure

Structure:
``` bash
monitoring-integration
├── monitoring-app/ (repository)
├── adapters/ (repository)
│   ├── monitoring-app-adapter-localhost/ (repository)
│   ├── monitoring-app-adapter-hetzner/ (repository)
│   └── monitoring-app-adapter-azure/ (repository)
└── README.md
```

## Monitoring App Repository

- **Repository Name**: `monitoring-app`

- Branches:
    - main: Stable and production-ready configurations.
    - develop: For testing and developing new configurations.
    - Feature branches (e.g., feature/add-slack-alerts).

- Versioning:

    Use semantic versioning (v1.0.0) for releases of the Monitoring App.

### Adapter Repositories

- Repository Names:
    - `monitoring-app-adapter-localhost`
    - monitoring-app-adapter-docker
    - monitoring-app-adapter-hetzner
    - monitoring-app-adapter-azure

- Branches:
    - main: Stable and production-ready configurations.
    - develop: For testing and developing new configurations.
    - Feature branches (e.g., feature/add-slack-alerts).

    Same structure as the Monitoring App (main, develop, feature branches).

- Versioning:

    Use semantic versioning for each adapter independently (e.g., v1.0.0 for the adapter-localhost).

## (optional) Integration Repository

- **Repository Name:** `monitoring-app-integration`

## Branching Strategy

- GitFlow Workflow:
    - Use GitFlow across all repositories:
        - main: Stable, production-ready branch.
        - develop: Development and testing branch.
        - feature/{feature-name}: Feature-specific branches.

- Synchronization:
    - Maintain loose coupling between Monitoring App and Adapters:
    - Only update the Monitoring App when a new adapter release is stable.
    - Use tags or releases to lock adapter versions in Prometheus configuration:

- Example: Use adapter-localhost:v1.2.0 in prometheus.yml.

### Collaboration Best Practices

- Separate Teams:
    - Assign separate teams or individuals to manage the Monitoring App and Adapters.
    - Use integration tests to ensure compatibility.

- Testing:
    - Each repository includes unit tests and integration tests.
    - Test adapters locally before updating Prometheus configurations.

- Documentation:
    - Maintain clear and up-to-date README.md files in each repository with setup and deployment instructions.

- Automation:
    - Use CI/CD pipelines for each repository to:
        - Run tests.
        - Build Docker images (if applicable).
        - Push updates to a container registry or deploy directly.
