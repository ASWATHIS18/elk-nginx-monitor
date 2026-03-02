# ELK Stack – Nginx Log Monitor

A production-style centralized logging architecture built using the ELK Stack to monitor Nginx access and error logs in real-time.

This project simulates a real-world DevOps logging pipeline using Docker containers.

---

# Architecture Overview

Nginx → Filebeat → Elasticsearch → Kibana

### Components

- Nginx
  - Generates access and error logs
  - Exposed on port 80

- Filebeat
  - Reads logs from Nginx
  - Uses Nginx module for structured parsing
  - Ships logs to Elasticsearch

- Elasticsearch
  - Stores structured log data
  - Provides indexing and search capabilities
  - Runs in single-node mode

-Kibana
  - Connects to Elasticsearch
  - Visualizes logs
  - Used for dashboards and analysis

---

# Tech Stack

- Docker
- Docker Compose
- Nginx (latest)
- Elasticsearch 8.12.0
- Filebeat 8.12.0
- Kibana 8.12.0
- ECS (Elastic Common Schema)

---

# Project Structure

```
elk-nginx-monitor/
│
├── docker-compose.yml
├── README.md
│
├── nginx/
│   └── logs/
│       ├── access.log
│       └── error.log
│
├── filebeat/
│   └── filebeat.yml
│
├── elasticsearch/
│   └── elastic.yml
│
└── kibana/
    └── kibana.yml
```

---

# How It Works

1. Nginx writes logs to `/var/log/nginx`
2. Docker volume maps logs to local `nginx/logs/`
3. Filebeat monitors log files
4. Filebeat parses logs using ingest pipelines
5. Logs are stored in Elasticsearch index: `filebeat-*`
6. Kibana reads index and visualizes logs

---

# How To Run

## Prerequisites

- Docker installed
- Docker Compose installed

Check:

```bash
docker --version
docker compose version
```

---

## Clone Repository

```bash
git clone https://github.com/ASWATHIS18/elk-nginx-monitor.git
cd elk-nginx-monitor
```

---

## Start ELK Stack

```bash
docker compose up -d
```

Check running containers:

```bash
docker ps
```

You should see:

- nginx
- elasticsearch
- kibana
- filebeat

---

## Wait For Initialization

Wait 30–60 seconds for Elasticsearch and Kibana to fully start.

You can monitor logs:

```bash
docker logs elasticsearch
docker logs kibana
```

---

## Generate Traffic

Open browser:

```
http://localhost
```

OR run:

```bash
for i in {1..20}; do curl http://localhost; done
```

This generates access logs.

---

## Verify Elasticsearch

Open:

```
http://localhost:9200
```

You should see JSON response.

---

## Open Kibana

```
http://localhost:5601
```

---

## Create Data View (First Time Only)

1. Go to Stack Management
2. Click Data Views
3. Create Data View
4. Enter:

```
filebeat-*
```

5. Select time field: `@timestamp`
6. Save

---

## View Logs

Go to:

```
Discover
```

Select time filter → Last 15 minutes

You will see structured fields:

- http.response.status_code
- http.request.method
- nginx.access.remote_ip_list
- url.original

---

# Monitoring Capabilities

This stack enables:

- Real-time log streaming
- Status code analysis
- HTTP method tracking
- Client IP tracking
- Request trend monitoring
- Error monitoring (4xx, 5xx)

---

# Stop The Stack

Stop containers:

```bash
docker compose down
```

Remove volumes (clean reset):

```bash
docker compose down -v
```

---

# Troubleshooting

### Issue: Filebeat not connecting

Check:

```bash
docker logs filebeat
```

### Issue: Elasticsearch not reachable

Check:

```bash
docker logs elasticsearch
```

### Issue: No logs in Kibana

- Ensure traffic is generated
- Check time filter (Last 15 minutes)
- Verify index exists:

```bash
curl http://localhost:9200/_cat/indices?v
```

---
# Learning Outcomes

This project demonstrates:

- Centralized logging architecture
- Containerized microservices setup
- Log parsing using Filebeat modules
- Elasticsearch indexing
- Kibana visualization
- Docker networking & volume mapping
- Git workflow & repository management

---

# Author

Aswathi Selvam
GitHub: https://github.com/ASWATHIS18
