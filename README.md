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

## 🔗 n8n Integration & Lead Processing

GoHighLevel handles the CRM and customer-facing business process, while n8n handles the more advanced integration and processing logic.

The GHL Workflow sends lead data to an authenticated n8n Webhook. n8n then processes the lead before writing qualification results back to GoHighLevel through the REST API.

### Processing Flow

**GHL Workflow → Authenticated Webhook → Normalize Data → Search Existing Contact → Deduplication Check → Validation → Budget Qualification → AI Intent Assessment → GHL API Update**

### n8n Responsibilities

✅ **Data Normalization**  
Incoming GHL data is mapped into a consistent structure for downstream processing.

✅ **Existing Contact Lookup**  
The workflow searches GoHighLevel for the corresponding Contact before continuing.

✅ **Deduplication Logic**  
Incoming and existing Contact information is compared to reduce unintended duplicate processing.

✅ **Validation**  
Required values such as Contact ID, email, and valid budget data are checked before qualification.

✅ **Deterministic Qualification**  
Budget qualification is handled with explicit business rules rather than relying on AI.

Example classification:

- Budget ≥ 5,000,000 → **High Budget**
- Budget below threshold → **Standard Budget**

✅ **AI Intent Assessment**  
Google Gemini analyzes the lead's inquiry and classifies intent as:

- Strong Intent
- Moderate Intent
- Weak Intent

AI is used for interpreting lead intent, while deterministic business rules remain responsible for budget qualification.

✅ **CRM Write-Back**  
The final qualification and AI assessment are written back to the corresponding GoHighLevel Contact through the REST API.

### 📸 n8n Lead Processing Workflow

![n8n Lead Processing Workflow](docs/screenshots/04-aminul-real-estate-n8n-lead-processing-workflow.png)

## 🤖 AI Qualification & CRM Write-Back

After validation and rule-based qualification, the lead is analyzed for purchase intent using Google Gemini.

The AI assessment is intentionally separated from deterministic business rules:

- **Budget Qualification** → Rule-based logic
- **Lead Intent Assessment** → AI analysis

This keeps critical business decisions predictable while using AI where interpretation of unstructured lead messages is useful.

### Example Processed Lead

For a successfully processed test lead:

- **Budget:** 6,500,000
- **Lead Qualification:** High Budget
- **AI Lead Assessment:** Strong Intent
- **Pipeline Progression:** New Lead → Qualified

The qualification and AI assessment are written back to custom fields on the GoHighLevel Contact record.

This allows the sales team to see the processed lead intelligence directly inside the CRM without opening n8n.

### 📸 AI Qualification Result in GoHighLevel

![AI Qualification and CRM Update](docs/screenshots/05-aminul-real-estate-ai-qualification-crm-update.png)

## 📅 Property Viewing Calendar & Appointment Automation

Qualified leads can schedule a property viewing through a dedicated GoHighLevel booking calendar.

### Calendar Configuration

The **Aminul Real Estate - Property Viewing** calendar provides:

- 30-minute property-viewing appointments
- Available date and time-slot selection
- Customer self-booking
- Appointment data connected directly to the CRM

This removes unnecessary back-and-forth communication when scheduling property viewings.

### 📸 Property Viewing Booking Calendar

![Property Viewing Booking Calendar](docs/screenshots/06-aminul-real-estate-property-viewing-booking-calendar.png)

### Booking Automation

When a customer books a property viewing, a dedicated GHL Workflow handles the appointment process:

**Customer Booked Appointment → Confirmation Email → Find Opportunity → Update Opportunity → Wait → Reminder Email**

The workflow searches for the lead's existing open Opportunity before updating the sales process.

If the Opportunity is found:

1. The Opportunity is moved to **Property Viewing**
2. The customer receives booking confirmation
3. The workflow waits until the configured reminder time
4. A property-viewing reminder is sent before the appointment

If no matching Opportunity is found, the workflow ends without creating an unintended duplicate Opportunity.

### 📸 Property Viewing Automation

![Property Viewing Appointment Automation](docs/screenshots/07-aminul-real-estate-property-viewing-automation.png)

