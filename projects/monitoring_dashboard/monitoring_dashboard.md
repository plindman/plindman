# Project Plan: Full Monitoring Solution

This project plan outlines all steps required to create a comprehensive monitoring solution integrating local host, Hetzner, and Azure resources into a unified dashboard.

## Solution architecture

[Architecture](monitoring_dashboard/monitoring_architecture.md)

## Plan 

This plan ensures a scalable and unified monitoring solution across the infrastructure.

:::mermaid
gantt
   title Project Plan for Monitoring Solution
   dateFormat  YYYY-MM-DD
   axisFormat  %W
   tickInterval 1week
   weekday monday
   todayMarker off

   section App
      Dev Env                       :core1, 2024-01-01, 1w
      Host Monitoring               :core2, after core1, 1w
      Docker Monitoring             :core3, after core1, 1w
      Security Monitoring           :core3, after core1, 1w
      Alerts & Notifications        :core5, 2024-01-22, 1w

   section Dashboards
      Unified Dashboards            :dash1, after core1, 1w
      Integrate Hetzner             :dash2, after dash1, 1w
      Integrate Azure               :dash3, after dash2, 1w

   section Adapters
      Local Endpoint                :host1a, after core1, 1w
      Local Exporters               :host1b, after core1, 1w

      Hetzner Endpoint              :host2a, after host1b, 1w
      Hetzner Exporters             :host2b, after host1b, 1w
      Endpoint Security             :host2c, after host1b, 1w

      Azure Endpoint                :host3a, after host2c, 1w
      Azure Exporters               :host3b, after host2c, 1w

   section Production
      Prod Environment              :prod1, after host3b, 1w
      Security Hardening            :prod2, after host3b, 1w

   section Maintenance
      Optimize & Maintain           :maint, after prod1, 1w
      Security Monitoring           :sec2, after prod2, 1w
:::

| **Section** | **Phase** | **Description** | **Expected Time** |
|:------------|:---------|:----------------|:-----------------:|
| **App** | Local Dev Environment  | Set up and test monitoring stack on local host. | 1–2 days |
|         | Host Monitoring        | Monitor system metrics on local host. | 1–2 days |
|         | Docker Monitoring      | Monitor Docker containers on local host. | 1–2 days |
|         | Security Monitoring      | Monitor security on local host. | 1–2 days |
|         | Alerts & Notifications | Set up alerts and notifications for key metrics. | 1 day |
| **Dashboards & Visualization**  | Unified Dashboards| Create unified dashboard in Grafana using local data. | 1 day |
|                                 | Hetzner Dashboard | Integrate Hetzner metrics into unified dashboards | 1 day |
|                                 | Azure Dashboard | Integrate Azure metrics into unified dashboards | 1 day |
| **Host Adapters** | localhost adapter | Integrate Hetzner-specific resources into monitoring.| 1–2 days |
|                   | Hetzner Host adapter | Integrate Hetzner-specific resources into monitoring.| 1–2 days |
|                   | Azure Host adapter   | Integrate Azure resources into monitoring. | 1–2 days |
| **Production environment** | Hetzner Prod Environment    | Deploy monitoring stack on Hetzner with FQDN setup.  | 2–3 days          |
|                            | Security Hardening          | Define and implement access control, encryption, etc.| 2–3 days          |
| **Maintenance**                 | Optimize & Maintain         | Periodic updates, tuning, and scaling.               | Ongoing           |
|                                  | Security Monitoring         | Monitor security-related metrics in the stack.       | Ongoing          |

## Local Dev Environment w/ Local Monitoring App

Set up and test the entire monitoring stack in a local environment.

1. Install Docker and Docker Compose on the local dev host.
2. Set up the local adapter with an endpoint for system and Docker monitoring.
3. Deploy Prometheus, Grafana, cAdvisor, and Node Exporter locally.
4. Configure Prometheus to scrape metrics from the local adapter.
5. Visualize metrics in Grafana.

### Local Host System and Docker Monitoring

Set up system-level monitoring to track the health of the local host. Key tasks include:
- Deploying Node Exporter and cAdvisor for metric collection.
- Configuring both tools to expose metrics via an endpoint.
- Configuring Prometheus to scrape metrics from these endpoints.
- Verifying metrics such as CPU, RAM, disk usage, and system load.

