---
description: Autonomous Software Factory Cycle
---

**Trigger**: /startcycle

## Phase 1: Design & Architecture
1. **@pm** executes `skills/write_specs.md` based on the user's prompt.
    - 🛑 **[HUMAN-IN-THE-LOOP]**: Execution PAUSES here for user to approve the MVP scope and requirements.
2. **@architect** executes `skills/design_architecture.md`.
    - 🛑 **[HUMAN-IN-THE-LOOP]**: Execution PAUSES here for user to approve the Tech Stack and Design Pattern.

## Phase 2: Implementation
3. **@engineer** executes `skills/generate_code.md`.
    - *Action*: Scaffolds the app, writes code, and builds the initial unit tests inside `app_build/`.

## Phase 3: Quality & Security Loop
4. **@secdevops** executes `skills/security_code_audit.md` (SAST & Auto-patching).
5. **@qa** executes `skills/audit_code.md` (Functional checks & Edge cases).

### [CRITICAL CHECK] Validation Gate
- **IF** @qa or @secdevops finds ANY errors, bugs, or vulnerabilities:
    - **THEN** **@engineer** executes `skills/refactor_code.md` (Surgical fixing & Regression tests).
    - **CIRCUIT BREAKER**: If the exact same bug persists for 3 consecutive loops, **PAUSE** execution and ask the human user for manual intervention.
    - **GOTO** Step 4 (Repeat the validation loop).
- **ELSE**:
    - **PROCEED** to Phase 4.

## Phase 4: Offensive Security
6. **@pentester** executes `skills/execute_pentest.md` to simulate advanced black-box attacks.
    - **IF** @pentester breaks the system or finds a critical exploit:
        - **THEN** **@engineer** executes `skills/refactor_code.md` specifically to patch the PoC exploit.
        - **GOTO** Step 4 (Re-verify the entire pipeline from the start to ensure the patch didn't break functionality).

## Phase 5: Knowledge Consolidation & Version Control
7. **@architect** executes `skills/generate_documentation.md`.
    - *Action*: Compiles/updates the living technical wiki inside the `docs/` directory.
8. **@engineer** executes system commands for Version Control:
    - Runs `git init` (if the repository is not yet initialized).
    - Runs `git add .` to stage the application code, reports, and documentation.
    - Runs `git commit -m "feat(auto): successful cycle completion - <brief summary of features added>"`
9. **System** outputs a final success message in the chat: "✅ **Cycle Complete!** The application is secure, tested, documented, and safely committed to version control."