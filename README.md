# Vibe Coding Workflow Guide

## Overview

AI coding assistants can really speed things up, but just winging it with prompts, especially on bigger or long-term projects, often leads to messy, hard-to-manage code that eats up any time you saved. That's where this Vibe Coding Workflow Guide comes in. It's a straightforward, organized way to get the most out of AI assistants by treating them as smart collaborators you guide, not tools that run on autopilot.

This guide lays out a clear, step-by-step process with firm boundaries. This keeps your AI focused on *your* project's architecture, design, and standards, not wandering off. We emphasize solid planning *before* diving into code, and then checking each implementation step. This really helps keep your code quality high and the project on track. A huge piece of the puzzle is giving the AI the right information – **context is king** for LLMs, after all. By setting up good foundation documents, rules, and specific prompts, your AI gets the understanding it needs to write code that's on point and actually maintainable.

You can enforce strict standards using `rules.mdc` and your own style guides, but the system is also customizable and designed to work well with the latest AI coding tools. Vibe Coding is all about keeping you and your AI assistant working together smoothly to build clean, solid, and well-documented software.

The core workflow involves:

* **Establishing Foundation Documents:** Defining the project's design, tech stack, architecture, and style guides.
* **Setting Rules:** Configuring your coding AI editor with rules for standards and document interaction.
* **Creating Implementation Plans:** Breaking down work into detailed, testable steps, starting with test scaffolds and ending with comprehensive cleanup and testing.
* **Iterative Execution:** Using a coding AI to execute plan steps one by one, following a defined multi-stage process for each step, and validating each step before proceeding.
* **Maintaining Context:** Updating foundation documents and archiving completed plans with final reports.

### Vibe-Coding — Key Principles

| Theme                  | Operational Rules (governed AI-assisted development)                                                                     |
| :--------------------- | :----------------------------------------------------------------------------------------------------------------------- |
| Planning & Control     | You define the plan and the rules; the AI executes. Do not let the AI plan autonomously or dictate the architecture.      |
| Human-in-the-Loop      | Non-negotiable. Every AI output is reviewed, explained, tested, and committed by a named developer.                        |
| Prompt Engineering     | Precise intent, adequate context, small scope. If ambiguity remains, AI must ask a clarifying question before writing code. |
| TDD & CI               | Tests (unit → integration → security/perf) precede implementation; all run automatically in CI after every commit. Target ≥ 80 % coverage. |
| Versioning & Traceability | Feature branches per plan, atomic commits per validated step, prompt/response logs stored under `.cursor/logs/`.        |
| Security by Design     | Secrets never leave secrets.*; all external input is validated; Bandit/Snyk run in CI; OWASP Top-10 checklist reviewed on PR. |
| Documentation & Provenance | Core docs are "living"; every AI interaction that mutates the code is paired with a "why/what changed" entry. Mermaid diagrams visualise data-flow. |
| Performance & Observability | Each plan defines success thresholds (latency, memory, FPS, etc.). Profilers and loggers wired from day one.          |
| Exploration vs Hardening | Explicit two-phase model: exploratory prototype (fast, permissive) → hardening (strict rules above).                       |
| Project Suitability Matrix | Low-risk tooling and MVPs can start in exploratory mode; anything customer-facing or regulated must start in governed mode immediately. |

> **Mental checkpoint — "If I can't explain the diff, I don't accept."**

### Tools

The Vibe Coding workflow is designed to be tool-agnostic, but the following categories and examples are recommended for optimal results:

- **Code Editors:**  
  *Examples:* Cursor, Windsurf, Cline, Roo  
  *Usage:* For step-by-step plan execution, code review, and context-aware editing.  

- **Speech-to-Text:**  
  *Examples:* Superwhisper, Whisperflow, others  
  *Usage:* For transcribing spoken project overviews, requirements, or code reviews.  

- **Codebase Context:**  
  *Examples:* RepoPrompt, PasteMax  
  *Usage:* For generating high-level codebase summaries or extracting relevant code snippets to provide as context in prompts.  

- **Frontier AI Models:**  
  *Examples:* Gemini (2.5 Pro), Claude (3.7, 3.5), OpenAI GPT-4.5/o3  
  *Usage:* For generating and refining core documents, implementation plans, and handling large-context reasoning tasks.  

**As of 5/8/25, I tend to use Cursor with 3.7, 2.5 and 4.1 for implementation, Gemini 2.5 and O3 for planning, Superprompt and Pastemax. Use whatever works for you**

## How to vibe code

This framework employs a structured, iterative process for AI-assisted development, leveraging different tools and prompts for planning and execution:

