---
title: System Architecture
---

# System Architecture

IddirNet is a mobile-first platform (Android app for members/leaders, web dashboard for admins) built to enhance transparency and efficiency in Ethiopian Iddir communities. It uses a layered architecture: Frontend (React Native for mobile, React for web), Backend (Node.js/Express REST APIs), Database (PostgreSQL), and external integrations for payments and locations.

## Overview
The system follows a client-server model with role-based access (members, leaders, admins). Core modules include:
- **Inventory Tracking**: Manages resource lifecycle (owned, borrowed, rented) with real-time status updates.
- **Payment Management**: Processes fees/fines/rentals via Daraja API, generates digital receipts.
- **Member Management**: Profiles, histories, and status tracking.
- **Authentication**: JWT with RBAC for secure access.

Data flows through secure REST APIs to PostgreSQL, with logging for auditing. Scalable for thousands of users, with encryption and compliance.

## Architecture Diagram
![System Architecture Diagram](images/architecture-diagram.png)
<!-- Upload your diagram (e.g., Draw.io: Frontend → API Gateway → Services → PostgreSQL + External APIs) to docs/images/architecture-diagram.png -->

### Key Components
| Component | Description | Technologies |
|-----------|-------------|--------------|
| **Frontend (Mobile)** | User interfaces for members/leaders (payments, resources, notifications). | React Native (Android) |
| **Frontend (Admin Dashboard)** | Web-based oversight for multi-Iddir management and reports. | React + Material-UI |
| **Backend Services** | Handles CRUD for modules; integrates Daraja/LocationIQ. | Node.js/Express, JWT Auth |
| **Database** | Stores users, payments, resources, logs; supports analytics. | PostgreSQL (with pgAdmin for schema) |
| **External APIs** | Payments (Daraja) and Geocoding (LocationIQ). | RESTful calls via Axios |
| **Notifications** | Automated reminders for payments/resources. | Twilio/SendGrid |

### Data Flow Summary
Based on user journeys:

| User Type | Journey | Flow |
|-----------|---------|------|
| **Member** | Registration/Login → Payment → Resource Request | App → Auth API → DB (validate/join) → Daraja (pay) → Receipt Generation → LocationIQ (map pickup). |
| **Leader** | Login → Track Contributions → Manage Resources | App → API (fetch members/payments) → Approve Requests → Update Inventory DB → Notify via SMS. |
| **Admin** | Dashboard Login → Oversight → Reporting | Web → MFA Auth → Analytics Query (DB) → Generate Reports → Mediate Grievances. |

All interactions use encrypted HTTPS; logs ensure auditability.

For data models, see [ERD](erd.md). Integrations detailed in [Integrations](integrations.md).

---

<div class="admonition tip">
  <p class="admonition-title">Pro Tip</p>
  <p>Deploy via Heroku/Docker; use Redis for caching high-traffic queries like payment histories.</p>
</div>