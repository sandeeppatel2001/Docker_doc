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
-i -t: Keep STDIN open and allocate a pseudo-TTY.
--net=host: Use the host network.
-p 0.0.0.0:9090:9090: Map port 9090 on the host to port 9090 in the container.
-v ./prometheus.yml:/etc/prometheus/prometheus.yml: Mount the Prometheus configuration file from the host into the container.