0.  **Initial Project Setup & Core Document Generation:**
    *   **Copy `.cursor` Template:** If starting a new project or adopting this workflow, copy the `vibe-coding/.cursor` directory contents into your project root, creating a `.cursor` folder.
    *   **Customize Style Guides:** Review the example style guides in `.cursor/rules/style-guides/`. Adapt them to your project's languages and conventions, deleting any unnecessary ones.
    *   **Generate Core Context Docs:**
        *   **Tooling:** Use a **frontier AI model** with a large context window (e.g., via a browser interface).
        *   **Input Context Preparation:**
            *   Gather the content of the `vibe-coding/README.md` (this file) to provide the AI with workflow and document structure definitions.
            *   If working on an existing codebase, prepare relevant code snippets using a codebase context tool like **PasteMax**.
            *   Prepare a detailed project description, goals, target users, and known technical constraints. Tools like **Superwhisper** can be used to transcribe a spoken overview.
            *   Have the content of any drafted style guides ready.
        *   **Execution:**
            *   Paste all the prepared context (README content, code snippets/summary, project description/transcription, style guides) into the corresponding placeholders within `prompt-0-context-setup.md`.
            *   Submit this completed prompt to the frontier AI model.
        *   **Output:** The AI will generate initial drafts of `.cursor/rules/architecture.mdc`, `.cursor/rules/design.mdc`, and `.cursor/rules/tech-stack.mdc`.
        *   **Crucial Human Review:** Meticulously review and refine these AI-generated drafts. Ensure they accurately reflect your project, adhere to the structures defined in this README, and provide a solid foundation. *Do not proceed until these documents are reviewed and approved.*

1.  **Implementation Plan Generation:**
    *   **Tooling:** Typically uses the same **frontier AI model** setup as Step 0.
    *   **Input Context Preparation:**
        *   Define the specific feature (`[feature-name]`, `[feature-description]`).
        *   Prepare relevant existing code snippets (`[relevant-code]`).
        *   Ensure the reviewed and approved core context documents (`architecture.mdc`, `design.mdc`, `tech-stack.mdc`), `rules.mdc`, `README.md`, and relevant style guides are ready.
    *   **Execution:**
        *   Paste all required context into the placeholders within `prompt-1-generate-implementation-plan.md`.
        *   Submit this completed prompt to the frontier AI model.
    *   **Output:** The AI generates a detailed `implementation-plan-[feature-name].mdc` in `.cursor/rules/`.
    *   **Crucial Human Review:** Thoroughly review this plan. Verify it includes `Step 0: Test Scaffold` and `Step n: Clean Up and Testing`. Ensure steps are logical, atomic, and have clear validation. Refine manually as needed.

2.  **Iterative Implementation (Step-by-Step Execution):**
    *   **Tooling:** Switch to an AI-assisted coding editor (e.g., **Cursor**) configured with `.cursor/rules/rules.mdc` and style guides. Ensure the generated/reviewed `implementation-plan-[feature-name].mdc` is present in the local `.cursor/rules/` directory.
    *   **Begin Plan (First Step):**
        *   Use `prompt-2-implement-plan.md` within the editor, replacing `[feature-name]`.
        *   The AI executes the first step (usually `Step 0`) using its 4-stage process (Understand Validation -> Execute/Self-Correct -> Update Context -> Report).
    *   **Continue Plan (Subsequent Steps):**
        *   After validating the previous step, use `prompt-3-continue-plan.md` within the editor, replacing `[feature-name]`.
        *   The AI executes the next incomplete step using the same 4-stage process.
    *   **Human Validation:** After each step execution by the AI, meticulously review the code changes and the AI's report. Perform all manual validation checks defined in the plan.

3.  **Handling Issues During Implementation:**
    *   If AI self-correction fails during its execution stage, or if your validation identifies problems:
        *   Use `prompt-3.5-continue-with-clarification.md` within the editor.
        *   Provide error logs, your explanation, and a clear fix request.

4.  **Final Plan Review & Archival:**
    *   Once all steps in the plan (including Step n) are 'Completed':
        *   Use `prompt-4-review-completed-plan.md` within the editor, replacing `[feature-name]`.
        *   The AI verifies completion, ensures core document consistency (updating if needed), appends a final report to the plan, and moves the plan to `.cursor/archive/`.
    *   Review the final report and confirm core documents are up-to-date.

5.  **Ad-Hoc Core Document Updates:**
    *   For updates needed outside of a specific plan's execution:
        *   Use `prompt-5-update-context-doc.md` (likely with the frontier AI model, providing necessary context).

## Project Structure and Foundation Documents

All Vibe Coding artifacts are located within the `.cursor/` directory structure. These documents and prompts work together to guide the AI, ensuring a structured and controlled development process. For `.mdc` files, detailed specifications (purpose, scope, non-scope, usage notes) are embedded as XML comments (`<!-- <vibeSpec>...</vibeSpec>`) at the top of each file; always refer to these for comprehensive details.

