# Monitoring Solution Architecture

This architecture outlines the components and structure of the monitoring solution, which consists of Prometheus, Grafana, and various exporters.

[Diagrams](diagrams.md)

---

## System Architecture

1. **Centralized Monitoring App**
1. **Adapters on the Monitored Hosts**

### Centralized Monitoring App

- **Role**: Core application responsible for collecting, storing, visualizing metrics, and sending alerts.
- **Deployment**: Can run independently in any environment (local development or production).

### Adapters on the Monitored Hosts

- **Role**: Lightweight components deployed on monitored hosts, exposing system and application metrics to the Monitoring App.
- **Deployment**: Configured and deployed independently of the Monitoring App.

### Relationships Between Roles

1. **Users ↔ Monitoring App**:
   - Users access dashboards hosted on the Monitoring App to view real-time metrics and trends.
   - Users receive alerts from the Monitoring App via configured channels (e.g., email, messaging services).

1. **Monitoring App ↔ Monitored Hosts**:
   - The Monitoring App periodically queries the adapters on the monitored hosts to collect metrics.
   - The monitored host adapters run independently, exposing metrics regardless of the Monitoring App's deployment.
   - Any monitored host can expose metrics to multiple Monitoring App deployments (e.g., both local and production instances).

## Tech Stack

This chapter outlines the key technologies and software that form the monitoring solution, along with their specific roles in the system:

### Monitoring App

| Tech/software | Role | Details | 
|:-|:-|:-|
| **Prometheus** | Central metrics collection and storage engine. | Scrapes metrics from adapters on monitored hosts and stores them for querying and alerting. It is the backbone of the monitoring system, enabling the pull-based collection of time-series data. |
| **Grafana** | Visualization and dashboarding tool. | Provides a user-friendly interface for creating and viewing real-time dashboards. Grafana integrates with Prometheus to query and display metrics while also offering alerting functionality. |
| **Alertmanager** | Notification and alert management. | Integrates with Prometheus to handle alerts generated based on predefined thresholds, distributing them via configured channels like email or Slack. | 
| **NGINX**<br>(in Production Environment) | Reverse proxy and SSL terminator.  | Manages secure access to Prometheus and Grafana dashboards by serving as a reverse proxy and enabling HTTPS for external users. |
| **Docker & Docker Compose** | Containerization and orchestration. | Used to deploy all components (Prometheus, Grafana, exporters, etc.) in isolated environments, ensuring portability and consistency across local and production setups. |

### Adapters on Monitored Hosts

| Tech/software | Role | Details | 
|:-|:-|:-|
| **Node Exporter** | Role: System metrics exporter. | Exposes hardware and OS-level metrics such as CPU usage, memory usage, and disk performance for Prometheus to scrape. |
| **cAdvisor** | Docker container metrics exporter. | Collects detailed metrics on running containers, including resource usage and performance data. |
| **Custom Adapters** | Extend monitoring to custom applications or services. | These may include scripts or tools tailored to specific use cases, such as monitoring databases or web servers. |
| **Azure Monitor (optional)**: |  Native Azure metrics collection. | Integrated into the monitoring system to gather metrics from Azure-specific resources such as VMs or storage accounts. |

### Alternative Technologies

While the chosen tech stack leverages widely used and robust tools, alternatives could be considered depending on specific needs:

- **Telegraf + InfluxDB + Chronograf**: A comparable stack with Telegraf for data collection, InfluxDB for time-series storage, and Chronograf for visualization. This stack is a strong contender but lacks some of the flexibility and ecosystem integration of Prometheus and Grafana.
- **Elastic Stack (ELK/EFK)**: ElasticSearch for data storage, Kibana for visualization, and Beats for metric collection. This solution offers powerful search capabilities and scalability but may be overkill for simpler monitoring setups.
- **Zabbix**: A comprehensive monitoring system that combines data collection, visualization, and alerting in a single package. However, it has a steeper learning curve and may be less flexible for containerized environments.
- **OpenTelemetry**: A vendor-neutral option for observability. While OpenTelemetry excels at tracing and logs, it can also collect metrics but requires additional effort to integrate with storage and visualization tools.

The current stack was chosen for its balance of simplicity, flexibility, and broad community support, making it suitable for both hobby projects and scalable production environments.

