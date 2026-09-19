# Kubernetes Application Observability with Prometheus

A hands-on observability project that monitors a containerized Node.js application running on Kubernetes using custom Prometheus metrics, ServiceMonitor, and PromQL.

## Project Overview

This project implements application-level monitoring for a Node.js application deployed on Kubernetes.

The application exposes custom metrics through a `/metrics` endpoint, which are automatically discovered and scraped by Prometheus using a Kubernetes `ServiceMonitor`.

The collected metrics can then be queried using PromQL to monitor application traffic and performance.

## Architecture

```text
Node.js Application
        │
        │ /metrics
        ▼
Kubernetes Service
        │
        │ ServiceMonitor
        ▼
Prometheus
        │
        │ PromQL
        ▼
Application Metrics
```

## Technologies Used

* Kubernetes
* Amazon EKS
* Docker
* Node.js
* Prometheus
* PromQL
* ServiceMonitor
* YAML

## Implementation

### 1. Application Instrumentation

Instrumented the Node.js application with custom Prometheus metrics.

Implemented:

* `http_requests_total` — tracks HTTP requests
* `http_request_duration_seconds` — tracks request latency using Histogram
* `http_request_duration_summary_seconds` — tracks request latency using Summary
* `node_gauge_example` — demonstrates Gauge metrics

The application exposes these metrics through:

```text
/metrics
```

### 2. Containerization

Created a Docker image for the Node.js application and deployed the containerized application to Kubernetes.

### 3. Kubernetes Deployment

Created Kubernetes resources for the application:

* Deployment
* Service
* Service configuration for exposing the application

Verified the application using Kubernetes:

```bash
kubectl get pods -n dev
kubectl get svc -n dev
```

### 4. Prometheus ServiceMonitor

Configured a `ServiceMonitor` to allow Prometheus to discover and scrape the application's `/metrics` endpoint.

The ServiceMonitor connects:

```text
Kubernetes Service → /metrics → Prometheus
```

### 5. Prometheus Monitoring

Verified that Prometheus successfully discovered the application and started collecting custom application metrics.

Example metric:

```promql
http_requests_total
```

### 6. PromQL Monitoring

Used PromQL to analyze application traffic.

Example:

```promql
sum by (path) (rate(http_requests_total[1m]))
```

This shows the request rate for each application endpoint.

## Result

The application was successfully deployed on Kubernetes and connected to Prometheus for application-level monitoring.

The implementation demonstrates how custom application metrics can be exposed, discovered through Kubernetes, collected by Prometheus, and analyzed using PromQL.

## Key Project Flow

```text
Application Instrumentation
        ↓
Docker Container
        ↓
Kubernetes Deployment
        ↓
Kubernetes Service
        ↓
ServiceMonitor
        ↓
Prometheus Scraping
        ↓
PromQL Queries
        ↓
Application Observability
```
