# BulkUploader Migration Meeting Questions
**Date:** July 17, 2025  
**Purpose:** Gather requirements for recreating BulkUploader web app in new Azure tenant

## 1. Architecture & Components

### Application Structure
- [ ] Can you confirm the current architecture is Angular frontend + C# API backend?
- [ ] What version of Angular is being used?
- [ ] What .NET version is the C# API running on? (.NET Core, .NET 5/6/7/8?)
- [ ] Are there any other components not visible in the resource visualizer?

### App Service Configuration
- [ ] What is the current App Service Plan SKU/tier (Standard, Premium, etc.)?
- [ ] What are the specific configurations for:
  - `acc-bulkuploader` (main app service)
  - `acc-bulkuploaderapt` (API app service)
- [ ] Why are the frontend and API deployed as separate App Services?
- [ ] Are there any custom domains configured?

### Deployment Slots
- [ ] What is the purpose of the `avalon` slot on acc-bulkuploader?
- [ ] What is the purpose of the `v6` slot on acc-bulkuploaderapt?
- [ ] What is your current deployment strategy using these slots?
- [ ] Do we need to recreate the same slot structure?

## 2. Database & Storage

### SQL Server
- [ ] What SQL Server version/tier is being used?
- [ ] Database size and performance requirements?
- [ ] Are there any specific database configurations (firewall rules, AD authentication)?
- [ ] Do we need to migrate existing data or start fresh?
- [ ] Are there any SQL Agent jobs or stored procedures?

### Storage Requirements
- [ ] Are you using any Azure Storage accounts (blob, file, queue)?
- [ ] Any CDN or static content hosting requirements?
- [ ] File upload/download capabilities in the application?

## 3. Security & Authentication

### Authentication
- [ ] What authentication method is currently used (Azure AD, B2C, custom)?
- [ ] Are users federated from on-premises AD?
- [ ] Any specific RBAC roles or permissions configured?
- [ ] API authentication method (JWT, API keys, certificates)?

### Security Configuration
- [ ] Any IP restrictions on App Services?
- [ ] SSL/TLS certificate requirements?
- [ ] Any Key Vault integration for secrets?
- [ ] CORS configuration requirements?

## 4. Monitoring & Logging

### Application Insights
- [ ] What specific metrics/alerts are configured in both Application Insights instances?
- [ ] Why are there two separate Application Insights (bulkuploader vs bulkuploaderapt)?
- [ ] Any custom telemetry or instrumentation?
- [ ] Retention period for logs?

### Alerts & Dashboards
- [ ] What do the FailureAnomalies smart detectors monitor specifically?
- [ ] What metrics are displayed in the shared dashboard?
- [ ] Any other alerts or action groups configured?

## 5. Integration & Dependencies

### External Services
- [ ] Any external API integrations?
- [ ] Dependencies on other Azure services (Service Bus, Event Grid, etc.)?
- [ ] Any on-premises connectivity requirements?
- [ ] Third-party service integrations?

### Network Configuration
- [ ] Any VNet integration requirements?
- [ ] Private endpoints needed?
- [ ] Outbound IP restrictions?

## 6. Application Configuration

### Environment Variables
- [ ] List of all app settings/connection strings needed?
- [ ] Any environment-specific configurations?
- [ ] Configuration differences between slots?

### Application Features
- [ ] Core business functionality of BulkUploader?
- [ ] File size/type limitations?
- [ ] Concurrent user expectations?
- [ ] Any scheduled jobs or background processes?

## 7. Migration Strategy

### Timeline & Approach
- [ ] Preferred migration timeline?
- [ ] Can we have downtime during migration?
- [ ] Parallel run requirements (both old and new running simultaneously)?
- [ ] Rollback strategy if issues arise?

### Data Migration
- [ ] Volume of data to migrate?
- [ ] Can we do incremental migration?
- [ ] Data validation requirements?

## 8. DevOps & Deployment

### CI/CD Pipeline
- [ ] Current deployment pipeline (Azure DevOps, GitHub Actions)?
- [ ] Source code repository location?
- [ ] Build and release pipeline configurations?
- [ ] Any specific deployment scripts or processes?

### Testing
- [ ] Testing environments needed (Dev, Test, Staging)?
- [ ] Automated testing in place?
- [ ] Performance testing requirements?

## 9. Compliance & Governance

### Requirements
- [ ] Any compliance requirements (HIPAA, SOC2, etc.)?
- [ ] Data residency requirements?
- [ ] Backup and disaster recovery requirements?
- [ ] Audit logging requirements?

## 10. Documentation & Knowledge Transfer

### Available Resources
- [ ] Is there existing documentation we can reference?
- [ ] Architecture diagrams available?
- [ ] Runbooks or operational procedures?
- [ ] Known issues or limitations?

### Support
- [ ] Who will be the technical contacts during migration?
- [ ] Any specific team members who know certain components best?
- [ ] Post-migration support expectations?

## Additional Notes Section
_Space for capturing any additional information during the meeting_

---

**Next Steps After Meeting:**
1. Review and prioritize requirements
2. Create migration plan and timeline
3. Set up new Azure tenant resources
4. Begin phased migration