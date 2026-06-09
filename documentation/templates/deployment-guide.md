# Deployment Guide: [Project/Application Name]

> Complete guide for deploying [Project/Application Name] to ServiceNow production environments

## Document Information

- **Author(s)**: [Name, Role]
- **Last Updated**: [Date]
- **Version**: 1.0
- **Status**: [Draft/Review/Final]

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Pre-Deployment Checklist](#pre-deployment-checklist)
- [Deployment Steps](#deployment-steps)
- [Validation & Testing](#validation--testing)
- [Post-Deployment](#post-deployment)
- [Rollback Procedure](#rollback-procedure)
- [Troubleshooting](#troubleshooting)
- [Appendix](#appendix)

## Overview

### Scope

This guide covers deploying [Project/Application Name] Version [X.Y.Z] to ServiceNow production instances.

### Deployment Types

- [ ] **Initial Deployment**: First time installation
- [ ] **Upgrade Deployment**: Upgrading from existing version
- [ ] **Patch Deployment**: Applying a minor patch
- [ ] **Emergency Hotfix**: Critical bug fix deployment

### Timeline & Downtime

| Activity | Duration | Estimated Time |
|----------|----------|-----------------|
| Pre-deployment backup | [X] minutes | [Start time] |
| Deployment execution | [X] minutes | [Duration] |
| Post-deployment testing | [X] minutes | [Duration] |
| **Total estimated time** | **[X] minutes** | **[Total time]** |

### Stakeholders

| Role | Name | Contact | Responsibility |
|------|------|---------|-----------------|
| Deployment Lead | [Name] | [Contact] | Overall deployment |
| Technical Lead | [Name] | [Contact] | Technical execution |
| Business Owner | [Name] | [Contact] | Business validation |
| Support Team | [Name] | [Contact] | User support |

## Prerequisites

### System Requirements

- **ServiceNow Instance**: [Version] or later
- **Instance Type**: [On-Premise/Cloud/Instance]
- **Required Plugins**: 
  - [Plugin Name 1]
  - [Plugin Name 2]
  - [Plugin Name 3]

### Infrastructure Requirements

| Component | Requirement | Current |
|-----------|-------------|---------|
| Storage Space | [Size needed] | [Current] |
| Memory | [Amount needed] | [Current] |
| CPU Cores | [Number needed] | [Current] |
| Disk I/O | [IOPS needed] | [Current] |
| Network Bandwidth | [Mbps needed] | [Current] |

### Access & Permissions

- [ ] Admin/System Administrator role
- [ ] Access to System Maintenance module
- [ ] Access to Integration modules (if applicable)
- [ ] Database backup privileges
- [ ] Change Management approval (if required)

### Knowledge Requirements

- Familiarity with ServiceNow administration
- Understanding of the application being deployed
- Knowledge of your organization's change management process
- Experience with rollback procedures

### External Dependencies

- [ ] All dependent systems are operational
- [ ] Third-party APIs are accessible and functional
- [ ] Network connectivity to integration endpoints confirmed
- [ ] SSL certificates are valid and updated

## Pre-Deployment Checklist

### 1 Week Before

- [ ] Review deployment plan with team
- [ ] Verify all prerequisites are met
- [ ] Schedule deployment window
- [ ] Notify stakeholders and users
- [ ] Assign deployment team roles
- [ ] Review release notes for breaking changes
- [ ] Complete risk assessment

### 48 Hours Before

- [ ] Lock production instance (if possible)
- [ ] Notify users of upcoming maintenance
- [ ] Prepare rollback plan
- [ ] Review any outstanding change requests
- [ ] Verify backup infrastructure
- [ ] Test backup/restore procedures

### 24 Hours Before

- [ ] Final verification of all prerequisites
- [ ] Confirm deployment team availability
- [ ] Brief deployment team
- [ ] Verify deployment artifacts are ready
- [ ] Test deployment in staging environment
- [ ] Document current system state

### 1 Hour Before

- [ ] Final communication to stakeholders
- [ ] Verify database connectivity
- [ ] Confirm backup is recent and valid
- [ ] Review deployment runbook
- [ ] Ensure support team is ready
- [ ] Document start time

### Pre-Deployment Validation

```bash
# Verify system health
System Maintenance > Health Check

# Check disk space
System Maintenance > Disk Usage

# Verify database connectivity
System Maintenance > Database
```

## Deployment Steps

### Phase 1: Backup & Preparation (30 min)

#### Step 1: Create Full System Backup

1. Navigate to: **System Maintenance > Backup/Restore**
2. Click: **"New Backup"**
3. Select:
   - Backup Type: Full
   - Include: All data and settings
4. Click: **"Create Backup"**
5. Wait for completion (monitor: System Maintenance > Backup Jobs)
6. Verify backup success in backup log
7. Document backup details:
   - Backup ID: [____________]
   - Backup Time: [____________]
   - Backup Size: [____________]

#### Step 2: Document Current Configuration

1. Export current configuration:
   ```
   1. Navigate to: System Definition > Tables
   2. Export custom tables
   3. Export business rules
   4. Export workflows
   5. Save all exports with timestamp
   ```

2. Screenshot key system settings for reference

3. Document any active integrations

#### Step 3: Disable Scheduled Jobs (if applicable)

1. Navigate to: **System Scheduler > Scheduled Jobs**
2. Disable jobs that might interfere:
   - [ ] [Job name 1]
   - [ ] [Job name 2]
   - [ ] [Job name 3]
3. Document disabled jobs for re-enabling later

#### Step 4: Notify Users

1. Broadcast system message:
   - Content: "System under maintenance. Expected downtime: [X] minutes"
   - Duration: Until deployment completes
2. Send email notification to key users
3. Post in user community/chat channels

### Phase 2: Application Deployment (45 min)

#### Step 1: Deploy Application Package

**Method 1: Update Set**
```
1. Navigate to: System Maintenance > Update Sets > Local Update Sets
2. Click: "Import Update Set"
3. Upload: [update-set-file.xml]
4. Click: "Upload"
5. Preview changes (verify they look correct)
6. Click: "Commit Update Set"
7. Wait for commitment to complete
8. Verify: System Maintenance > Update History
```

**Method 2: ServiceNow Store**
```
1. Navigate to: ServiceNow Store
2. Search: [Application Name]
3. Click: "Install" or "Upgrade"
4. Accept terms
5. Click: "Continue"
6. Monitor: System Logs > Error
```

**Method 3: Automated Deployment Script**
```bash
./deploy.sh --environment=production --version=[X.Y.Z]
# Monitor output for any errors
```

#### Step 2: Run Post-Deployment Scripts

```bash
1. Navigate to: [Application Admin Panel]
2. Click: "Initialize"
3. Run: Database migrations
4. Run: Data synchronization scripts
5. Verify: All scripts complete successfully
```

#### Step 3: Verify Deployment Success

```
Verify in System Logs:
✓ No critical errors
✓ No failed transactions
✓ Application services started

Verify in Application:
✓ New features visible
✓ UI renders correctly
✓ Functionality accessible
```

### Phase 3: Validation (30 min)

#### Step 1: System Health Checks

```bash
System Maintenance > Health Check

Verify:
- [ ] All services running
- [ ] Database connected
- [ ] No critical warnings
- [ ] Performance baseline met
```

#### Step 2: Application Testing

| Test Case | Steps | Expected Result | Status |
|-----------|-------|-----------------|--------|
| [Feature 1] | [Steps] | [Expected] | [ ] Pass |
| [Feature 2] | [Steps] | [Expected] | [ ] Pass |
| [Feature 3] | [Steps] | [Expected] | [ ] Pass |

#### Step 3: Integration Testing

1. **API Endpoint Testing**
   ```bash
   curl -X GET "[API_ENDPOINT]" \
        -H "Authorization: Bearer [TOKEN]"
   
   Expected: 200 OK response
   ```

2. **Third-Party Integration Testing**
   - [ ] [Integration 1]: Verify connection
   - [ ] [Integration 2]: Verify data sync
   - [ ] [Integration 3]: Verify error handling

3. **Database Verification**
   - [ ] Record count matches expected
   - [ ] No orphaned records
   - [ ] Indexes rebuilt successfully

#### Step 4: User Acceptance Testing

Assign to: [Business Owner/Key User]

```
Test Scenarios:
1. [ ] Scenario A: [Description]
   Status: [Pass/Fail]
   Notes: [Any issues]

2. [ ] Scenario B: [Description]
   Status: [Pass/Fail]
   Notes: [Any issues]

3. [ ] Scenario C: [Description]
   Status: [Pass/Fail]
   Notes: [Any issues]
```

### Phase 4: Post-Deployment Activation (15 min)

#### Step 1: Re-enable Disabled Services

```
1. Navigate to: System Scheduler > Scheduled Jobs
2. Enable jobs previously disabled:
   - [ ] [Job name 1]
   - [ ] [Job name 2]
3. Verify jobs execute successfully
```

#### Step 2: Update System Settings

```
1. Update system configuration as needed
2. Configure feature flags if applicable
3. Set environment-specific settings
```

#### Step 3: Clear Caches

```
1. Navigate to: System Cache
2. Click: "Clear All Caches"
3. Wait for completion
4. Verify application responsiveness
```

## Post-Deployment

### Immediate Actions (within 1 hour)

- [ ] Monitor system logs for errors
- [ ] Check application performance metrics
- [ ] Verify user access and permissions
- [ ] Validate critical business processes
- [ ] Collect initial user feedback

### Communication

1. **Notify Users**
   - Send: "System deployment successful"
   - Include: Known issues and workarounds
   - Attach: User guide updates if applicable

2. **Stakeholder Notification**
   - Inform: All stakeholders of successful deployment
   - Document: Deployment summary
   - Schedule: Post-deployment review meeting

3. **Internal Documentation**
   - Update: Deployment log
   - Document: Any manual steps taken
   - Record: Actual vs. estimated times

### Monitoring Period

**24 Hours Post-Deployment**:
- Monitor error logs continuously
- Watch system performance metrics
- Check integrations are functioning
- Document any issues

**7 Days Post-Deployment**:
- Review system performance trends
- Collect user feedback
- Document lessons learned
- Address any reported issues

### Documentation Updates

- [ ] Update runbooks with any changes
- [ ] Update configuration documentation
- [ ] Update user guides if needed
- [ ] Document deployment in change log
- [ ] Update system architecture documentation

## Rollback Procedure

**Use this only if critical issues are encountered that impact production**

### Rollback Decision Criteria

Initiate rollback if:
- Critical system functionality is unavailable
- Data corruption is detected
- Performance degradation > [X%]
- Unresolvable security vulnerability
- Business continuity is threatened

### Rollback Steps

#### Phase 1: Preparation (10 min)

1. **Stop all background jobs**
   ```
   System Scheduler > Scheduled Jobs > [disable all]
   ```

2. **Notify all users**
   - Message: "Rolling back to previous version"
   - Action: Terminate user sessions (if necessary)

3. **Document current state**
   - Take screenshots of errors
   - Note error messages
   - Export logs for analysis

#### Phase 2: Restore Backup (20-30 min)

1. **Navigate to Backup/Restore**
   ```
   System Maintenance > Backup/Restore
   ```

2. **Select backup to restore**
   - Choose backup from before deployment
   - Verify backup details are correct

3. **Execute restoration**
   - Click: "Restore Backup"
   - Wait for restoration to complete
   - Monitor: System Logs > Restore Progress

4. **Verify restoration**
   - Confirm version reverted to previous
   - Check all data is restored
   - Verify database integrity

#### Phase 3: Validation (15 min)

1. **System health check**
   - Navigate to: System Maintenance > Health Check
   - Verify all systems operational

2. **Application verification**
   - Test basic functionality
   - Verify data integrity
   - Check integrations

3. **User notification**
   - Communicate rollback completion
   - Inform of data state
   - Provide next steps

### Post-Rollback

- [ ] Root cause analysis meeting
- [ ] Document issues encountered
- [ ] Plan for re-deployment
- [ ] Update deployment plan
- [ ] Schedule post-mortem review

## Troubleshooting

### Common Issues

#### Issue 1: Deployment Fails During Import

**Symptoms**: Update set import fails with error

**Resolution**:
1. Check error message in System Log
2. Verify database has sufficient space
3. Check for conflicting customizations
4. Try importing in staging environment first
5. Contact support if error persists

**Prevention**: Test deployment in staging first

#### Issue 2: Performance Degradation After Deployment

**Symptoms**: System is slow, queries take longer

**Resolution**:
1. Clear all caches: System Cache > Clear All Caches
2. Rebuild indexes: System Definition > Tables > [affected table] > Rebuild Indexes
3. Verify CPU and memory usage
4. Check for long-running jobs
5. Review deployment changes for performance impacts

#### Issue 3: User Access Issues

**Symptoms**: Users can't access new features, permission errors

**Resolution**:
1. Verify user roles are assigned
2. Check ACL rules are not blocking access
3. Verify UI policies allow field access
4. Clear user cache and session
5. Verify user role includes required tables

#### Issue 4: Integration Failures

**Symptoms**: External systems not communicating, API errors

**Resolution**:
1. Verify network connectivity
2. Check authentication credentials
3. Verify SSL certificates valid
4. Check integration endpoint availability
5. Review integration logs for detailed errors

### Debug Mode

Enable debugging to gather more information:

```
1. Navigate to: System Diagnostics > Debug
2. Enable: Debug Output
3. Set Level: [Debug/Trace]
4. Reproduce the issue
5. Export debug log
6. Review debug log for details
```

### Escalation

If unable to resolve:

1. **Immediately initiate rollback** if business continuity threatened
2. **Document issue details**:
   - Error messages
   - Steps to reproduce
   - Screenshots
   - System logs
3. **Contact support**: [Support contact information]
4. **Escalate to**: [Escalation path]

## Appendix

### A. Deployment Checklist

- [ ] Prerequisites verified
- [ ] Backup completed successfully
- [ ] Deployment team briefed
- [ ] Users notified
- [ ] Deployment executed per plan
- [ ] Testing completed successfully
- [ ] Post-deployment tasks completed
- [ ] Monitoring established
- [ ] Documentation updated
- [ ] Stakeholders notified

### B. Contact Information

| Role | Name | Phone | Email |
|------|------|-------|-------|
| Deployment Lead | [Name] | [Phone] | [Email] |
| Technical Lead | [Name] | [Phone] | [Email] |
| Support | [Team] | [Phone] | [Email] |

### C. Useful Links

- [Deployment Runbook](#)
- [Release Notes](#)
- [Implementation Guide](#)
- [Rollback Playbook](#)
- [Troubleshooting Guide](#)

### D. Related Documentation

- [Pre-Deployment Planning](planning.md)
- [Configuration Guide](configuration.md)
- [Testing Procedures](testing.md)
- [Support Procedures](support.md)

---

**Deployment Manager**: [Name]  
**Deployment Date**: [Date]  
**Start Time**: [Time]  
**End Time**: [Time]  
**Total Duration**: [Duration]  
**Status**: [Successful/Rolled Back/Partial]

**Sign-Off**:
- [ ] Technical Lead: _________________ Date: _____
- [ ] Business Owner: _________________ Date: _____
- [ ] QA Lead: _________________ Date: _____
