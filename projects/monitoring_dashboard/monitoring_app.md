# Monitoring App Overview

The **Monitoring App** is the centralized system responsible for collecting, storing, visualizing, and alerting on metrics provided by **Adapters on Monitored Hosts**. It is designed for flexibility, allowing deployments in both development (e.g., WSL) and production (e.g., Hetzner) environments.

---

## **What the Monitoring App Includes**

### 1. **Core Components**
The Monitoring App is composed of several key tools and configurations working together:

- **Prometheus**:
  - The main engine for scraping and storing metrics from adapters on monitored hosts.
  - Responsible for querying data for dashboards and alerting purposes.

- **Grafana**:
  - Provides user-friendly dashboards for visualizing the metrics collected by Prometheus.
  - Supports custom panels, filters, and queries for detailed monitoring.

- **Alertmanager**:
  - Integrates with Prometheus to send notifications when predefined thresholds or conditions are met.
  - Configurable to send alerts via email, Slack, or other messaging systems.

### 2. **Configuration and Code**
- **Scrape Configurations**:
  - Defines the endpoints for each monitored host's adapter.
  - Includes rules for scraping intervals, labels, and service discovery (if applicable).
  - Ensures all adapters (local, Hetzner, Azure) are properly queried.

- **Dashboards and Visualizations**:
  - Preconfigured Grafana dashboards for commonly monitored metrics (e.g., CPU, memory, disk, network, and Docker containers).
  - Allows customization based on specific needs for development and production.

- **Alerting Rules**:
  - Rules in Prometheus define thresholds for critical metrics (e.g., CPU usage > 90%).
  - Trigger alerts to Alertmanager when conditions are breached.

- **Secure Connections**:
  - Includes optional configurations for secure communication with monitored hosts, such as TLS for encrypted data transfer.

### 3. **Integration Code**
- **Adapters Connectivity**:
  - Code and configurations for connecting to adapters on monitored hosts, whether they are local (e.g., WSL) or remote (e.g., Hetzner, Azure).
  - Includes support for static host definitions, dynamic service discovery, or API-based integrations (e.g., Azure Monitor).

### 4. **Deployment Templates**
- **Development (Local WSL)**:
  - Lightweight configuration to test monitoring functionality using local Docker containers and system metrics.
- **Production (Hetzner)**:
  - Scaled configuration for monitoring multiple hosts, containers, and cloud services.
  - Includes templates for persistent storage and high-availability setups.

---

## **Deployment Independence**
- The Monitoring App is designed to be self-contained and can be deployed independently of the monitored hosts and their adapters.
- Supports multiple monitored environments, querying metrics from WSL, Hetzner, and Azure simultaneously.

---

## **Key Benefits**
- **Centralized Monitoring**:
  - Unified dashboards and alerts for all monitored hosts, regardless of their environment.
- **Flexible Deployment**:
  - Same Monitoring App can serve both development and production setups with minor configuration changes.
- **Extensibility**:
  - Easily integrates with new monitored hosts and adapters by updating scrape configurations.

---