### Workflow Governance (`.cursor/rules/`)

These files define how the Vibe Coding process itself operates and the rules the AI must follow.

*   **`rules.mdc`**:
    *   **Purpose & Content:** Defines the AI's operational rules, interaction protocols (meta-rules), core project conventions, coding standards, and references to style guides. This is a critical document for controlling AI behavior. Contains its own `<vibeSpec>` header.
    *   **AI Usage:** **Always provided as context to the AI for all code-generating or modifying tasks.** It governs the AI's behavior throughout the entire workflow.
    *   **Updates:** Manually updated by the development team as project standards evolve. The AI does not modify this file.
*   **`vibe-coding.mdc`**:
    *   **Purpose & Content:** This document previously held detailed definitions for other `.mdc` files. It now serves as a high-level pointer, reminding users that detailed specifications for `architecture.mdc`, `design.mdc`, `tech-stack.mdc`, `rules.mdc`, and implementation plans are embedded directly within those files as `<vibeSpec>` XML headers.
    *   **AI Usage:** May be occasionally referenced by the AI if explicitly included in context, primarily to understand the self-documenting nature of other `.mdc` files.
    *   **Updates:** Manually updated if the overall documentation strategy changes.

### Core Context Documents (`.cursor/rules/`)

These documents establish the foundational knowledge for the project, defining its technical and design parameters. They are living documents, primarily updated by the AI during plan execution or via `prompt-5-update-context-doc.md`, with human review and approval being crucial.

*   **`architecture.mdc`**:
    *   **Purpose & Content:** This document serves as the high-level blueprint for the codebase. It details the overall system overview, directory and file structure conventions, key modules/components and their responsibilities, data flow patterns, critical design patterns employed, and internal communication protocols.
    *   **AI Usage:** Requested by the AI agent to understand system structure, ensure new components are placed correctly, and that new code aligns with established architectural patterns.
    *   **Updates:** Can be updated by the AI during plan steps (as specified in the plan) or via `prompt-5-update-context-doc.md`.
*   **`design.mdc`**:
    *   **Purpose & Content:** Defines the target User Experience (UX) and User Interface (UI) of the project. It includes descriptions of user flows/journeys, textual wireframes or UI layout descriptions, key user interactions, and accessibility considerations.
    *   **AI Usage:** Requested by the AI agent when implementing or modifying UI elements or any features that directly affect user interaction.
    *   **Updates:** Can be updated by the AI during plan steps (as specified in the plan) or via `prompt-5-update-context-doc.md`.
*   **`tech-stack.mdc`**:
    *   **Purpose & Content:** Lists all approved technologies, programming languages, frameworks, libraries, databases, APIs, build tools, testing frameworks, and deployment considerations, including versions.
    *   **AI Usage:** Requested by the AI agent to verify allowed dependencies, select appropriate tools, or confirm platform-specific details during code generation.
    *   **Updates:** Can be updated by the AI during plan steps (as specified in the plan) or via `prompt-5-update-context-doc.md`.

### Feature Implementation Lifecycle

This group of artifacts directly supports the process of developing and tracking features.

*   **Implementation Plans (`.cursor/rules/implementation-plan-[feature-name].mdc`)**:
    *   **Purpose & Content:** A detailed, step-by-step plan for implementing a specific feature, generated by `prompt-1-generate-implementation-plan.md`. Each plan includes goals, actions (with TDD), validation criteria, risk assessment, and progress markers for every step. Contains its own `<vibeSpec>` header detailing its structure.
    *   **AI Usage:** Manually provided as primary context to the AI when executing `prompt-2-implement-plan.md`, `prompt-3-continue-plan.md`, `prompt-3.5-continue-with-clarification.md`, and `prompt-4-review-completed-plan.md` within an editor. The AI updates the `Progress:` marker in this file after successfully validating each step.
    *   **Updates:** `Progress` markers updated by AI; content refined manually before execution.
*   **Plan Archive (`.cursor/archive/`)**:
    *   **Purpose & Content:** Stores completed implementation plans and their final reports.
    *   **AI Usage:** Automatically populated by `prompt-4-review-completed-plan.md` when a plan is successfully finalized. The AI does not typically read from this directory unless specifically instructed for historical analysis.

### Style Guides (`.cursor/rules/style-guides/`)

This directory contains specific coding style guides for different programming languages used in the project.

*   **Purpose & Content:** Houses individual `.mdc` files (e.g., `python-style-guide.mdc`, `typescript-style-guide.mdc`) that detail language-specific formatting, naming conventions, and best practices.
*   **AI Usage:** Referenced by the AI during code generation and cleanup steps, as directed by `rules.mdc`, to ensure code consistency.
*   **Updates:** Manually updated by the development team. The AI does not modify these files.

## Standardized Prompts (`.cursor/prompts/`)

