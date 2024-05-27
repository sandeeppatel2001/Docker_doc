# Prometheus and Grafana Setup Guide
Prometheus is a powerful monitoring and alerting tool designed for reliability and scalability. It collects and stores its data as time series, providing metrics data in a matrix format. Grafana, on the other hand, is a multi-platform open-source analytics and interactive visualization web application, which allows you to query, visualize, alert on, and understand your metrics.
- Below are the steps to set up Prometheus and Grafana using Docker.


### References.
https://squaredup.com/blog/instrument-node-with-prometheus/
https://stackabuse.com/nodejs-application-monitoring-with-prometheus-and-grafana/
https://codersociety.com/blog/articles/nodejs-application-monitoring-with-prometheus-and-grafana


----

## Version History.

- Version: 1.0, Author: Sandeep Patel, Last Updated: May 21th, 2024, Created: May 21th, 2024.

-------
## 1. Prometheus Configuration
Create a prometheus.yml configuration file with the following content:
```
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: "restapi"
    static_configs:
      - targets:
          - "<your_server_ip>:8900"
        labels:
          instance: "server"

      - targets:
          - "<your_server_ip>:9100"
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
## Output Of The Command 
<img width="960" alt="Screenshot 2024-05-19 192516" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/24614fd2-d1a0-4005-8c72-32a70e456e0f">

# Running Grafana with Docker
- Best Reference: https://grafana.com/docs/grafana/latest/setup-grafana/installation/docker/
## Run Grafana via Docker CLI
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
## Output Of The Command 
<img width="668" alt="Screenshot 2024-05-19 192236" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/06d9aafe-4065-4108-8e8f-1c968431ca21">

## Run Grafana via Docker Compose

```
version: '3.8'  # Specifies the version of the Docker Compose file format
services:  # Defines the services that make up the application
  grafana:  # The Grafana service
    image: grafana/grafana:latest  # Uses the latest version of the Grafana image from Docker Hub
    ports:  # Maps ports on the host to ports in the container
      - "3000:3000"  # Maps port 3000 on the host to port 3000 in the container, making the Grafana web interface accessible on port 3000
    environment:  # Sets environment variables inside the container
      GF_SECURITY_ADMIN_USER: admin  # Sets the Grafana admin username to 'admin'
      GF_SECURITY_ADMIN_PASSWORD: admin_password  # Sets the Grafana admin password to 'admin_password'
```
## Accessing the Applications
- Prometheus: Open your browser and navigate to `http://<your_server_ip>:9090` to access the Prometheus web UI.
- Grafana: Open your browser and navigate to `http://<your_server_ip>:3000` to access the Grafana web UI. The default login credentials are admin for both the username and password (By default username=admin, password=admin) Or If you are running through docker-compose then you can set your username or password as we have done it in uper docker compose file.
### Prometheus on `http://<your_server_ip>:9090`
<img width="951" alt="Screenshot 2024-05-19 192441" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/65ae8d08-5a45-485e-8374-bacba8b87332">

### Grafana on `http://<your_server_ip>:3000`

<img width="952" alt="Screenshot 2024-05-19 192002" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/0f7cd845-e562-4c27-bb60-1039859a822d">

---
# node_exporter, postgres_exporter, Redis_exporter and Kafka_exporter Setup Guide



This guide covers the setup of Prometheus and Grafana using Docker, along with specific exporters (`node_exporter` for Node.js servers and `postgres_exporter` for PostgreSQL databases) to collect metrics.

### Node Exporter Setup

