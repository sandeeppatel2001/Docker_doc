# Prometheus and Grafana Setup Guide
Prometheus is a powerful monitoring and alerting tool designed for reliability and scalability. It collects and stores its data as time series, providing metrics data in a matrix format. Grafana, on the other hand, is a multi-platform open-source analytics and interactive visualization web application, which allows you to query, visualize, alert on, and understand your metrics.
- Below are the steps to set up Prometheus and Grafana using Docker.
## 1. Prometheus Configuration
Create a prometheus.yml configuration file with the following content:
```
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: "restapi"
    static_configs:
      - targets:
          - "10.9.0.14:8900"
        labels:
          instance: "server"

      - targets:
          - "10.9.0.15:9100"
        labels:
          instance: "database_pgsql"
```
This configuration file tells Prometheus to scrape metrics from the specified targets every 5 seconds.
Use the following command to run Prometheus in a Docker container:
```
sudo docker run --rm -i -t --net=host -p 0.0.0.0:9090:9090 \
-v ./prometheus.yml:/etc/prometheus/prometheus.yml \
prom/prometheus:v2.20.1
```
- --rm: Automatically remove the container when it exits.
- -i -t: Keep STDIN open and allocate a pseudo-TTY.
- --net=host: Use the host network.
- -p 0.0.0.0:9090:9090: Map port 9090 on the host to port 9090 in the container.
- -v ./prometheus.yml:/etc/prometheus/prometheus.yml: Mount the Prometheus configuration file from the host into the container.
# Running Grafana with Docker
Use the following command to run Grafana in a Docker container:
```
sudo docker run -d -p 0.0.0.0:3000:3000 \
-v /home/ubuntu/grafana_data:/var/lib/grafana \
-i -t --net=host grafana/grafana-oss
```
- -d: Run the container in detached mode.
- -p 0.0.0.0:3000:3000: Map port 3000 on the host to port 3000 in the container.
- -v /home/ubuntu/grafana_data:/var/lib/grafana: Mount the Grafana data directory from the host into the container.
- -i -t: Keep STDIN open and allocate a pseudo-TTY.
- --net=host: Use the host network.
## Accessing the Applications
- Prometheus: Open your browser and navigate to http://localhost:9090 to access the Prometheus web UI.
- Grafana: Open your browser and navigate to http://localhost:3000 to access the Grafana web UI. The default login credentials are admin for both the username and password (By default username=admin, password=admin).
---
# Prometheus and Grafana Setup Guide

## Overview

This guide covers the setup of Prometheus and Grafana using Docker, along with specific exporters (`node_exporter` for Node.js servers and `postgres_exporter` for PostgreSQL databases) to collect metrics.

## Prometheus Setup

1. **Create Prometheus Configuration File (`prometheus.yml`):**

    ```yaml
    global:
      scrape_interval: 5s
    scrape_configs:
      - job_name: "restapi"
        static_configs:
          - targets:
              - "10.9.0.14:8900"
            labels:
              instance: "server"  # Add a label to identify this instance

          - targets:
              - "10.9.0.15:9100"
            labels:
              instance: "database_pgsql"  # Add a label to identify this instance
    ```

2. **Run Prometheus Docker Container:**

    ```bash
    sudo docker run --rm -i -t --net=host -p 0.0.0.0:9090:9090 \
      -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml \
      prom/prometheus:v2.20.1
    ```

## Grafana Setup

1. **Run Grafana Docker Container:**

    ```bash
    sudo docker run -d -p 0.0.0.0:3000:3000 \
      -v /home/ubuntu/grafana_data:/var/lib/grafana \
      -i -t --net=host grafana/grafana-oss
    ```

## Exporter Setup

### Node Exporter Setup

Follow the instructions provided in the [Grafana documentation](https://grafana.com/docs/grafana-cloud/send-data/metrics/metrics-prometheus/prometheus-config-examples/noagent_linuxnode/) to set up `node_exporter` for monitoring Node.js servers.

### PostgreSQL Exporter Setup

Follow the instructions provided in the [Grafana documentation](https://grafana.com/oss/prometheus/exporters/postgres-exporter/?tab=installation#step-1-setting-up-postgres-exporter) to set up `postgres_exporter` for monitoring PostgreSQL databases.

## Additional Steps

1. **Access Grafana**:
   - Open a browser and navigate to `http://<your_server_ip>:3000`.
   - The default login is `admin` for both the username and password (you will be prompted to change the password upon first login).

2. **Add Prometheus as a Data Source in Grafana**:
   - In Grafana, go to `Configuration` > `Data Sources`.
   - Click `Add data source`, select `Prometheus`, and configure it with the URL `http://localhost:9090` (or the appropriate URL if different).

3. **Create Dashboards**:
   - Use Grafana to create dashboards and panels to visualize the data scraped by Prometheus.

## Summary

- **Prometheus Configuration File (`prometheus.yml`)**: Defines the targets from which Prometheus will scrape metrics.
- **Prometheus Docker Command**: Runs the Prometheus container with the specified configuration file.
- **Grafana Docker Command**: Runs the Grafana container, storing its data in a persistent volume.
- **Node Exporter**: Monitors Node.js servers.
- **PostgreSQL Exporter**: Monitors PostgreSQL databases.

Ensure that the IP addresses and ports in the Prometheus configuration match the endpoints from which you want to scrape metrics. The Grafana interface will be accessible at `http://<your_server_ip>:3000`.
