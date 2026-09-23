# 🏠 Aminul Real Estate — Automated Lead Management & Property Viewing System

> End-to-end CRM automation built with GoHighLevel, n8n, REST API, Webhooks, and Google Gemini AI.

## 📌 Project Overview

This project demonstrates an automated real estate lead management system designed to handle the customer journey from initial lead capture through qualification, property-viewing booking, and sales pipeline progression.

The system uses **GoHighLevel (GHL)** for CRM operations, sales pipeline management, workflows, follow-up, calendars, and appointments, while **n8n** handles validation, deduplication logic, advanced processing, API integration, and AI-powered lead intent assessment.

### 🔄 High-Level Flow

**Lead Form → GHL Contact → Opportunity → GHL Workflow → n8n → Validation → Deduplication → Budget Qualification → AI Intent Assessment → GHL Update → Property Viewing → Appointment → Pipeline Update**

## 🎯 Business Problems Addressed

- Manual lead management
- Slow lead follow-up
- Inconsistent lead qualification
- Duplicate-lead handling
- Poor sales pipeline visibility
- Manual property-viewing scheduling
- Disconnected CRM and appointment processes

## 📊 CRM Sales Pipeline

The CRM uses a structured sales pipeline to track each lead throughout the real estate sales journey.

### Pipeline Stages

**New Lead → Contacted → Qualified → Property Viewing → Negotiation → Closed**

Each stage represents a clear step in the sales process:

- **New Lead** — A newly captured inquiry enters the CRM.
- **Contacted** — The sales team has started communication with the lead.
- **Qualified** — The lead meets the required qualification criteria.
- **Property Viewing** — A property-viewing appointment has been scheduled.
- **Negotiation** — The lead attended the viewing and progressed to negotiation.
- **Closed** — Final stage for the completed sales process.

### 📸 Pipeline Overview

![Aminul Real Estate Sales Pipeline](docs/screenshots/01-aminul-real-estate-sales-pipeline.png)

## 📝 Lead Capture & CRM Data

A dedicated GoHighLevel form captures the information required to process and qualify a real estate lead.

### Lead Information Collected

- First Name
- Last Name
- Phone
- Email
- Property Interest
- Budget
- Preferred Location
- Message

The real-estate-specific information is stored in custom CRM fields so it can be used later by GHL Workflows, n8n business logic, and AI qualification.

### Lead Capture Process

**Website / Campaign → GHL Form → Contact Record → Opportunity → Sales Pipeline**

After a successful form submission, the system can:

- Create or update the Contact
- Store property requirements in CRM fields
- Add the lead tag
- Create an Opportunity
- Place the Opportunity in the **New Lead** stage
- Start the lead-processing Workflow

### 📸 Lead Capture Form

![Aminul Real Estate Lead Capture Form](docs/screenshots/03-aminul-real-estate-lead-capture-form.png)

## ⚙️ GoHighLevel Lead Capture & Qualification Workflow

The main GoHighLevel Workflow orchestrates the initial CRM automation after a lead submits the real estate inquiry form.

### Workflow Trigger

**Form Submitted → Aminul Real Estate Lead Form**

### Automation Sequence

1. **Add Contact Tag** — Identifies the contact as a real estate lead.
2. **Create Opportunity** — Creates an Opportunity in the Sales Pipeline at the **New Lead** stage.
3. **Send Lead to n8n** — Sends the lead data securely to the external processing workflow.
4. **Internal Notification** — Notifies the team about the new inquiry.
5. **Confirmation Email** — Sends an acknowledgement to the lead.
6. **Wait** — Controls follow-up timing.
7. **Budget Condition** — Evaluates the lead's budget.
8. **Pipeline / Follow-Up Action** — Qualified leads can progress while other leads follow the appropriate follow-up path.

### Duplicate & Re-entry Protection

The Workflow is configured to reduce unintended repeated processing:

- **Allow Re-entry:** Disabled
- **Allow Multiple Opportunities:** Disabled
- **Stop on Response:** Enabled

This helps prevent repeated messages and unnecessary duplicate Opportunities when the same Contact interacts with the automation again.

### 📸 Main GHL Workflow

![GoHighLevel Lead Capture and Qualification Workflow](docs/screenshots/02-aminul-real-estate-lead-capture-qualification-workflow.png)
