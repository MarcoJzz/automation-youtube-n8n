# Hybrid Video Publishing Pipeline (Cloud-to-Local)

An automated backend pipeline that coordinates digital content publishing through a hybrid architecture. It connects a cloud-based orchestrator with a local system agent to handle scheduling, content distribution, and automated uploads.

## 🚀 Architecture Overview

The system is designed to bypass the limitations of cloud-hosted web automation by splitting tasks into a decoupled, hybrid execution model:

1. **Orchestration Layer (Cloud):** A self-hosted instance of **n8n** running inside **Docker** behind a **Caddy** reverse proxy handles logic scheduling, Webhook management, and data mapping.
2. **Data & Storage Layer:** Integrated with **Google Cloud Platform (GCP)** using a secure **Service Account** to programmatically fetch files from Google Drive and coordinate pipeline logs.
3. **Execution Layer (Local OS):** A decoupled local script executes OS-level processes and schedules publishing routines via browser automation (**Playwright/Python**).

## 🛠️ Tech Stack

* **Infrastructure:** Docker, Docker Compose, Linux VPS (DigitalOcean Droplet), Caddy Proxy (Automated TLS/HTTPS)
* **Workflow Orchestration:** n8n (Advanced node-based logic & JavaScript data manipulation)
* **APIs:** Google Cloud IAM (Service Accounts), Google Drive API, Airtable Backend (Database logs)
* **Automation Agent:** Python, Playwright, Node.js (Child Processes)

## 📂 Repository Structure

```text
├── docker-compose.yml     # Self-hosted n8n & Caddy infrastructure setup
├── .env.example           # Secure template for critical environment variables
├── .gitignore             # Strict patterns protecting service account keys & media binaries
├── WorkflowN8n.json       # Production-ready n8n workflow pipeline export
└── README.md              # Project documentation
⚙️ Setup and Installation
1. Cloud Infrastructure
To spin up the orchestration server, configure your .env following .env.example and deploy via Docker Compose:

Bash
docker compose up -d
2. Google Cloud Authentication
Instead of relying on standard human-based OAuth2 tokens that expire frequently, this pipeline relies on a machine-to-machine GCP Service Account.

Generate a secure JSON Private Key in your GCP console.

Share your specific Google Drive content folders with the generated bot email as an Editor.

Inject the private key strings natively into the n8n credential manager.

🎯 Highlights & Solutions
Incremental Scheduling Logic: Dynamically calculates publishing offsets and time slots through inline JavaScript data transformations based on task queues rather than hardcoded timestamps.

Security First: Explicitly uses service account structures to isolate data access, granting zero peripheral permissions to personal accounts.

Anti-Fingerprinting Strategy: Offloads browser instance rendering to local execution pipelines, mitigating data center IP blocklists typically enforced by major media networks.