# BulkUploader Architecture Diagram

## System Overview

The BulkUploader system consists of multiple Azure resources including App Services, Application Insights, and supporting services for handling bulk upload operations.

## Architecture Diagram

```mermaid
graph TB
    subgraph "Monitoring & Analytics"
        FA1[FailureAnomalies-acc-bulkuploader<br/>Smart detector alert rule]
        FA2[FailureAnomalies-acc-bulkuploaderiapi<br/>Smart detector alert rule]
        SD[1f35c3a9-fa1b-4384-bd4c-86b-a9c2cfd30-dashboard<br/>Shared dashboard]
    end

    subgraph "Application Services"
        AS1[acc-bulkuploader<br/>App Service]
        AS2[acc-bulkuploaderapi<br/>App Service]
        AS3[acc-bulkuploader/avalon<br/>App Service Slot]
        AS4[acc-bulkuploader/acp<br/>App Service Slot]
        AS5[acc-bulkuploaderapi/v6<br/>App Service Slot]
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
```

## Component Details

### Smart Detector Alert Rules
- **FailureAnomalies-acc-bulkuploader**: Monitors the main bulk uploader service for anomalies
- **FailureAnomalies-acc-bulkuploaderapi**: Monitors the API service for anomalies

### Shared Dashboard
- **1f35c3a9-fa1b-4384-bd4c-86b-a9c2cfd30-dashboard**: Centralized monitoring dashboard for all services

### App Services
1. **acc-bulkuploader**: Main bulk upload service
   - **Slots**:
     - avalon: Likely a staging or testing slot
     - acp: Another deployment slot (possibly for A/B testing or blue-green deployment)

2. **acc-bulkuploaderapi**: API service for bulk upload operations
   - **Slots**:
     - v6: Version 6 deployment slot

### Application Insights
- **acc-bulkuploader**: Telemetry and monitoring for the main service
- **acc-bulkuploaderapi**: Telemetry and monitoring for the API service

## Data Flow

1. Both App Services are monitored by their respective Application Insights instances
2. Smart detector alert rules analyze the Application Insights data for anomalies
3. The shared dashboard aggregates monitoring data from both Application Insights instances
4. App Service slots allow for staged deployments and testing

## Key Architectural Patterns

- **Microservices**: Separate services for main functionality and API
- **Blue-Green Deployment**: Multiple slots for staged deployments
- **Monitoring First**: Comprehensive monitoring with Application Insights and smart alerts
- **Separation of Concerns**: API layer separated from main processing service