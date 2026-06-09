# API Documentation: [API Name]

> [Brief description of the API and its purpose]

## Document Information

- **API Version**: 1.0
- **Last Updated**: [Date]
- **Status**: [Active/Beta/Deprecated]
- **Maintainer**: [Team/Person name]
- **Base URL**: [https://your-instance.service-now.com/api/]

## Table of Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
- [Authentication](#authentication)
- [Rate Limiting](#rate-limiting)
- [Endpoints](#endpoints)
- [Request/Response Examples](#requestresponse-examples)
- [Error Handling](#error-handling)
- [Status Codes](#status-codes)
- [Code Examples](#code-examples)
- [FAQ](#faq)

## Overview

### Purpose

[Explain what this API does and why it exists]

### Key Features

- [Feature 1]
- [Feature 2]
- [Feature 3]

### Use Cases

- [Use case 1]
- [Use case 2]
- [Use case 3]

### API Type

- [ ] REST API
- [ ] SOAP Web Service
- [ ] GraphQL API
- [ ] Custom Scripting API

## Getting Started

### Prerequisites

- Active ServiceNow instance (version [X.X] or later)
- API access credentials
- [Any other required software/tools]

### Quick Start

1. **Obtain Credentials**
   ```
   Contact: [Team/Person]
   Process: [How to request API access]
   ```

2. **Construct Request**
   ```
   Base URL: [URL]
   Endpoint: [/endpoint]
   Full URL: [Full example URL]
   ```

3. **Make API Call**
   ```bash
   curl -X GET "[URL]" \
        -H "Authorization: Bearer [YOUR_TOKEN]" \
        -H "Content-Type: application/json"
   ```

## Authentication

### Authentication Methods

#### Method 1: OAuth 2.0

- **Flow Type**: [Authorization Code/Client Credentials/etc]
- **Token Endpoint**: [URL]
- **Required Scopes**: [List scopes]

**Setup**:
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Request Header**:
```
Authorization: Bearer [access_token]
```

#### Method 2: API Key

**Setup**:
1. Generate API key in [System > Integration]
2. [Other setup steps]

**Request Header**:
```
X-API-Key: [your_api_key]
```

#### Method 3: Basic Authentication

**Request Header**:
```
Authorization: Basic [base64_encoded_credentials]
```

## Rate Limiting

### Rate Limits

| Tier | Requests/Minute | Requests/Day | Burst Limit |
|------|-----------------|--------------|------------|
| Free | 60 | 10,000 | 5 |
| Pro | 300 | 100,000 | 10 |
| Enterprise | Unlimited | Unlimited | N/A |

### Headers

```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1640995200
```

### Handling Rate Limits

If you receive a 429 (Too Many Requests) response:
1. Wait until the time specified in `X-RateLimit-Reset`
2. Implement exponential backoff in your client
3. Consider upgrading your plan

## Endpoints

### Endpoint 1: List Records

**Description**: [Brief description of what this endpoint does]

**Endpoint**: `GET /api/now/table/[table_name]`

**Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| sysparm_query | String | No | Query filter (encoded query) |
| sysparm_limit | Integer | No | Number of records to return (default: 10, max: 10000) |
| sysparm_offset | Integer | No | Number of records to skip (pagination) |
| sysparm_fields | String | No | Comma-separated list of fields to return |

**Request Example**:
```
GET /api/now/table/incident?sysparm_query=priority=1&sysparm_limit=10
```

**Response Example**:
```json
{
  "result": [
    {
      "sys_id": "a1b2c3d4e5f6g7h8",
      "number": "INC0000001",
      "short_description": "Network connectivity issue",
      "priority": "1",
      "state": "1",
      "created_on": "2024-01-15 10:30:00"
    }
  ]
}
```

### Endpoint 2: Get Single Record

**Description**: [Brief description]

**Endpoint**: `GET /api/now/table/[table_name]/[sys_id]`

**Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| sysparm_fields | String | No | Comma-separated list of fields to return |

**Request Example**:
```
GET /api/now/table/incident/a1b2c3d4e5f6g7h8
```

**Response Example**:
```json
{
  "result": {
    "sys_id": "a1b2c3d4e5f6g7h8",
    "number": "INC0000001",
    "short_description": "Network connectivity issue",
    "priority": "1",
    "state": "1",
    "assigned_to": "d1e2f3g4h5i6j7k8",
    "created_on": "2024-01-15 10:30:00",
    "updated_on": "2024-01-15 14:45:00"
  }
}
```

### Endpoint 3: Create Record

**Description**: [Brief description]

**Endpoint**: `POST /api/now/table/[table_name]`

**Request Body**:

```json
{
  "short_description": "New incident",
  "priority": "2",
  "assignment_group": "a1b2c3d4e5f6g7h8"
}
```

**Response Example** (201 Created):
```json
{
  "result": {
    "sys_id": "new_sys_id_here",
    "number": "INC0000002",
    "short_description": "New incident",
    "priority": "2"
  }
}
```

### Endpoint 4: Update Record

**Description**: [Brief description]

**Endpoint**: `PATCH /api/now/table/[table_name]/[sys_id]`

**Request Body**:
```json
{
  "state": "2",
  "work_notes": "Updated status"
}
```

**Response Example** (200 OK):
```json
{
  "result": {
    "sys_id": "a1b2c3d4e5f6g7h8",
    "number": "INC0000001",
    "state": "2",
    "work_notes": "Updated status"
  }
}
```

### Endpoint 5: Delete Record

**Description**: [Brief description]

**Endpoint**: `DELETE /api/now/table/[table_name]/[sys_id]`

**Response Example** (204 No Content):
```
[Empty body]
```

## Request/Response Examples

### Example 1: Search for Active Incidents

**Request**:
```bash
curl -X GET "https://instance.service-now.com/api/now/table/incident" \
     -H "Authorization: Bearer your_token" \
     -H "Content-Type: application/json" \
     -d '{"sysparm_query": "state=1", "sysparm_limit": 5}'
```

**Response**:
```json
{
  "result": [
    {
      "sys_id": "1",
      "number": "INC001",
      "short_description": "Issue 1",
      "state": "1"
    }
  ]
}
```

### Example 2: Create and Update Incident

**Create**:
```bash
curl -X POST "https://instance.service-now.com/api/now/table/incident" \
     -H "Authorization: Bearer your_token" \
     -H "Content-Type: application/json" \
     -d '{
       "short_description": "Server down",
       "priority": "1",
       "urgency": "1"
     }'
```

**Update**:
```bash
curl -X PATCH "https://instance.service-now.com/api/now/table/incident/sys_id" \
     -H "Authorization: Bearer your_token" \
     -H "Content-Type: application/json" \
     -d '{"state": "7"}'
```

## Error Handling

### Error Response Format

```json
{
  "error": {
    "message": "Invalid request",
    "detail": "Field 'priority' is required"
  },
  "status": "failure"
}
```

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Invalid credentials | Expired or incorrect token | Refresh your authentication token |
| Record not found | sys_id doesn't exist | Verify the sys_id is correct |
| Invalid query | Malformed encoded query | Check query syntax |
| Permission denied | Insufficient role access | Request required roles |
| Rate limit exceeded | Too many requests | Wait and retry with backoff |

## Status Codes

| Code | Status | Description |
|------|--------|-------------|
| 200 | OK | Request succeeded |
| 201 | Created | Resource successfully created |
| 204 | No Content | Request succeeded, no content to return |
| 400 | Bad Request | Invalid request format or parameters |
| 401 | Unauthorized | Missing or invalid authentication |
| 403 | Forbidden | Authenticated but insufficient permissions |
| 404 | Not Found | Resource not found |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server error occurred |
| 503 | Service Unavailable | Service temporarily unavailable |

## Code Examples

### JavaScript/Node.js

```javascript
const axios = require('axios');

const apiCall = async () => {
  try {
    const response = await axios.get(
      'https://instance.service-now.com/api/now/table/incident',
      {
        headers: {
          'Authorization': 'Bearer YOUR_TOKEN',
          'Content-Type': 'application/json'
        },
        params: {
          sysparm_query: 'priority=1',
          sysparm_limit: 10
        }
      }
    );
    console.log(response.data);
  } catch (error) {
    console.error('Error:', error.response?.data || error.message);
  }
};

apiCall();
```

### Python

```python
import requests

url = 'https://instance.service-now.com/api/now/table/incident'
headers = {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
}
params = {
    'sysparm_query': 'priority=1',
    'sysparm_limit': 10
}

response = requests.get(url, headers=headers, params=params)
print(response.json())
```

### Java

```java
import javax.net.ssl.HttpsURLConnection;
import java.net.URL;
import java.io.InputStream;

String url = "https://instance.service-now.com/api/now/table/incident";
URL obj = new URL(url);
HttpsURLConnection conn = (HttpsURLConnection) obj.openConnection();

conn.setRequestMethod("GET");
conn.setRequestProperty("Authorization", "Bearer YOUR_TOKEN");
conn.setRequestProperty("Content-Type", "application/json");

int responseCode = conn.getResponseCode();
InputStream is = conn.getInputStream();
// Parse response...
```

## FAQ

### Q: How do I get started with the API?

A: [Detailed answer about getting started]

### Q: What authentication method should I use?

A: [Guidance on choosing authentication method]

### Q: How do I handle errors?

A: [Explanation of error handling approach]

### Q: What are the rate limits?

A: [Information about rate limiting]

### Q: How do I report a bug or request a feature?

A: Contact [Email/Support channel] with details

## Completion Checklist

- [ ] All endpoints documented
- [ ] Request/response examples provided
- [ ] Authentication methods clearly explained
- [ ] Error handling documented
- [ ] Code examples in multiple languages included
- [ ] Rate limiting clearly communicated
- [ ] FAQ section completed
- [ ] Links to supporting documentation
- [ ] Contact information for support provided
- [ ] Version information maintained

---

**Support**: [Support email/channel]
**Bug Reporting**: [How to report bugs]
**Change Log**: See [CHANGELOG.md](../CHANGELOG.md)
**Last Updated**: [Date]
**Next Review**: [Date]