Follow the instructions provided in the [Grafana node_exporter documentation](https://grafana.com/docs/grafana-cloud/send-data/metrics/metrics-prometheus/prometheus-config-examples/noagent_linuxnode/) to set up `node_exporter` for monitoring Node.js servers.

### PostgreSQL Exporter Setup

Follow the instructions provided in the [Grafana postgres_exporter documentation](https://grafana.com/oss/prometheus/exporters/postgres-exporter/?tab=installation#step-1-setting-up-postgres-exporter) to set up `postgres_exporter` for monitoring PostgreSQL databases.

### Redis Exporter Setup

Follow the instructions provided in the [Grafana Redis_exporter documentation](https://grafana.com/oss/prometheus/exporters/redis-exporter/?tab=installation) to set up `Redis_exporter` for monitoring Redis databases.

### Kafka Exporter Setup
For this we need to set-up Kafka exporter in docker-compose.yml file

```
version: '3'

networks:
  kafka-net:

services:
  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - 2181:2181
    networks:
      - kafka-net
      
  kafka:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper
    ports:
      - 9092:9092
      - 29092:29092
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:29092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
    networks:
      - kafka-net

  kafka-exporter:
    image: danielqsj/kafka-exporter:latest
    environment:
      KAFKA_SERVER: kafka:29092
    ports:
      - 9308:9308
    depends_on:
      - kafka
    networks:
      - kafka-net

# EXPOSE : 9092
# EXPOSE : 29092
```
See This For More Information Regarding Kafka Exporter: https://stackoverflow.com/a/70599882
### Run this docker-compose file and see the result:

<img width="568" alt="3-container running" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/f10926d5-ff60-4a02-b83a-e2627e7e1e7c">

### Go to `http://<your_server_ip>:9308` where you will able to see the kafka exporter like this:

<img width="640" alt="Screenshot 2024-05-27 010938" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/65d272aa-4238-41c7-a7d7-32b482828ce9">

### And if you click on Metrix then you will able to see the Matrix output like this:

<img width="698" alt="Screenshot 2024-05-27 011010" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/096b31bb-f341-4d3f-acbc-8fb7c77097ca">

# Connecting Grafana to Prometheus
- [1] First open Grafana UI and do logging like as mentioned upper;

- [2] Navigate to Configuration > Data Sources.

  <img width="346" alt="k1" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/083eb83b-3444-45ce-acbe-9ba14fe0cfdc">

- [3] Click Add data source

  <img width="416" alt="k2" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/4d4ca1bb-ea0d-4285-9a38-3f2fbaed4dff">

- [4] You’ll see a number of options here. Prometheus should be close to the top. Hover over it and click Select.
  
  <img width="413" alt="k3" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/6e3bd4ab-3b20-4ef9-8b66-3edbc422e8ed">

- [5] Fill out all the details for your Prometheus server in the form that appears. Then click Save & Test.

   <img width="395" alt="k4" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/8537e681-95f3-4c25-b3b1-47de75d6eebd">

- [6] Once you click Save & Test, you should see a green message bar similar to the one below. If you get an error message, resolve that first before moving on to next steps.

  <img width="475" alt="k5" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/ff453c67-ed1a-4aed-ad08-72c3e1f20e76">
  
- [6] You can now close the page, as you have successfully set up Grafana communication with Prometheus as a data source.

# Setting up Grafana dashboards
- [1] Import the dashboards into Grafana using JSON files. Download all of the dashboards in the jmx-monitoring-stacks repository and save them to a known location in your local system.

  <img width="410" alt="d1" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/312950d4-4f0f-477b-9272-c64c8609bf2d">

- [2] Log in to your Grafana instance from the web browser.

  <img width="470" alt="d2" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/bb7e2cb0-f31e-49df-b047-472c3234ca4e">

- [3] Navigate to Dashboards > Manage.

  <img width="631" alt="d3" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/cafde1c9-c7ff-441d-aae6-3b2b730f222b">

- [4] Click Import.
- [5] Click Upload .json file or give dashboard ID now that we have our dashboards available locally.
- [6] Click Import, and the dashboard will be available for viewing and introspecting your Kafka broker metrics. Yay!
- [7] Import the remaining dashboards, and you’re done!

## NodeExporter Grafana Dashboard 

<img width="762" alt="nodeexp" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/1bab389d-c2dc-42a0-87d6-586495e8bfe5">

## PgsqlExporter Dashboard

<img width="760" alt="pgsqlExpo" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/84ca3ebf-9989-4962-bc32-320154eae2b4">

## KafkaExporter Grafana Dashboard

<img width="506" alt="kafkaExpo" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/596ecafe-616f-421c-974d-0a09dac87cbc">

## Summary

- **Prometheus Configuration File (`prometheus.yml`)**: Defines the targets from which Prometheus will scrape metrics.
- **Prometheus Docker Command**: Runs the Prometheus container with the specified configuration file.
- **Grafana Docker Command**: Runs the Grafana container, storing its data in a persistent volume.
- **Node Exporter**: Monitors Node.js servers.
- **PostgreSQL Exporter**: Monitors PostgreSQL databases.

Ensure that the IP addresses and ports in the Prometheus configuration match the endpoints from which you want to scrape metrics. The Grafana interface will be accessible at `http://<your_server_ip>:3000`.
## Steps

1. **Access Grafana**:
   - Open a browser and navigate to `http://<your_server_ip>:3000`.
   - The default login is `admin` for both the username and password (you will be prompted to change the password upon first login).

2. **Add Prometheus as a Data Source in Grafana**:
   - In Grafana, go to `Configuration` > `Data Sources`.
   - Click `Add data source`, select `Prometheus`, and configure it with the URL `http://localhost:9090` (or the appropriate URL if different).

3. **Create Dashboards**:
   - Use Grafana to create dashboards and panels to visualize the data scraped by Prometheus.

