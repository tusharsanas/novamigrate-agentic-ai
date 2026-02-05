# Complexity Rules – Agentic AI Migration Assessment

## Purpose
This document defines the decision rules used by the Agentic AI system to assess application and database complexity.

Each rule maps an input parameter to a standardized complexity level:
- Low
- Medium
- High
- Very High

These rules are referenced by the agent during assessment to ensure consistent and explainable decisions.

---

## Rule Structure (Standard)

Each input parameter follows the same structure:
- Parameter Name
- Description
- Complexity Mapping Rules
- Notes (optional)

---

## Parameter 1: Integration Count

### Description
Represents the number of upstream and downstream system integrations associated with the application.

Higher integration count increases migration risk, testing effort, and coordination complexity.

### Complexity Mapping Rules

| Integration Count | Complexity Level |
|-------------------|------------------|
| 0 – 2 | Low |
| 3 – 5 | Medium |
| 6 – 10 | High |
| > 10 | Very High |

### Notes
- Includes both synchronous and asynchronous integrations
- Third-party and external integrations increase risk more than internal integrations

---

## How the Agent Uses These Rules

For each application:
1. Read the input value from Excel
2. Match the value against the rule thresholds
3. Assign the corresponding complexity level
4. Store the result as an intermediate reasoning output

This process is repeated for all defined parameters.

---

## Example Agent Reasoning (Illustrative)

```text
Input: Integration Count = 8
Rule matched: 6–10 → High
Result: Integration Complexity = High
```

## Parameter 2: Application Architecture

### Description
Represents the overall architectural style of the application.

Application architecture significantly influences migration complexity, deployment strategy, scalability, and modernization effort.

---

### Complexity Mapping Rules

| Application Architecture | Complexity Level |
|--------------------------|------------------|
| Microservices | Low |
| Modular Monolith | Medium |
| Monolithic | High |
| Legacy Monolithic | Very High |

---

### Interpretation Guidelines
- **Microservices**:
  - Independently deployable services
  - Stateless or loosely coupled components
  - Cloud-native or cloud-ready
- **Modular Monolith**:
  - Single deployable unit but with clear module boundaries
  - Limited coupling between components
- **Monolithic**:
  - Tightly coupled components
  - Single deployment artifact
  - Requires significant coordination during migration
- **Legacy Monolithic**:
  - Hard-coded dependencies
  - Tight coupling with OS, middleware, or database
  - Often requires refactoring or re-architecture

---

### Example Agent Reasoning

```text
Input: Application Architecture = Monolithic
Rule matched: Monolithic → High
Result: Application Architecture Complexity = High
```

## Parameter 3: Integration Count

### Description
Represents the number of upstream and downstream system integrations associated with the application.

A higher number of integrations increases migration risk, dependency coordination, testing effort, and cutover complexity.

---

### Complexity Mapping Rules

| Integration Count | Complexity Level |
|-------------------|------------------|
| 1 – 2 | Low |
| 3 – 5 | Medium |
| 6 – 8 | High |
| ≥ 9 | Very High |

---

### Example Agent Reasoning

```text
Input: Integration Count = 7
Rule matched: 6–8 → High
Result: Integration Complexity = High
```

## Parameter 4: App Server OS

### Description
Represents the cloud compatibility and supportability of the operating systems used by application servers.

Operating system compatibility directly impacts migration feasibility, refactoring effort, and risk.

---

### Complexity Mapping Rules

| OS Compatibility | Supported Server Percentage | Complexity Level |
|------------------|-----------------------------|------------------|
| Fully supported | 100% | Low |
| Mostly supported | ≥ 75% | Medium |
| Partially supported | 40% – 74% | High |
| Not supported | < 40% | Very High |

---

### Interpretation Guidelines
- Supported OS examples: Amazon Linux, RHEL, Ubuntu, Windows Server (supported versions)
- Partially supported includes mixed OS environments
- Not supported includes legacy or end-of-life OS versions

---

### Example Agent Reasoning

```text
Input: Total App Servers = 4
Supported OS Servers = 2
Supported Percentage = 50%
Rule matched: 40%–74% → High
Result: App Server OS Complexity = High
```

## Parameter 5: Middleware Technologies

### Description
Represents the number of middleware components used by the application, such as application servers, messaging systems, and integration middleware.

Multiple middleware technologies increase deployment complexity, configuration effort, and migration risk.

---

### Complexity Mapping Rules

| Number of Middleware Technologies | Complexity Level |
|----------------------------------|------------------|
| 1 | Low |
| 2 – 3 | Medium |
| 4 – 5 | High |
| ≥ 6 | Very High |

---

### Examples of Middleware Technologies
- Application Servers: Tomcat, WebLogic, WebSphere, JBoss
- Messaging: Kafka, ActiveMQ, RabbitMQ
- Integration: MuleSoft, IBM IIB
- Caching: Redis, Memcached

