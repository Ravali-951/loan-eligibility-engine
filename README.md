# Loan Eligibility Engine – SDE Intern Backend Assignment

## Overview

This project implements an automated **Loan Eligibility Engine** that ingests user data, discovers personal loan products from public websites, matches users to eligible loan products, and notifies them via email.

The system is designed using **AWS serverless services** and a **self-hosted n8n workflow automation engine**, with a focus on scalability, cost efficiency, and clean architecture.

---

## Architecture Overview

The system follows an event-driven architecture:

1. User uploads a CSV file through a minimal UI
2. CSV is stored in Amazon S3
3. S3 triggers AWS Lambda for ingestion
4. User data is stored in Amazon RDS (PostgreSQL)
5. n8n workflows orchestrate:
   - Loan product discovery
   - User–loan matching
   - Email notifications
6. AWS SES sends final emails to users

### Architecture Diagram

**Screenshot to add here:**
- `screenshots/architecture-diagram.png`
- Diagram showing: UI → S3 → Lambda → RDS → n8n → SES

(Add architecture diagram image here)


---

## Tech Stack

- **Backend Language:** Python
- **Workflow Engine:** n8n (self-hosted using Docker)
- **Cloud Provider:** AWS
  - S3 – CSV storage
  - Lambda – CSV ingestion
  - RDS (PostgreSQL) – Data storage
  - SES – Email notifications
- **Deployment Tools:** Serverless Framework, Docker Compose
- **Optional AI:** OpenAI / Gemini for eligibility text processing

---

## Repository Structure

loan-eligibility-engine/
│
├── backend/
│   ├── ingest/
│   │   ├── handler.py              # AWS Lambda – CSV ingestion
│   │   └── requirements.txt
│   │
│   ├── db/
│   │   └── schema.sql              # PostgreSQL schema
│   │
│   └── utils/
│       └── db.py                   # Database connection helper
│
├── infrastructure/
│   └── serverless.yml              # AWS resources deployment
│
├── n8n/
│   ├── docker-compose.yml          # Self-hosted n8n setup
│   └── workflows/
│       ├── workflow_a_discovery.json
│       ├── workflow_b_matching.json
│       └── workflow_c_notification.json
│
├── ui/
│   └── index.html                  # CSV upload UI
│
├── README.md
└── .gitignore


---

## Database Schema

The PostgreSQL database contains three main tables:

- `users` – stores uploaded user data
- `loan_products` – stores scraped loan product details
- `matches` – stores successful user–loan matches

**Screenshot to add here:**
- `screenshots/postgres-tables.png`
- Show tables in PostgreSQL

(Add PostgreSQL tables screenshot here)



---

## Scalable CSV Ingestion

### Flow

1. User uploads CSV via UI
2. CSV is stored in S3
3. S3 event triggers Lambda
4. Lambda parses CSV and inserts data into RDS

This avoids API Gateway size/time limits and ensures scalability.

**Screenshot to add here:**
- `screenshots/s3-upload.png`
- `screenshots/lambda-trigger.png`
- `screenshots/users-table-data.png`

(Add S3 upload screenshot here)
(Add Lambda trigger screenshot here)
(Add users table data screenshot here)



---

## n8n Workflow Automation

All core business logic is orchestrated using **n8n**.

### Workflow A – Loan Product Discovery (Web Crawler)

**Trigger:** Cron (daily)

**Steps:**
- Crawl public financial websites
- Extract loan product data
- Normalize eligibility criteria
- Store results in `loan_products` table

**Screenshot to add here:**
- `screenshots/n8n-workflow-a.png`
- `screenshots/loan-products-table.png`


(Add Workflow A screenshot here)
(Add loan_products table screenshot here)


---

### Workflow B – User–Loan Matching (Eligibility Filter)

**Trigger:** Webhook (after CSV ingestion)

#### Optimization Strategy (Multi-Stage Filtering)

**Stage 1 – SQL Pre-filter**
- Filter users by income and credit score directly in SQL

**Stage 2 – Workflow Logic**
- Apply additional rules in n8n

**Stage 3 – Optional AI Check**
- LLM used only for complex qualitative rules

This significantly reduces cost and execution time.

**Screenshot to add here:**
- `screenshots/n8n-workflow-b.png`
- `screenshots/matches-table.png`

(Add Workflow B screenshot here)
(Add matches table screenshot here)



---

### Workflow C – User Notification

**Trigger:** After matching workflow completion

**Steps:**
- Group matched loans per user
- Generate personalized email content
- Send emails using AWS SES

**Screenshot to add here:**
- `screenshots/n8n-workflow-c.png`
- `screenshots/email-received.png`

(Add Workflow C screenshot here)
(Add received email screenshot here)



---

## User Interface

A minimal HTML interface is provided for CSV upload.

**Screenshot to add here:**
- `screenshots/ui-upload-page.png`

(Add UI upload page screenshot here)


---

## Setup Instructions

### 1. Clone Repository

```bash
git clone <repository-url>
cd loan-eligibility-engine

### 2. Deploy AWS Resources

cd infrastructure
serverless deploy



