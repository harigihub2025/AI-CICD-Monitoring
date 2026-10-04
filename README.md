# 🚀 AI-Powered CI/CD Monitoring Platform

A production-style DevOps project demonstrating automated CI/CD,
containerized application deployment, AWS cloud infrastructure,
and real-time monitoring.

---

## 📌 Project Overview

The AI-Powered CI/CD Monitoring Platform automates the complete
application delivery workflow from source-code management to
cloud deployment and infrastructure monitoring.

The project uses GitHub for source control, Jenkins for CI/CD,
Docker for containerization, AWS EC2 for cloud infrastructure,
Nginx for application serving, and Prometheus + Grafana for
real-time infrastructure monitoring.

---

## 🏗️ Architecture

```text
                    ┌───────────────┐
                    │    GitHub     │
                    │ Source Code   │
                    └───────┬───────┘
                            │
                         git push
                            │
                            ▼
                    ┌───────────────┐
                    │    Jenkins    │
                    │    CI / CD    │
                    └───────┬───────┘
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
          Docker Build             Application Test
                 │                     │
                 └──────────┬──────────┘
                            │
                       SSH Deployment
                            │
                            ▼
              ┌──────────────────────────┐
              │         AWS EC2          │
              │                          │
              │  ┌────────────────────┐  │
              │  │       Nginx        │  │
              │  │        :80         │  │
              │  └────────────────────┘  │
              │                          │
              │      Docker Compose      │
              │                          │
              │  Prometheus :9090        │
              │  Grafana    :3000        │
              │  Node Exporter :9100     │
              └────────────┬─────────────┘
                           │
                           ▼
                    Infrastructure
                      Monitoring