## Monitoring App

The **Monitoring App** is the centralized system responsible for collecting, storing, visualizing, and alerting on metrics provided by **Adapters on Monitored Hosts**. It is designed for flexibility, allowing deployments in both development and production environments.

- **Role**: Central system responsible for collecting, visualizing, and alerting based on metrics.
- **Core Features**:
  - **Data Collection**: Periodically scrapes metrics from adapters running on monitored hosts.
  - **Visualization**: Displays metrics in user-friendly dashboards for system and application monitoring.
  - **Alerting**: Sends notifications when thresholds or conditions are breached (e.g., resource exhaustion).
- **Components**:
  - **Prometheus**: The main engine for scraping and storing metrics from adapters on monitored hosts.
  - **Grafana**: Provides user-friendly dashboards for visualizing the metrics collected by Prometheus.
  - **Alertmanager**: Integrates with Prometheus to send notifications when predefined thresholds or conditions are met.
- **Deployment**:
  - **Development**: Runs locally for testing and debugging.
  - **Production**: Deployed on production infrastructure to monitor production workloads.

### Prometheus

- The main engine for scraping and storing metrics from adapters on monitored hosts.
- Responsible for querying data for dashboards and alerting purposes.
- Collects data from the adapter endpoints on monitored hosts.
- Stores collected metrics from various sources (exporters, applications, etc.) using a pull model.

### Grafana

- Visualizes Prometheus metrics in the form of dashboards.
- Supports custom panels, filters, and queries for detailed monitoring.

### Alertmanager

- Handles alert notifications generated by Prometheus when certain thresholds are met.
- Alerts for container failures or high resource usage in the local environment.
- Alerts for system-level failures, resource over-utilization, or other critical issues on hosts.
- Configurable to send alerts via email, Slack, or other messaging systems.

## Adapter on Monitored Host

The **Adapter on Monitored Host** is a lightweight, independent component deployed on monitored systems. Its primary responsibility is to expose system and application metrics in a format that the **Monitoring App** (running Prometheus) can scrape and process.

- **Role**: Lightweight components deployed on monitored hosts, exposing system and application metrics to the Monitoring App.
- **Core Features**:
  - **Expose Metrics**: Collect and serve system and application metrics.
  - **Support Monitoring App**: Serve metrics for Prometheus running on the Monitoring App.
- **Key Benefits**:
  - **Simplicity**: Focused on collecting and exposing metrics without additional complexity.
  - **Lightweight**: The adapter does not include heavy components like databases or dashboards. It only exposes metrics.
  - **Scalability**: Supports large-scale monitoring by being lightweight and stateless.
  - **Independent**: Can run without any connection to the centralized Monitoring App. Metrics remain accessible for any compatible scraping tool.
  - **Flexibility**: Works across diverse environments, including local, on-premise, and cloud.
  - **Reusable**: A single adapter can serve multiple Monitoring App deployments (e.g., local dev and production).
  - **Extendability**: Can be enhanced with additional exporters or custom scripts as needed.
- **Components**:
  - **Endpoint for Metrics Exposure**
  - **Exporters**: Expose metrics from the monitored system and applications.
- **Exporter Types**:
  - **System Metrics Exporter**: System-level metrics such as CPU and memory usage.
  - **Application Exporter(s)**: Metrics for specific applications or services, such as Docker containers (e.g., container stats, image usage).
  - **Optional Exporters**: Specific exporters can be added based on the host's environment, such as database and web server metrics.
- **Exporter Software**:
  - **Node Exporter**: Collects system-level metrics (CPU, RAM, disk usage, etc.).
  - **cAdvisor**: Collects Docker container metrics (CPU, memory, network).
- **Deployment**:
  - Runs independently on each monitored host.
  - Configured to expose metrics through standard protocols (e.g., HTTP endpoints for scraping).
- **Supported Hosts**:
  1. **Local WSL Host**:
     - Development environment metrics, including Docker container metrics and local system performance.
  1. **Hetzner Servers**:
     - Production host metrics for VMs, bare-metal servers, and Docker containers.
  1. **Azure Resources**:
     - Metrics for VMs, cloud-hosted applications, and Azure-specific services.
