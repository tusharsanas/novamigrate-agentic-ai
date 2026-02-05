# NovaMigrate – Input Schema (v1)

## Overview
NovaMigrate uses an Excel-based input file to capture application and database characteristics required for early-stage migration complexity assessment. Each row represents a single application.

The schema is intentionally minimal and focused on parameters that materially influence migration complexity.

---

## Application Parameters (6)

| Column Name | Type | Allowed Values |
|------------|----|---------------|
| App ID | String | Free text |
| App Name | String | Free text |
| Application Architecture | Enum | Monolith, 3-Tier, Microservices |
| App Server Count | Integer | ≥1 |
| App Server OS | Enum | Redhat, Windows, Ubuntu, RockyLinux |
| Middleware Technologies | Enum (Multi) | WebLogic, WebSphere, Tomcat, IIS, None |
| Technical Stack | Enum | Java, .NET, C/C++, Mixed |
| Integration Count | Integer | ≥0 |

---

## Database Parameters (4)

| Column Name | Type | Allowed Values |
|------------|----|---------------|
| Database/Schema Count | Integer | ≥0 |
| Database Type | Enum | Oracle, MySQL, SQL Server, Informix, None |
| Database Version | Interger | ≥0 |
| Database Size (GB) | Integer | ≥0 |

---

## Input Rules
- Each row represents one application
- Column names must match exactly
- Missing values are allowed and handled through assumptions
- This schema supports early-stage migration assessment only
