# BulkUploader Architecture Diagram

## App Services (Avalon Bulk Upload Platform)

Avalon’s BulkUploader solution is centered around robust Azure App Services, ensuring secure, scalable, and reliable bulk upload operations.

### App Services Overview

1. **acc-bulkuploader**: Main Avalon bulk upload service  
   - **Security**: All endpoints are protected using Azure Active Directory authentication and HTTPS enforced for secure data transmission. Role-based access controls (RBAC) restrict actions based on user roles.
   - **Logging & Monitoring**: Integrated with Application Insights for real-time telemetry, audit logs, and error tracking.
   - **Authentication**: Supports OAuth2/OIDC for Avalon employees and partners.
   - **Slots**:
     - **avalon**: Dedicated Avalon staging slot for pre-production testing and validation. Used for Avalon’s internal QA and deployment previews.
     - **acp**: A/B testing or blue-green deployment slot for Avalon rollouts and experiments.
2. **acc-bulkuploaderapi**: Avalon API service for bulk upload operations  
   - **Security**: API endpoints are protected via managed identities and IP whitelisting for Avalon’s core systems.
   - **Logging & Monitoring**: API telemetry is streamed to Avalon dashboards for visibility and compliance.
   - **Slots**:
     - **v6**: Latest Avalon API deployment slot supporting enhanced features and integration.

Avalon’s deployment strategy leverages App Service slots, allowing seamless updates and staged releases that minimize downtime and risk.

---

## System Overview

The BulkUploader system for Avalon consists of multiple Azure resources including App Services, Application Insights, and supporting monitoring solutions for handling secure, scalable bulk upload operations.

## Architecture Diagram

```mermaid
graph TB
    subgraph "Application Services (Avalon)"
        AS1[acc-bulkuploader<br/>App Service]
        AS2[acc-bulkuploaderapi<br/>App Service]
        AS3[acc-bulkuploader/avalon<br/>App Service Slot]
        AS4[acc-bulkuploader/acp<br/>App Service Slot]
        AS5[acc-bulkuploaderapi/v6<br/>App Service Slot]
    end

    subgraph "Monitoring & Analytics"
        FA1[FailureAnomalies-acc-bulkuploader<br/>Smart detector alert rule]
        FA2[FailureAnomalies-acc-bulkuploaderapi<br/>Smart detector alert rule]
        SD[1f35c3a9-fa1b-4384-bd4c-86b-a9c2cfd30-dashboard<br/>Shared dashboard]
    end

    subgraph "Application Insights"
        AI1[acc-bulkuploader<br/>Application Insights]
        AI2[acc-bulkuploaderapi<br/>Application Insights]
    end

    %% Relationships
    FA1 --> AI1
    FA2 --> AI2
    SD --> AI1
    SD --> AI2

    AI1 --> AS1
    AI1 --> AS3
    AI1 --> AS4

    AI2 --> AS2
    AI2 --> AS5

    AS1 --> AS3
    AS1 --> AS4
    AS2 --> AS5
Component Details
Smart Detector Alert Rules
FailureAnomalies-acc-bulkuploader: Monitors Avalon’s main bulk uploader service for anomalies
FailureAnomalies-acc-bulkuploaderapi: Monitors the Avalon API service for anomalies
Shared Dashboard
1f35c3a9-fa1b-4384-bd4c-86b-a9c2cfd30-dashboard: Centralized monitoring dashboard for all Avalon services
Application Insights
acc-bulkuploader: Telemetry and monitoring for Avalon’s main service, including security events and performance metrics
acc-bulkuploaderapi: Telemetry and monitoring for Avalon’s API service
Data Flow
Both Avalon App Services are monitored by their respective Application Insights instances
Smart detector alert rules analyze the Application Insights data for operational or security anomalies
The shared dashboard aggregates monitoring data from both Application Insights instances for Avalon teams
App Service slots allow for staged deployments, testing, and secure releases
Key Architectural Patterns
Microservices: Separate Avalon services for core functionality and API, enabling modular development
Blue-Green Deployment: Multiple Avalon slots for staged deployments
Monitoring First: Comprehensive monitoring with Application Insights and smart alerts, supporting Avalon’s compliance needs
Security First: All Avalon endpoints require authentication, authorization, and encrypted communication
Separation of Concerns: API layer separated from bulk processing for improved security and scalability
