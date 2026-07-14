---
description: OpenSpec Autonomous Software Factory
---

---
description: OpenSpec Autonomous Software Factory
---

**Trigger**: /openspec-cycle

## Phase 1: Propose (Alinhamento de Intenção)
1. **@pm** executes `skills/write_specs.md`.
    - *Action*: Reads `openspec/specs/` and creates a new Delta Spec inside `openspec/changes/`.
2. **@designer** executes `skills/generate_assets.md` based on the new Delta Spec[cite: 16].
3. **@architect** executes `skills/design_architecture.md` (only if the Delta Spec demands architectural changes)[cite: 6].
    - 🛑 **[HUMAN-IN-THE-LOOP]**: Execution PAUSES here. The human user MUST read the Delta Spec Markdown in the `openspec/changes/` folder. Only type "APPROVE" if the agent perfectly understood the feature.

## Phase 2: Apply (Implementação Blindada)
4. **@engineer** executes `skills/generate_code.md`[cite: 9].
    - *Action*: Writes code strictly adhering to the approved Delta Spec.

## Phase 3: QA & Security Loop
5. **@secdevops** executes `skills/security_code_audit.md`[cite: 12].
6. **@qa** executes `skills/audit_code.md`[cite: 5].
    - *Validation Check*: They evaluate the code AGAINST the Delta Spec rules.
    - *If Failed*: **@engineer** executes `skills/refactor_code.md`[cite: 11] to surgically fix the specific defect without touching out-of-scope code. Repeat this loop until PASSED.

## Phase 4: Archive (Consolidação do Conhecimento)
7. **@architect** executes `skills/generate_documentation.md`[cite: 10].
    - *Action*: Merges the approved Delta Spec into the main `openspec/specs/` folder and moves the change to `openspec/archive/`.
8. **System** outputs: "✅ Cycle Complete! Feature implemented, QA passed, and OpenSpec Source of Truth updated."