# Employee Hub API

[![OpenAPI version](https://img.shields.io/badge/OpenAPI-3.0-85EA2D.svg?logo=swagger)](https://swagger.io/specification/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

The **Employee Hub API** provides endpoints to manage employee identity information and time-off requests.

- **Employee Identity**: Retrieve basic employee directory data (ID, photo, pronouns, office location) and reporting structure.
- **Timeoff**: Manage time-off requests (create, retrieve, delete) with details such as dates, type, and manager approval status.

**OpenAPI Version**: 3.0.2  
**API Version**: 1.0  
**Contact**: Michal Kolasinski – michalkolasinski86@gmail.com

## Servers (for portfolio purposes)

- **Production**  
  `https://virtserver.swaggerhub.com/none-2ef-05d/employee-hub-api/1.0`

## Authentication

This API uses **Bearer Authentication** (JWT).

Include the JWT token in the `Authorization` header:

```http
Authorization: Bearer <your-jwt-token>
```

## Getting Started

The full API specification is available in [`openapi.yaml`](openapi.yaml).

For interactive documentation:
- Use [Swagger Editor](https://editor.swagger.io) [Visual Studio Code](https://code.visualstudio.com/) – or any other web or local editor that supports YAML. Paste the YAML content.
- Or view it on SwaggerHub: [Employee Hub API on SwaggerHub](https://app.swaggerhub.com/apis/none-2ef-05d/employee-hub-api/1.0)

## Tags

- **Employee identity** – Employee directory and basic information
- **Timeoff** – Time-off requests and management

## Key Endpoints & Examples

### Employee Identity

#### List / Filter Employees
```http
GET /identity-people
```
Query parameters (all optional):
- `employeeIdParam` – Employee ID (4 digits)
- `employeePhotoParam` – Photo URI
- `employeePronounsParam` – Pronouns
- `officeLocationParam` – Office location

**Example** (curl):
```bash
curl -X GET "https://virtserver.swaggerhub.com/none-2ef-05d/employee-hub-api/1.0/identity-people?officeLocationParam=Katowice" \
  -H "Authorization: Bearer <token>"
```

#### Get Employee by ID
```http
GET /identity-people/{employeeIdParamPath}
```

**Example**:
```bash
curl -X GET "https://virtserver.swaggerhub.com/none-2ef-05d/employee-hub-api/1.0/identity-people/1234" \
  -H "Authorization: Bearer <token>"
```

#### Get Employee Reports (Direct Reports)
```http
GET /identity-people/identity-people/{employeeIdParamPath}/reports
```

#### Create Employee
```http
POST /identity-people
```
Body: JSON matching the `IdentityPeople` schema.

**Example**:
```bash
curl -X POST "https://virtserver.swaggerhub.com/none-2ef-05d/employee-hub-api/1.0/identity-people" \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "employeePhoto": "https://example.com/image_1.jpg",
    "employeePronouns": "they/them",
    "office_location": "Phoenix"
  }'
```

### Timeoff Requests

#### List / Filter Timeoff Requests
```http
GET /timeoff/requests
```
Query parameters (optional):
- `requestId`
- `timeoffStartDate` (YYYY-MM-DD)
- `timeoffFinnishDate` (YYYY-MM-DD)
- `timeoffType`
- `managerApproval`

**Example**:
```bash
curl -X GET "https://virtserver.swaggerhub.com/none-2ef-05d/employee-hub-api/1.0/timeoff/requests?timeoffType=Holiday&managerApproval=yes" \
  -H "Authorization: Bearer <token>"
```

#### Create Timeoff Request
```http
POST /timeoff/requests
```

**Example**:
```bash
curl -X POST "https://virtserver.swaggerhub.com/none-2ef-05d/employee-hub-api/1.0/timeoff/requests" \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "timeoffStartDate": "2025-05-15",
    "timeoffFinnishDate": "2025-05-20",
    "timeoffType": "Holiday",
    "managerApproval": "yes"
  }'
```

#### Get Timeoff Request by ID
```http
GET /timeoff/requests/{requestIdParamPath}
```

#### Delete Timeoff Request by ID
```http
DELETE /timeoff/requests/{requestIdParamPath}
```

## Error Handling

- **400 Bad Request** – Invalid input or parameters.
- Other standard HTTP status codes may apply depending on the implementation.

## Schemas Overview

### IdentityPeople
- `employeeId` (string, 4 digits, read-only)
- `employeePhoto` (URI)
- `employeePronouns` (enum: he/him, she/her, they/them, I do not wish to say)
- `office_location` (enum: Katowice, Phoenix, Salt Lake City, Choroszcz)

### Timeoff
- `requestId` (string, 4 digits, read-only)
- `timeoffStartDate` (date)
- `timeoffFinnishDate` (date)
- `timeoffType` (enum: Sick leave, Holiday, Wedding/Funeral)
- `managerApproval` (enum: yes, no)

## Tools & Validation

- Validate the spec: [Swagger Editor](https://editor.swagger.io)
- Generate client SDKs using Swagger Codegen or OpenAPI Generator.
- View rendered docs with Redoc or Swagger UI.

## License

MIT License – see the [LICENSE](LICENSE) file for details (if applicable).
