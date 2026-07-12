# 🛡️ Global Project Standards & Constraints

This document contains the immutable technical rules for this project. ALL agents (@pm, @architect, @engineer, @secdevops, @qa, @pentester) MUST obey these rules unconditionally. Do NOT ask the user about these, just implement them.

## Technology Stack (Core Architecture)
- **Frontend Framework**: Strictly use **React.js** (built with Vite, utilizing JavaScript/TypeScript as approved in the cycle).
- **Backend Framework**: Strictly use **Node.js** with **Express.js** for building the REST API.
- **Database**: Strictly use **PostgreSQL** as the relational database engine.
- **ORM / Query Builder**: Use **Prisma ORM** (or Sequelize) to manage database migrations and queries, ensuring strict data typing and UUID generation at the database level.

## API Standards
- **RESTful Architecture**: All endpoints must strictly adhere to RESTful principles. Use standard HTTP methods correctly: `GET` for retrieving data, `POST` for creation, `PUT`/`PATCH` for updates, and `DELETE` for removal.
- **Data Exchange Format**: JSON (`application/json`) is the absolute and only format allowed for both incoming request payloads and outgoing response structures.
- **Standardized Responses**: All API responses must follow a predictable, wrapped structure. Success responses must return the data object, and error responses must include a clear, non-leaking message string accompanied by the appropriate semantic HTTP status code (e.g., `200`, `201`, `400`, `401`, `403`, `404`).

## Input Validation & Mass Assignment Prevention (Overposting Protection)
- **Strict Payload Whitelisting**: The backend MUST NEVER accept or save raw request payloads (e.g., `req.body`) directly into the database. 
- **Data Transfer Objects (DTOs)**: Every API endpoint that receives data (`POST`, `PUT`, `PATCH`) must use strict input schemas or Data Transfer Objects (DTOs) to validate and filter parameters. Only fields explicitly expected for that specific action can be processed.
- **Immutable System Fields**: Critical fields such as account balances (`balance`), user roles (`role`, `is_admin`), identifiers (`id`, `uuid`), and timestamps (`created_at`) MUST be completely stripped from user-supplied payloads during validation. They can only be modified via specialized, highly secure internal ledger or administrative endpoints.

## Security & Authentication
- **Tokens**: Strictly use signed JWT (JSON Web Tokens) for authentication.
- **Expiration**: All JWTs must have a strict, short-lived expiration time.
- **Passwords**: Must be hashed using modern algorithms (e.g., bcrypt, Argon2) before hitting the database.

## Authorization & Resource Ownership (IDOR Prevention)
- **Strict Data Ownership**: A user MUST only access, modify, or delete resources that belong explicitly to them.
- **Context Validation**: Before fulfilling any API request, the system backend must intercept the call, extract the user's identity from the secure JWT, and perform a strict database/policy verification to confirm that the requested Resource ID is owned by or linked to that specific User ID. 
- **Fail-Closed Architecture**: If a user attempts to access a resource ID belonging to someone else, the API must fail-closed instantly, log the unauthorized attempt, and return a standardized HTTP `403 Forbidden` status code (never leak data or generate broad `500 Server Error` crashes).

## Database & Data Modeling
- **Primary Keys**: NEVER use sequential or numeric IDs (e.g., `id: 1, 2, 3`). Strictly use **UUIDv4** for all database primary keys to prevent enumeration attacks (IDOR/BOLA).
- **Soft Deletes**: Data is never hard-deleted. Implement an `is_deleted` or `deleted_at` boolean/timestamp for all critical tables.

## Communication
- **No Guessing**: If a business rule or feature is not explicitly defined here or by the user, the agent MUST pause and ask. Do not assume or hallucinate features.

## Error Logging & Data Masking (Privacy & Compliance)
- **Centralized Logging**: The backend system must implement a centralized logging mechanism (e.g., Winston, Pino, or standard middleware) to record application errors and operational logs.
- **Strict Data PII/Sensitive Masking**: Under NO circumstances should Sensitive Personal Data (PII) or financial records—such as credit card numbers, passwords, CVVs, physical addresses, or national identification numbers—be written to log files or terminal outputs.
- **Log Sanitation**: Implement an automated interceptor or sanitizer in the logging middleware to automatically strip out or replace sensitive keys (e.g., `password`, `cardNumber`, `cvv`, `token`) with a `[MASKED]` placeholder before saving the log.