### Create Unified Dashboards

Design Grafana dashboards to integrate:
- Host system metrics.
- Docker container metrics.
- Security metrics (e.g., failed login attempts, audit logs)

## Add Hetzner Server Support

1. Create and deploy the Hetzner adapter on the Hetzner server.
2. Configure Prometheus to scrape metrics from the Hetzner adapter endpoint.
3. Deploy Node Exporter and cAdvisor on Hetzner for system and Docker monitoring.
4. Update Grafana dashboards to include Hetzner metrics.

### Integrate Hetzner metrics into unified dashboards

Integrate Hetzner-specific infrastructure into the unified monitoring solution:
- Consolidate metrics from all sources—local host, Hetzner, and Docker containers—into Grafana dashboards.
- Aggregate and logically group metrics for easier tracking and comparison.
- Ensure dashboards are real-time and interactive.

## Develop Alerts and Notifications

Set up robust alerts and notification systems to address key thresholds and system conditions.

**Objectives:**
- Ensure timely notifications for system health and failures.
- Create escalation rules for critical issues.
- Test alert configurations to verify reliability.

***Tasks:**

1. Deploy Prometheus Alertmanager.
2. Configure alerting rules in Prometheus for:
   - High resource usage.
   - Service failures or downtime.
   - Security monitoring (e.g., for abnormal access attempts or unauthorized changes).
3. Set up Grafana alerts for key metrics.
4. Configure notification channels, such as:
   - Email.
   - Slack or messaging services.
   - Webhooks.

## Add Azure Server Support

1. Create and deploy the Azure adapter with an endpoint for metrics.
2. Configure Prometheus to scrape metrics from the Azure adapter.
3. Integrate Azure Monitor into Grafana to visualize metrics like:
   - Website uptime and performance.
   - Storage account usage.
   - Virtual Machine (VM) health.

### Unified Dashboards

Expand dashboards to incorporate Azure metrics:
- Consolidate data from local host, Hetzner, Docker, and Azure into a unified view.
- Aggregate metrics logically for consistency across all monitored environments.
- Enhance interactivity for real-time monitoring and insights.

## Production Environment

1. Install Docker and Docker Compose on the Hetzner host.
2. Transfer the monitoring stack configuration from WSL to Hetzner.
3. Deploy the monitoring stack on Hetzner, including Prometheus, Grafana, and adapters.
4. Set up NGINX as a reverse proxy for Grafana and Prometheus.
5. Secure remote access with SSL (optional).
6. Configure Prometheus to scrape metrics from Hetzner and Docker containers.
7. Use a Fully Qualified Domain Name (FQDN) for remote access.

### Security Hardening

Secure the production environment to protect the monitoring solution from unauthorized access and attacks:

- **Define Security Requirements**:
   - Identify sensitive components (e.g., credentials, FQDN, IPs).
   - Determine access control and encryption standards.

- **Secure Access**:
   - Implement Role-Based Access Control (RBAC) for Grafana users.
   - Use basic authentication or reverse proxy rules to secure Prometheus endpoints.

- **Protect Data in Transit**:
   - Enforce HTTPS for all external access.
   - Use VPN or firewall rules for internal communication between Prometheus, Grafana, and adapters.

## Ongoing Maintenance

Maintain and optimize the monitoring stack over time:

- Regularly update all monitoring components (Docker images, Prometheus, Grafana, etc.).
- Adjust scrape intervals and storage settings in Prometheus to handle data growth.
- Periodically review and refine dashboards and alerts to match evolving requirements.
- Add new monitored resources or environments as needed.

### Security Monitoring

Monitor the security of the monitoring solution itself to detect and address vulnerabilities:

1. Track unauthorized access attempts and failed login attempts.
2. Set up alerts for suspicious activity within the monitoring stack.
3. Monitor SSL certificate expiration and renewal.

### Key Actions

- Create dashboards to track security metrics like failed login attempts and certificate status.
- Stay updated by patching vulnerabilities in the monitoring stack components.