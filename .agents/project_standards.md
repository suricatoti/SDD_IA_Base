# 🛡️ Global Project Standards & Constraints

This document contains the immutable technical rules for this project. ALL agents (@pm, @architect, @engineer, @secdevops, @qa, @pentester) MUST obey these rules unconditionally. Do NOT ask the user about these, just implement them.

## Security & Authentication
- **Tokens**: Strictly use signed JWT (JSON Web Tokens) for authentication.
- **Expiration**: All JWTs must have a strict, short-lived expiration time.
- **Passwords**: Must be hashed using modern algorithms (e.g., bcrypt, Argon2) before hitting the database.

## Database & Data Modeling
- **Primary Keys**: NEVER use sequential or numeric IDs (e.g., `id: 1, 2, 3`). Strictly use **UUIDv4** for all database primary keys to prevent enumeration attacks (IDOR/BOLA).
- **Soft Deletes**: Data is never hard-deleted. Implement an `is_deleted` or `deleted_at` boolean/timestamp for all critical tables.

## Communication
- **No Guessing**: If a business rule or feature is not explicitly defined here or by the user, the agent MUST pause and ask. Do not assume or hallucinate features.