These prompt templates ensure consistent and effective interaction with the AI throughout the Vibe Coding workflow. **For complete details on each prompt's usage, specific placeholders, and the AI's expected behavior, always refer to the `<promptSpec>` XML header at the beginning of the respective prompt file.**

*(General Note: Prompts `prompt-0-context-setup.md`, `prompt-1-generate-implementation-plan.md`, and `prompt-5-update-context-doc.md` are typically used with a frontier AI model (e.g., via a browser interface) that can handle larger contexts. Prompts `prompt-2-implement-plan.md`, `prompt-3-continue-plan.md`, `prompt-3.5-continue-with-clarification.md`, and `prompt-4-review-completed-plan.md` are designed for use within an AI-assisted editor environment like Cursor.)*

*   **`prompt-0-context-setup.md`**:
    *   **Purpose/Description:** Generates initial drafts of the core context documents for a new or existing project. Used at the very beginning of a project or when adopting the Vibe Coding workflow.
    *   **Key Inputs:** `vibe-coding/README.md` content, project description/goals, existing code snippets (if any), and paths to style guides. Placeholders: `[.cursor-copied]`, `[relevant-code]`, `[user-provided-context]`.
    *   **Key Outputs/Interactions:** Creates initial versions of `.cursor/rules/architecture.mdc`, `design.mdc`, and `tech-stack.mdc`.
*   **`prompt-1-generate-implementation-plan.md`**:
    *   **Purpose/Description:** Generates a new, detailed, step-by-step implementation plan for a specific feature. Used at the start of a new feature development cycle.
    *   **Key Inputs:** Feature name and description, relevant existing code snippets, and all core context documents (`architecture.mdc`, `design.mdc`, `tech-stack.mdc`, `rules.mdc`), `README.md`, and style guides. Placeholders: `[feature-name]`, `[feature-description]`, `[relevant-code]`.
    *   **Key Outputs/Interactions:** Creates a new `.cursor/rules/implementation-plan-[feature-name].mdc` file.
*   **`prompt-2-implement-plan.md`**:
    *   **Purpose/Description:** Instructs the AI coding editor to start executing an implementation plan, beginning with the first step (usually "Step 0: Test Scaffold").
    *   **Key Inputs:** The `[feature-name]` (to locate the corresponding plan), the implementation plan itself, and all core context documents.
    *   **Key Outputs/Interactions:** Modifies project code files as per the plan step, updates the `Progress` marker in the `implementation-plan-[feature-name].mdc`, and potentially updates core context documents if specified by the plan step.
*   **`prompt-3-continue-plan.md`**:
    *   **Purpose/Description:** Instructs the AI coding editor to execute the next incomplete step in an ongoing implementation plan.
    *   **Key Inputs:** The `[feature-name]`, the implementation plan, and all core context documents.
    *   **Key Outputs/Interactions:** Modifies project code files, updates the `Progress` marker in the plan, and potentially updates core context documents, similar to `prompt-2`.
*   **`prompt-3.5-continue-with-clarification.md`**:
    *   **Purpose/Description:** Used when a plan step execution fails validation, to provide the AI with error details and specific instructions for correction.
    *   **Key Inputs:** `[feature-name]`, `[step-number]` that failed, `[error-or-log-output]`, user's `[explanation]` of the problem, and a specific `[clarification]` or fix request. Consumes the implementation plan and core context documents.
    *   **Key Outputs/Interactions:** Modifies project code files to attempt a fix. Aims to get the current step to a 'Completed' state.
*   **`prompt-4-review-completed-plan.md`**:
    *   **Purpose/Description:** Performs a final review of a fully implemented plan. Verifies completion, ensures core documents are consistent, generates a final report within the plan, and archives it.
    *   **Key Inputs:** The `[feature-name]`, the completed implementation plan, and all core context documents.
    *   **Key Outputs/Interactions:** Potentially updates core context documents, appends a final report to the `implementation-plan-[feature-name].mdc`, and then moves this plan file to the `.cursor/archive/` directory.
*   **`prompt-5-update-context-doc.md`**:
    *   **Purpose/Description:** Instructs the AI to make specific, targeted updates to one of the core context documents (`architecture.mdc`, `tech-stack.mdc`, or `design.mdc`) outside of an active implementation plan.
    *   **Key Inputs:** `[topic of change]`, the `[core-doc-filename.mdc]` to be modified, and clear instructions from the user detailing the required changes.
    *   **Key Outputs/Interactions:** Modifies the specified core context document in `.cursor/rules/`.

---

*This workflow guide was inspired by the concepts presented in the "Ultimate Guide to Vibe Coding" by Nicolas Zullo (@https://github.com/EnzeD/vibe-coding). Styles guides cam from awesome-cursorrules by PatrickJS and other contributors. Thanks for your work!!*