# AWS Microservices Deployment, Stress-Testing & Monitoring Stack

[![AWS](https://img.shields.io/badge/AWS-EC2%20%2F%20Elastic%20IP-232F3E?logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-Dashboards-F46800?logo=grafana&logoColor=white)](https://grafana.com/)

An end-to-end cloud infrastructure project focusing on the deployment, automated stress-testing, monitoring, and elasticity scaling of the multi-container **Sock Shop** microservices application on AWS EC2.

Developed as part of the **Services & Cloud Technologies (SCT)** Master 1 module at the University of Rennes.

---

## Demo Screencast & Documentation

* **Demo Video:** [Watch the Demo Video](https://www.loom.com/share/a2e96f053e4642f4bbf0620445ef047c)
* **Full Report:** See detailed documentation in [`/docs/Report.pdf`](./docs/Report.pdf)

---

## Overview

This project sets up and tests the **Sock Shop** microservices application on AWS EC2. 

The stack includes:
* 7 microservices deployed using Docker Compose behind Traefik.
* Prometheus and Grafana for server and container metrics.
* An automated load generator (`user-sim`) to stress-test the deployment.

---

## What was done

* **Fixed resource constraints:** The stack crashed on `t3.small` instances during startup due to memory limits, so we migrated to `c7i-flex.large` for stable execution.
* **Automated stress testing:** Used the built-in `user-sim` container to generate continuous traffic through Traefik.
* **Monitoring:** Created Grafana dashboards using PromQL to track QPS, CPU, and RAM usage.
* **Elasticity & Resilience:** Configured container auto-restarts (`restart: always`) and tested horizontal scaling (`docker-compose scale edge-router=3`) to absorb traffic spikes.

---

## Tech Stack & Tools

* **Cloud Platform:** AWS EC2, Elastic IP, AWS Security Groups
* **Containers & Orchestration:** Docker, Docker Compose, Traefik
* **Monitoring & Metrics:** Prometheus, Grafana, Alertmanager, Node Exporter
* **Stress-Testing:** Locust (`user-sim`)
* **Operating System:** Ubuntu Server 22.04 LTS

---

## Deployment Guide

### 1. Prerequisites & EC2 Provisioning
Provision an Ubuntu EC2 instance on AWS, attach an **Elastic IP**, and open the following Security Group ports:
* `80` (Sock Shop Frontend)
* `8080` (Traefik Health Dashboard)
* `3000` (Grafana)
* `9090` (Prometheus)

### 2. Deploy Microservices
```bash
# Clone the repository and navigate to deployment directory
git clone [https://github.com/ImThe404/aws-microservices-monitoring.git](https://github.com/ImThe404/aws-microservices-monitoring.git)
cd aws-microservices-monitoring/deploy

# Launch the core microservices stack
sudo docker-compose up -d
```

### 3. Launch Monitoring Stack
```bash
# Start Prometheus, Grafana, and Alertmanager
sudo docker-compose -f docker-compose.monitoring.yml up -d
```

### 4. Trigger Automated Load Generator
```bash
# Run the user simulator to generate stress-test traffic
sudo docker-compose up -d user-sim
```

### 5. Horizontal Scaling Command
```bash
sudo docker-compose up -d --scale edge-router=3
```

## Author
* Jonathan Aussant
* Abdullah Shahid