---

### Example Agent Reasoning

```text
Input: Middleware Technologies = Tomcat, Kafka, Redis
Count = 3
Rule matched: 2–3 → Medium
Result: Middleware Complexity = Medium

```

## Parameter 6: Technical Stack

### Description
Represents the application programming languages, frameworks, and runtime stacks used to build the application.

Technical stack complexity impacts modernization effort, refactoring scope, and skill availability.

---

### Complexity Mapping Rules

| Technical Stack Condition | Complexity Level |
|--------------------------|------------------|
| Single modern and supported stack | Low |
| Single stack requiring upgrade | Medium |
| 2–3 stacks OR presence of any legacy stack | High |
| ≥ 4 stacks OR multiple legacy stacks | Very High |

---

### Examples of Modern & Supported Stacks
- Java 11+ with Spring Boot
- .NET 6+
- Node.js LTS
- Python 3.x

---

### Examples of Legacy / Upgrade-Required Stacks
- Java 7 / 8
- .NET Framework (non-Core)
- Python 2.x
- PHP < 7
- PowerBuilder, COBOL, VB6

---

### Example Agent Reasoning

```text
Input: Technical Stack = Java 8, Spring 4
Legacy stack detected
Rule matched: Single stack requiring upgrade → Medium
Result: Technical Stack Complexity = Medium
Input: Technical Stack = Java 8 | .NET Framework 4.5
Stack count = 2
Legacy stacks present
Rule matched: 2–3 stacks OR legacy stack → High
Result: Technical Stack Complexity = High
```

## Parameter 7: Database Schema and Database Count

### Description
Represents the complexity introduced by the number of databases and schemas used by the application.

Database schema count directly impacts data migration effort, dependency mapping, and validation activities.

---

### Measurement Definition
**Total Schema Count** = Sum of schemas across all databases used by the application.

---

### Complexity Mapping Rules

| Total Schema Count | Complexity Level |
|-------------------|------------------|
| 1 | Low |
| 2 – 5 | Medium |
| 6 – 20 | High |
| > 20 | Very High |

---

### Example Agent Reasoning

```text
Input:
Databases = 2
Schemas per DB = 3 and 4
Total Schema Count = 7

Rule matched: 6–20 → High
Result: Database Schema Complexity = High
```

## Parameter 8: Database Type

### Description
Represents the type of database technology used by the application and its readiness for cloud migration and modernization.

Database type significantly influences migration approach, licensing considerations, and target AWS service selection.

---

### Complexity Mapping Rules

| Database Type Category | Example Technologies | Complexity Level |
|-----------------------|----------------------|------------------|
| Cloud-native / managed friendly | Amazon Aurora, DynamoDB, PostgreSQL | Low |
| Open-source relational | MySQL, MariaDB, PostgreSQL (self-managed) | Medium |
| Commercial relational | Oracle, SQL Server, IBM DB2 | High |
| Legacy / specialized | Mainframe DB, IMS, hierarchical databases | Very High |

---

### Example Agent Reasoning

```text
Input: Database Type = Oracle
Rule matched: Commercial relational → High
Result: Database Type Complexity = High
```

## Parameter 9: Database Version

### Description
Represents the supportability and cloud compatibility of the database version used by the application.

Database version impacts migration risk, modernization effort, and target platform selection.

---

### Complexity Mapping Rules

| Database Version Status | Definition | Complexity Level |
|------------------------|------------|------------------|
| Fully supported | Supported by AWS managed database services | Low |
| Near end-of-life | Supported but vendor EOL announced | Medium |
| End-of-life | Vendor support ended | High |
| Unsupported / obsolete | Not supported on cloud platforms | Very High |

---

### Examples

| Database | Version | Status |
|--------|---------|--------|
| Oracle 19c | 19.x | Fully supported |
| MySQL 5.7 | 5.7 | Near end-of-life |
| SQL Server 2008 | 2008 | End-of-life |
| Legacy Mainframe DB | N/A | Unsupported |

---

### Example Agent Reasoning

```text
Input: Database Version = SQL Server 2008
Vendor support ended
Rule matched: End-of-life → High
Result: Database Version Complexity = High
```
## Parameter 10: Database Size

### Description
Represents the total data volume of the database associated with the application.

Database size directly impacts migration duration, tooling selection, downtime planning, and validation effort.

---

### Measurement Definition
**Database Size** = Total database size in gigabytes (GB).

---

### Complexity Mapping Rules

| Database Size (GB) | Complexity Level |
|-------------------|------------------|
| ≤ 50 | Low |
| 51 – 500 | Medium |
| 501 – 5,000 | High |
| > 5,000 | Very High |

---

### Example Agent Reasoning

```text
Input: Database Size = 1200 GB
Rule matched: 501–5,000 → High
Result: Database Size Complexity = High