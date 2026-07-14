---
description: Autonomous Software Factory Cycle (Spec-Driven)
---

**Trigger**: /startcycle

## Phase 1: Design, Specification & Architecture (Propose)
1. **@pm** executes `skills/write_specs.md` based on the user's prompt.
    - *OpenSpec Action*: Reads `openspec/specs/` and creates a **Delta Spec** (`openspec/changes/delta_xxx.md`) detailing exactly what will be added, modified, or removed.
    - 🛑 **[HUMAN-IN-THE-LOOP]**: Execution PAUSES here. User MUST read and approve the Delta Spec.
2. **@designer** executes `skills/generate_assets.md`.
    - *Action*: Injects color palettes, style tokens, and generates/maps visual assets based on the Delta Spec.
3. **@architect** executes `skills/design_architecture.md`.
    - 🛑 **[HUMAN-IN-THE-LOOP]**: Execution PAUSES here for user to approve the Tech Stack and Design Pattern.

## Phase 2: Implementation (Apply)
4. **@engineer** executes `skills/generate_code.md`.
    - *OpenSpec Constraint*: The engineer is strictly locked to the approved Delta Spec. They scaffold the app, write code, and build unit tests inside `app_build/` mapping ONLY to the approved changes.

## Phase 3: Quality & Security Loop (Validate)
5. **@secdevops** executes `skills/security_code_audit.md` (SAST & Auto-patching).
6. **@qa** executes `skills/audit_code.md` (Functional checks & Edge cases).
    - *Validation Check*: @qa evaluates the code explicitly AGAINST the acceptance criteria in the Delta Spec.

### [CRITICAL CHECK] Validation Gate
- **IF** @qa or @secdevops finds ANY errors, bugs, or vulnerabilities:
    - **THEN** **@engineer** executes `skills/refactor_code.md` (Surgical fixing).
    - *OpenSpec Constraint*: @engineer ONLY alters code related to the specific bug reported. No global refactoring allowed.
    - **CIRCUIT BREAKER**: If the exact same bug persists for 3 consecutive loops, **PAUSE** execution and ask the human user for manual intervention.
    - **GOTO** Step 5 (Repeat the validation loop).
- **ELSE**:
    - **PROCEED** to Phase 4.

## Phase 4: Offensive Security
7. **@pentester** executes `skills/execute_pentest.md` to simulate advanced black-box attacks.
    - **IF** @pentester breaks the system or finds a critical exploit:
        - **THEN** **@engineer** executes `skills/refactor_code.md` specifically to patch the PoC exploit.
        - **GOTO** Step 5 (Re-verify the entire pipeline).

## Phase 5: Knowledge Consolidation (Archive)
8. **@architect** executes `skills/generate_documentation.md`.
    - *OpenSpec Action*: Compiles/updates the living technical wiki inside `docs/`. 
    - *Consolidation*: **Merges** the approved Delta Spec into the main `openspec/specs/` folder and moves the temporary Delta file to `openspec/archive/`.
9. **@engineer** executes the necessary terminal commands inside the `app_build/` directory to install dependencies and START/RESTART the application.
10. **System** outputs a final success message: "✅ **Cycle Complete!** The feature was implemented following the Delta Spec, tested, and archived. Awaiting your manual Git review and commit."