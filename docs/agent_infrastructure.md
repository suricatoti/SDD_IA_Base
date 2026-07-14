# 🤖 Multi-Agent Software Factory Infrastructure Guide

This document provides a comprehensive overview of the autonomous multi-agent pipeline built within this repository. It serves as the single source of truth for understanding how the AI engineering team operates, how models are routed, and how to execute or modify the development lifecycle.

---

## 🏗️ Architectural Core Principles & Constraints

To achieve maximum technical precision and eliminate common AI pitfalls (such as hallucinations and context degradation), this factory operates under strict engineering pillars:

1. **Isolated Context Windows:** No agent is polluted with the entire conversation history. When triggered, its context is wiped clean, injecting only its persona, the specific technical playbook (Skill), and the raw artifact left by the previous agent.
2. **Model Routing Optimization (Cost vs. Quality):** Tasks are strictly assigned to LLM classes based on their complexity. Heavy reasoning roles use frontier models (**Claude 3.5 Sonnet**). Context-heavy scanning tasks use massive context models (**Gemini 1.5 Pro**). Formatting and logging tasks use hyper-fast, cost-effective models (**Gemini 1.5 Flash**). 
3. **Automated Agent-in-the-Loop Remediation:** Quality validation is an iterative feedback loop. If an audit fails, the pipeline automatically routes a consolidated list of errors back to the `@engineer` and resets the validation phase until it passes 100% clean.
4. **Global Language Constraint:** Regardless of the user's input language, the entire pipeline is hardcoded to output all code comments, architecture blueprints, logs, and documentation **strictly in English**.

---

## 📂 Workspace Directory Structure

The repository relies on a strict folder hierarchy to manage context isolation and artifact hand-offs:

*   `.agents/`: The brain of the factory.
    *   `agents.md`: Centralizes all AI personas, goals, traits, and LLM model routing configurations.
    *   `workflows/startcycle.md`: The macro-pipeline that dictates the chronological execution order and conditional loops.
    *   `skills/`: The directory containing individual modular playbooks (`.md` files) dictating *how* an agent should perform a specific task.
*   `app_build/`: The isolated sandbox where the `@engineer` writes and updates the actual application source code and unit tests.
*   `openspec/`: The official SDD bridge folder. Contains `specs/` (Main Specs) and `changes/` (Delta Specs) where blueprints are managed for the engineer to read.
*   `reports/`: The audit trail folder. Holds the outputs of the `@secdevops`, `@qa`, and `@pentester` vulnerability and test execution logs.
*   `docs/`: The living technical wiki, auto-generated incrementally by the Architect at the end of every successful cycle.

---

## 👥 AI Agent Personas & Model Routing

| Agent Tag | Professional Role | Assigned LLM Model | Core Mission / Input-Output Focus |
| :--- | :--- | :--- | :--- |
| **@pm** | Product Manager | `gemini-1.5-flash` | Translates user prompts into a structured `Technical_Specification.md`. |
| **@architect** | Principal Architect | `claude-3-5-sonnet` | Converts specs into a `System_Architecture.md` blueprint, and compiles the final technical wikis. |
| **@engineer** | Full-Stack Developer | `claude-3-5-sonnet` | Writes production-ready code and corresponding unit test suites inside `app_build/`. |
| **@secdevops** | AppSec Engineer | `gemini-1.5-pro` | Audits raw code against OWASP Top 10 using a deep context window, patching flaws immediately. |
| **@qa** | QA Engineer | `gemini-1.5-flash` | Executes unit tests, simulates functional workflows, and creates compliance reports. |
| **@pentester** | Red Team Specialist | `claude-3-5-sonnet` | Attacks the final runtime package looking for business logic exploits and authentication bypasses. |

---

## 🔄 The Development Lifecycle (Workflow)

The automation sequence is controlled by `.agents/workflows/startcycle.md`.

```text
[ User Prompt ]
       │
       ▼
 🔹 PHASE 1: SYSTEM DESIGN
 └── @pm (Drafts Requirements) ➔ @architect (Drafts Folder & Stack Blueprint)
       │
       ▼
 🔹 PHASE 2: IMPLEMENTATION & CODE-GEN
 └── @engineer (Generates Application Source Code + Unit Tests)
       │
       ▼
 🔄 PHASE 3: QUALITY & SECURITY LOOP (Iterative)
 ├── @secdevops (Scans & Patches SAST Flaws)
 ├── @qa (Validates Business Rules & Unit Tests)
 │     └── ❌ IF BUGS FOUND: Triggers @engineer refactoring (`refactor_code.md`) ➔ Loop restarts at Step 4.
 ├── @pentester (Executes Offensive Black-Box Cyberattacks)
 │     └── ❌ IF EXPLOIT FOUND: Triggers @engineer secure patching ➔ Loop restarts at Step 4.
       │
       ▼
 🏁 PHASE 4: KNOWLEDGE CONSOLIDATION
 └── @architect (Scans entire production state ➔ Compiles the modular `docs/` Wiki)

## 🚀 Execution & Extensibility
### How to Run the Pipeline
Open this workspace directory within your Antigravity IDE.

Open the Conversation pane.

Fire the custom slash command /startcycle followed by your detailed feature request.

Example: /startcycle Create a multi-tenant SaaS billing API with microservices architecture using Node.js and PostgreSQL.

## How to Add a New Agent or Skill
To scale the team (e.g., adding a dedicated Database Administrator):

Define the new persona and its LLM model routing inside .agents/agents.md.

Create a new instruction playbook inside .agents/skills/ (e.g., optimize_database.md).

Inject the new skill execution step into the chronological order of .agents/workflows/startcycle.md.