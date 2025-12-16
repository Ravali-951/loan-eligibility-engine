# Loan Eligibility Engine – SDE Intern Backend Assignment

## Overview

The Loan Eligibility Engine is an automated backend system that ingests user data, discovers personal loan products from public websites, matches users with eligible loan products, and notifies them via email.  

The system is designed using a **serverless, event-driven architecture** on AWS and a **self-hosted n8n workflow engine** to orchestrate all business logic.

This project demonstrates skills in:
- Backend engineering
- Cloud architecture (AWS)
- Workflow automation (n8n)
- Data processing and optimization
- Clean, scalable system design

---

## Architecture Diagram

![Architecture Diagram](screenshots/architecture.png)

**Flow Explanation:**
1. User uploads a CSV file via a minimal UI.
2. CSV is stored in Amazon S3.
3. S3 event triggers an AWS Lambda function.
4. Lambda parses CSV and stores user data in PostgreSQL (RDS).
5. n8n workflows handle loan discovery, matching, and notifications.
6. AWS SES sends emails to matched users.

---

## Tech Stack

### Backend & Cloud
- Python (AWS Lambda)
- AWS S3 (CSV storage)
- AWS Lambda (CSV ingestion)
- AWS RDS (PostgreSQL)
- AWS SES (Email notifications)
- Serverless Framework

### Automation & AI
- n8n (Self-hosted via Docker)
- OpenAI / Gemini API (optional optimization layer)

### Database
- PostgreSQL

---

## Repository Structure

loan-eligibility-engine/
│
├── backend/
│ ├── ingest/
│ │ ├── handler.py
│ │ └── requirements.txt
│ │
│ ├── db/
│ │ └── schema.sql
│ │
│ └── utils/
│ └── db.py
│
├── infrastructure/
│ └── serverless.yml
│
├── n8n/
│ ├── docker-compose.yml
│ └── workflows/
│ ├── loan_product_discovery.json
│ ├── user_loan_matching.json
│ └── user_notification.json
│
├── ui/
│ └── index.html
│
├── screenshots/
│ ├── architecture.png
│ ├── n8n_workflow_a.png
│ ├── n8n_workflow_b.png
│ ├── n8n_workflow_c.png
│ ├── csv_upload.png
│ └── email_output.png
│
├── README.md
└── .gitignore



---

## Database Schema

![Database Schema](screenshots/db_schema.png)

### Tables
- **users** – stores uploaded user data
- **loan_products** – stores discovered loan products
- **matches** – stores user-loan eligibility results

---

## Setup Instructions


Modular architecture to support future enhancements.
