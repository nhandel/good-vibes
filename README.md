# Vibe Coding Workflow Guide

## Overview

AI coding assistants accelerate development but require clear guidance to avoid creating unmanageable codebases, especially on complex projects. This guide outlines a structured workflow using planning documents and iterative, validated execution to keep AI collaborators focused and productive, resulting in clean, maintainable code.

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

## Tooling Workflow

This Vibe Coding framework is designed to be used with a specific AI-assisted development workflow:

1.  **Implementation Plan Generation (Initial Planning):**
    * Utilize a **frontier model AI** with a very large context window (e.g., via a browser interface).
    * Employ tools like **RepoPrompt** or **PasteMax** to provide the AI with the entire codebase context.
    * Use `prompt-1-generate-implementation-plan.md` with this setup to generate the detailed `implementation-plan-[feature-name].mdc`. This leverages the large context capabilities for comprehensive planning.

2.  **Implementation Execution (Coding):**
    * Use an AI-assisted coding editor like **Cursor** or **Windsurf**.
    * Configure the editor with the rules defined in `.cursor/rules/rules.mdc` and the various style guides in `.cursor/rules/style-guides/`.
    * The remaining prompts (`prompt-0-update-context-doc.md`, `prompt-2-implement-plan.md`, `prompt-3-continue-plan.md`, `prompt-3.5-continue-with-clarification.md`, `prompt-4-review-completed-plan.md`) are designed to be used within this editor environment for iterative development.

## Project Structure and Foundation Documents

All context documents, rules, prompts, and archives reside within the `.cursor` directory at the project root.

### Design Document (`.cursor/rules/design.mdc`)

* **Purpose:** This document is the single source of truth for the User Experience (UX) and User Interface (UI) of the project. It meticulously describes what the end-user sees, interacts with, and how those interactions flow. Its primary goal is to provide the AI with unambiguous instructions on the "look and feel" and behavior of the application from a user's perspective, preventing the AI from making subjective or arbitrary design choices.
* **What Belongs:**
    * **User Flows & Journeys:** Detailed descriptions of how users navigate through the application to achieve specific goals (e.g., "User Registration Flow," "Product Purchase Journey").
    * **UI Layout & Structure:** Descriptions or textual wireframes of screens, pages, and key components. This includes the placement of elements, navigation patterns, and overall information architecture.
    * **Key Interactions:** Specifics on how users interact with UI elements (e.g., "Clicking the 'Submit' button validates the form and displays a success message," "Dragging an item from list A to list B triggers X action").
    * **Visual Elements (Descriptive):** Descriptions of visual styling, branding elements (if any), and overall aesthetic, if not covered by a separate style guide. For example, "Buttons should have rounded corners and a primary blue background."
    * **Accessibility Considerations:** Notes on how the design supports users with disabilities (e.g., "All images must have descriptive alt text," "Keyboard navigation must be fully supported").
* **What Does NOT Belong:**
    * Code implementation details (e.g., "Use a `<div>` with class `container`").
    * Specific front-end frameworks or libraries (these go in `tech-stack.mdc`).
    * Database schema or backend API endpoint definitions (these belong in `architecture.mdc` or API specifications).
    * Keep technology-specific coupling (e.g., Tailwind, GraphQL) in **Tech-Stack**, *not* in **Design**. Prompt Safety applies here as well: any insecure suggestion must be challenged.
* **Creation:** Use a Planning AI to generate the initial `design.mdc` based on your project idea, high-level requirements, and target user. Review and refine this document thoroughly with a human eye to ensure clarity and completeness before any coding begins.

### Tech Stack Document (`.cursor/rules/tech-stack.mdc`)

* **Purpose:** This document explicitly defines all technologies chosen for the project. It serves as a reference for the AI to ensure it uses the correct languages, frameworks, libraries, and tools, and adheres to any specified versions or constraints. This prevents the AI from introducing incompatible or unapproved technologies.
* **What Belongs:**
    * **Programming Languages & Versions:** (e.g., Python 3.9+, JavaScript ES2020).
    * **Frameworks & Major Libraries:** (e.g., React 18.x, Django 4.x, Express.js, Pandas, NumPy). Specify versions if critical.
    * **Databases:** (e.g., PostgreSQL 15, MongoDB 6.0, Redis).
    * **APIs (External):** List of critical third-party APIs the project will integrate with.
    * **Build Tools & Package Managers:** (e.g., Webpack, Vite, npm, pip, Maven).
    * **Testing Frameworks & Tools:** (e.g., Jest, PyTest, Selenium).
    * **Deployment Environment / Cloud Services (Key ones):** (e.g., Docker, AWS EC2, Firebase Hosting).
    * **Critical Constraints or Non-Functional Requirements impacting technology:** (e.g., "Must be deployable on a Linux server," "Real-time data processing required," "State management via Redux").
* **What Does NOT Belong:**
    * Application architecture or data flow diagrams (these go in `architecture.mdc`).
    * UI design specifications (these go in `design.mdc`).
    * Detailed coding standards (these go in specific style guide files in `.cursor/rules/style-guides/` or `rules.mdc`).
    * Business logic descriptions.
* **Creation:** Use a Planning AI, informed by the `design.mdc` and project requirements, to recommend the simplest, most robust technology stack. Review this recommendation carefully, make adjustments based on team expertise and project constraints, and then finalize the document.

### Architecture Document (`.cursor/rules/architecture.mdc`)

* **Purpose:** This document is the blueprint of the codebase. It describes how the software is structured, how its components interact, and the patterns that govern its organization. It guides the AI in creating a maintainable, scalable, and logically organized system.
* **What Belongs:**
    * **High-Level System Overview:** A brief description of the system's main parts and their overall responsibilities.
    * **Directory & File Structure Conventions:** Guidelines on how code should be organized into folders and files (e.g., "Feature modules reside in `src/features/`," "Utility functions in `src/utils/`").
    * **Key Modules/Components/Services:** Identification and description of major logical blocks of the application, their primary responsibilities, and their public interfaces (APIs).
    * **Data Flow Patterns:** How data moves through the system (e.g., "User input from UI is processed by X service, stored in Y database, and then Z component reads from Y"). Diagrams can be described textually.
    * **Key Design Patterns Employed:** (e.g., MVC, MVVM, Microservices, Event-Driven Architecture, Repository Pattern).
    * **Communication Protocols (Internal):** How different parts of the system talk to each other (e.g., REST APIs between frontend and backend, message queues for asynchronous tasks).
    * **Database Schema Overview (Conceptual):** High-level description of key data entities and their relationships, not a full SQL DDL.
* **What Does NOT Belong:**
    * Detailed UI mockups or user flows (these are in `design.mdc`).
    * Specific choices of minor libraries unless they are architecturally significant (most library choices are in `tech-stack.mdc`).
    * Implementation details of individual functions or methods.
    * Exhaustive lists of every file or class; focus on the structural elements and patterns.
    * User-facing documentation.
* **Creation:** Create an initial empty `architecture.mdc`. This document is dynamic and will be primarily populated and updated by the Coding AI (guided by your prompts) as implementation progresses. After each significant feature or step is validated, you will instruct the AI to update this document to reflect the current state of the codebase. Regular human review is crucial.

### Style Guides (`.cursor/rules/style-guides/`)

* **Purpose:** This directory contains one or more Markdown files (`.mdc`) detailing specific coding style, formatting, commenting, naming conventions, and other quality standards for the project. These guide the AI in producing clean, consistent, and maintainable code.
* **What Belongs:**
    * **Formatting Rules:** Guidelines for consistent code formatting (e.g., indentation, line length, brace style).
    * **Naming Conventions:** Rules for variables, functions, classes, files, etc. (e.g., `camelCase` for variables, `PascalCase` for classes).
    * **Commenting Standards:** Expectations for code comments (e.g., "All public methods must have docstrings," "Explain complex logic blocks").
    * **Language-Specific Best Practices:** Conventions specific to the languages used in the `tech-stack.mdc`.
    * **Project-Specific Patterns:** Preferred ways of structuring common code patterns within the project.
* **What Does NOT Belong:**
    * General project architecture (in `architecture.mdc`).
    * Tech stack choices (in `tech-stack.mdc`).
    * Feature-specific implementation details.
* **Creation:**
    * Start with common style guides for your chosen languages/frameworks.
    * Adapt and extend them with project-specific preferences. Can be generated or refined with AI assistance.
    * These are referenced during the "Clean Up and Testing" phase of an implementation plan.

### Coding Rules (`.cursor/rules/rules.mdc` or similar configuration)

* **Purpose:** This configuration file (or set of rules within the AI editor) dictates *how* the AI should behave during the development process. It defines overarching coding standards, best practices, and, most importantly, how the AI must interact with the foundational documents (`design.mdc`, `tech-stack.mdc`, `architecture.mdc`, style guides) and implementation plans. These rules are critical for ensuring consistency and adherence to the defined project structure and process. They should remain relatively stable throughout the project.
* **What Belongs:**
    * **General Coding Standards (if not in dedicated style guides):**
        * High-level formatting, naming, commenting guidelines if not detailed elsewhere.
    * **AI Meta-Rules (Interaction Rules):**
        * Instructions on when and how to consult context documents (e.g., "Always read `architecture.mdc` before creating new modules").
        * Rules for following implementation plans (e.g., "Execute only one step at a time from the plan," "Update 'Progress' marker after step validation").
        * Instructions for updating context documents post-implementation (e.g., "After a feature is complete, prompt user to confirm updates to `architecture.mdc`").
        * **Prompt Safety:** The AI must refuse or ask for clarification if an instruction would introduce TODOs, insecure code (hard-coded secrets, unsanitised input, missing tests promised in Step 0, etc.). Security scanners (Bandit, Snyk) run in CI to enforce this rule.
    * **Project-Specific Conventions & Best Practices:**
        * Modularity guidelines (e.g., "Avoid circular dependencies between modules").
        * Error handling strategies (e.g., "Use specific custom exception types for domain errors").
        * Testing preferences (e.g., "For every new public function, a corresponding unit test must be created," "Strive for TDD: define tests in plan steps before implementation actions").
        * Security considerations (e.g., "Sanitize all user inputs").
* **What Does NOT Belong:**
    * The content of `design.mdc`, `tech-stack.mdc`, `architecture.mdc`, or specific style guide files themselves.
    * Specific implementation plans for features.
    * Project requirements or user stories.
    * This file defines the *process and standards*, not the *project's substance*.
* **Creation:**
    * Generate initial rules based on `tech-stack.mdc` using editor features (if available) for language-specific standards.
    * Manually add rules for project structure, modularity, error handling, TDD preferences, and other best practices.
    * Crucially, add the meta-rules governing AI interaction with documents and plans. Mark essential rules like "Read Core Documents" as "Always Apply" or equivalent in your AI editor's configuration.

### Implementation Plan (`.cursor/rules/implementation-plan-[feature-name].mdc`)

* **Purpose:** A detailed, step-by-step guide for the Coding AI to build a specific feature or refactor a part of the project. Each plan focuses on a distinct unit of work.
* **What Belongs:**
    * **Frontmatter:** Metadata such as `description`, optional `globs` (file patterns affected), and `alwaysApply: false`.
    * **Overall Plan Goal:** A clear statement of what the entire plan aims to achieve.
    * **Core Documents Potentially Affected:** A list of core documents (e.g., `architecture.mdc`, `tech-stack.mdc`) that might need updates as the plan progresses.
    * **Sequenced Steps:** Each step should include:
        * **Step Goal:** A concise description of the step's objective.
        * **Actions:** A numbered list of specific, small, logical tasks for the AI to perform. This can include code snippets for context or as part of the instructions. Test creation or modification actions should be defined *before* code implementation actions to encourage Test-Driven Development (TDD).
        * **Design Specifications (if applicable):** Clarity on specific design elements being added or modified within the scope of this plan.
        * **Validation Criteria:** Explicit instructions for verifying the step's successful completion (e.g., "Write unit tests for function X and ensure they pass," "Run the application and verify feature Y behaves as described," "Check REPL logs for Z output").
        * **Risks:** Potential issues or challenges specific to the step.
        * **Core Document Updates (for the step):** Explicit mention of which core documents might need updating upon successful completion of *this specific step*.
        * **Progress Marker:** A status indicator with valid values `Progress: Not Started`, `Progress: In Progress`, and `Progress: Completed` (usually initialised to `Not Started`).
    * **Mandatory First Step (`Step 0: Test Scaffold`):** Details the creation of failing test stubs for all behavioral specifications of the feature.
    * **Mandatory Last Step (`Step n: Clean Up and Testing`):** Details the final code refinement process. This includes removing all temporary code/artifacts, addressing minor issues, ensuring adherence to all project standards (style guides, tech stack, naming conventions, comments), checking for overall codebase consistency, performing any necessary refactoring for quality, and executing the complete suite of feature-related tests to confirm everything passes and the code is commit-ready.
    * **Contextual Code (Optional):** Relevant code snippets or references to existing code to provide necessary context to the Planning AI.
* **What Does NOT Belong:**
    * Vague, overly broad, or ambiguous steps.
    * Steps lacking clear, testable validation criteria.
    * Core project-level rules or AI meta-rules (these belong in `rules.mdc`).
    * General project documentation or design specifications *not directly tied to the plan's execution* (these belong in the core documents).
* **Creation:**
    * Use `prompt-1-generate-implementation-plan.md` (typically with a frontier model AI and tools like RepoPrompt/PasteMax for full codebase context) to create a new `.cursor/rules/implementation-plan-[feature-name].mdc`.
    * Thoroughly review this plan. Ensure it includes `Step 0: Test Scaffold` and `Step n: Clean Up and Testing` (with comprehensive code polish actions). Make any necessary manual refinements.

### Standardized Prompts (`.cursor/prompts/`)

* **Purpose:** A collection of reusable prompt templates to ensure consistent, effective, and workflow-aligned interaction with the Coding AI.
* **What Belongs:**
    * **Prompt Templates:** Markdown files containing pre-defined instructions for the AI for various workflow stages. These templates include:
        * Placeholders for dynamic information (see "Placeholder Conventions" below).
        * Contextual instructions referencing core documents and plan files.
        * Workflow guidance (e.g., "Execute only the specified step," "Update the 'Progress' marker").
        * Explanatory HTML-style comments (``) for the human user.
* **Creation:**
    * The `.cursor/prompts/` directory and its initial set of prompt templates are included in the project template/repository.
    * Users **must** replace the placeholders in these templates with project-specific information before use.
    * For prompts like `prompt-1-generate-implementation-plan.md`, detailed feature descriptions can be dictated using tools like Super Whisper (or similar voice-to-text technology) and inserted into the designated placeholder.
* **Placeholder Conventions:**
    * Placeholders are denoted by square brackets, e.g., `[placeholder-name]`.
    * **Common Placeholders to Replace:**
        * `[feature-name]`: A descriptive name for a new feature, used for naming the implementation plan file (e.g., `implementation-plan-[feature-name].mdc`) and referencing it. Used in: `prompt-1-generate-implementation-plan.md`, `prompt-2-implement-plan.md`, `prompt-3-continue-plan.md`, `prompt-4-review-completed-plan.md`, `prompt-3.5-continue-with-clarification.md`.
        * `[step-number]`: The specific step number within an implementation plan. Used in: `prompt-3.5-continue-with-clarification.md`.
        * `[core-doc-filename.mdc]`: The filename of a core document to be updated (e.g., `architecture.mdc`, `design.mdc`, `tech-stack.mdc`). Used in: `prompt-0-update-context-doc.md`.
        * `[topic of change]`: A brief description of the change being made to a core document. Used in: `prompt-0-update-context-doc.md`.
        * `[list of precise changes]`: Specific instructions for updating a core document. Used in: `prompt-0-update-context-doc.md`.
        * `[relevant-code]`: Placeholder for relevant code snippets or context for plan generation. Used in: `prompt-1-generate-implementation-plan.md`.
        * `[feature-description]`: Placeholder for the detailed feature description for plan generation. Used in: `prompt-1-generate-implementation-plan.md`.
        * `[error-or-log-output]`: A section where you paste relevant error messages or log output. Used in: `prompt-3.5-continue-with-clarification.md`.
        * `[explanation]`: User explanation of an issue. Used in: `prompt-3.5-continue-with-clarification.md`.
        * `[clarification]`: User's specific question or fix request. Used in: `prompt-3.5-continue-with-clarification.md`.

### Archive (`.cursor/archive/`)

* **Purpose:** Stores completed and validated implementation plans for historical reference, knowledge sharing, and project auditability.
* **What Belongs:**
    * Completed implementation plan files (Markdown format) that have been fully executed, with all steps validated and marked as 'Completed', have a final report appended, and have undergone a final review.
* **Creation:**
    * The `.cursor/archive/` directory is included in the project template/repository. Plans are moved here from `.cursor/rules/` after they are fully completed and reviewed using `prompt-4-review-completed-plan.md`.

## The Workflow: AI-Assisted Implementation

This iterative process uses the Coding AI, guided by the context documents and implementation plans stored in `.cursor/rules/`.

1.  **Plan Generation & Review:**
    * Use `prompt-1-generate-implementation-plan.md` with a **frontier AI model** (large context window) and tools like **RepoPrompt/PasteMax** to provide full codebase context and create a new `.cursor/rules/implementation-plan-[feature-name].mdc`.
    * Thoroughly review this plan. Ensure it includes `Step 0: Test Scaffold` and `Step n: Clean Up and Testing` (with comprehensive code polish actions). Make any necessary manual refinements.
2.  **Begin Implementation (First Step):**
    * Within your AI-assisted editor (e.g., **Cursor**, **Windsurf**), use `prompt-2-implement-plan.md` (replacing `[feature-name]`).
    * The AI will execute the first available step (usually Step 0) following its internal 4-stage process:
        1.  **Understand Validation Criteria** for the current plan step.
        2.  **Execute Actions:** Implement TDD, run initial validation, attempt self-correction (up to 3 times if validation fails).
        3.  **Update Context:** Update relevant core documents as specified in the plan step, mark step as 'Completed'.
        4.  **Report & Review:** Provide a summary of changes, how to manually validate, and confirm tasks.
    * You (the human) review the AI's changes, the report, and perform manual validation if needed.
3.  **Continue Implementation (Subsequent Steps):**
    * Use `prompt-3-continue-plan.md` (replacing `[feature-name]`).
    * The AI identifies the next incomplete step and executes it using the same 4-stage process described above.
    * Review and validate each step's output.
4.  **Handling Issues During a Step:**
    * If the AI's self-correction (stage 2.3 of its process) fails, or if you identify issues, use `prompt-3.5-continue-with-clarification.md`. Provide error logs, an explanation, and a specific clarification or fix request.
5.  **Iterate:** Repeat step 3 (and 4 if needed) until all plan steps, including `Step n: Clean Up and Testing`, are marked 'Completed'.
6.  **Final Plan Review & Archival:**
    * Once all steps in `implementation-plan-[feature-name].mdc` are complete, use `prompt-4-review-completed-plan.md`.
    * The AI will verify all steps are completed, assess core documents for consistency, update them if needed, create a final report at the end of the plan document, and then move the plan to `.cursor/archive/`.
7.  **General Context Updates (As Needed):**
    * Use `prompt-0-update-context-doc.md` for any ad-hoc updates to core documents that arise outside the direct execution of an implementation plan step.

## Standardized Prompts (`.cursor/prompts/`)

Store these templates in the `.cursor/prompts/` directory for consistent interaction with the AI. Replace bracketed placeholders (e.g., `[feature-name]`) with actual values before use. These prompts are designed to guide the AI through the development workflow.

*(Note: `prompt-1-generate-implementation-plan.md` is typically used with a frontier model AI in a browser setting with full codebase context, while prompts 0, 2, 3, 3.5, and 4 are designed for use within an AI-assisted editor like Cursor.)*

### `.cursor/prompts/prompt-0-update-context-doc.md`
* **Goal:** To instruct the AI to make specific, targeted updates to one of the core context documents (`architecture.mdc`, `tech-stack.mdc`, or `design.mdc`).
* **How to Use:**
    * Use this prompt when a core document needs changes that were identified outside of an implementation plan's explicit "Core Document Updates" section, or based on discussions.
    * Specify the `[topic of change]` for context.
    * Provide the exact `[core-doc-filename.mdc]` to be modified.
    * Clearly list the precise changes the AI needs to make (e.g., "Add Library X to tech-stack.mdc under Core Libraries," "Update the description of Module Y in architecture.mdc").
* **Your Next Steps:**
    * Review the specified core document (`.cursor/rules/[core-doc-filename.mdc]`) to ensure the AI has integrated the requested changes accurately and clearly.
    * Verify that the changes maintain the existing format and style of the document.
    * Confirm that the document now correctly reflects the intended updates.

### `.cursor/prompts/prompt-1-generate-implementation-plan.md`
* **Goal:** To have a Planning AI generate a new, detailed, step-by-step implementation plan for a specific feature or enhancement, ensuring it starts with test scaffolding and ends with comprehensive cleanup and testing.
* **How to Use:**
    * Use this at the beginning of a new feature development cycle, typically with a frontier model AI and tools like RepoPrompt/PasteMax providing the entire codebase as context.
    * Provide a clear `[feature-description]` and `[relevant-code]` context.
    * Specify the `[feature-name]` which will be used to name the output plan file (e.g., `implementation-plan-[feature-name].mdc`).
    * The AI is instructed to read the existing core documents in `.cursor/rules/` for context and ensure the generated plan includes `Step 0: Test Scaffold` and `Step n: Clean Up and Testing` (which covers code polish, standards adherence, commenting, naming, and full test execution).
* **Your Next Steps:**
    * Carefully review the newly created `.cursor/rules/implementation-plan-[feature-name].mdc` file.
    * Check for:
        * **Clarity and Logic:** Are the steps well-defined, logical, and easy to understand?
        * **Granularity:** Are the steps small and specific enough for the AI to execute reliably?
        * **Completeness per Step:** Does each step have a clear goal, a list of actions, robust validation criteria, risks, and identified core document updates?
        * **TDD Adherence:** Are test creation/modification actions defined *before* implementation actions within each step?
        * **Mandatory Steps:** Does the plan correctly include `Step 0: Test Scaffold` and the detailed `Step n: Clean Up and Testing`?
        * **Progress Markers:** Is `Progress: Not Started` present for every step?
    * Make any necessary manual adjustments or clarifications to the plan to ensure its quality and readiness before starting implementation.

### `.cursor/prompts/prompt-2-implement-plan.md`
* **Goal:** To instruct the Coding AI to start executing an implementation plan (or the first step of a new one) by following a structured 4-stage process for the step.
* **How to Use:**
    * Use this within an AI-assisted editor (e.g., Cursor) when you are ready to begin coding the first step of an implementation plan (`.cursor/rules/implementation-plan-[feature-name].mdc`).
    * Specify the `[feature-name]` corresponding to the plan.
    * The AI will execute the identified step (usually Step 0) via: 1. Understanding Validation, 2. Executing Actions (TDD, its own validation, self-correction loop), 3. Updating Context documents and plan progress, 4. Reporting & Reviewing.
* **Your Next Steps:**
    * Thoroughly review the code changes generated by the AI for the step.
    * Examine the AI's comprehensive report.
    * Perform any manual validation defined for the step.
    * **If Validation Fails (despite AI self-correction):** Refer to `prompt-3.5-continue-with-clarification.md`. Do not proceed to the next step in the main plan.

### `.cursor/prompts/prompt-3-continue-plan.md`
* **Goal:** To instruct the Coding AI to execute the next incomplete step in an ongoing implementation plan, using the same 4-stage process as `prompt-2`.
* **How to Use:**
    * After a step has been successfully completed and validated using `prompt-2` or a previous `prompt-3`. Use within an AI-assisted editor.
    * Specify the `[feature-name]` corresponding to the plan (`.cursor/rules/implementation-plan-[feature-name].mdc`).
    * The AI will identify the next step marked 'Not Started' (or 'In Progress') and execute it following the 4-stage process (Understand Validation, Execute Actions, Update Context, Report & Review).
* **Your Next Steps:**
    * Same as for `prompt-2-implement-plan.md`: review code, report, and perform manual validation for the completed step.
    * If validation fails, use `prompt-3.5-continue-with-clarification.md`.

### `.cursor/prompts/prompt-3.5-continue-with-clarification.md`
* **Goal:** To guide the AI in fixing issues encountered during a step validation when its initial self-correction attempts were insufficient.
* **How to Use:**
    * When a step executed via `prompt-2` or `prompt-3` (within an AI-assisted editor) fails your validation, or the AI's self-correction doesn't resolve the issue.
    * Provide the `[feature-name]`, the `[step-number]` that failed, relevant `[error-or-log-output]`, your `[explanation]` of the problem, and a specific `[clarification]` or fix request.
* **Your Next Steps:**
    * Review the AI's suggested fix and code review summary.
    * Re-run validations for the step. If it now passes, you can update progress and context documents (potentially manually or by re-running the relevant part of `prompt-2`/`prompt-3`'s reporting logic for that step), and then proceed with `prompt-3-continue-plan.md` for the *next* step in the plan.

### `.cursor/prompts/prompt-4-review-completed-plan.md`
* **Goal:** To perform a final review of a fully implemented plan, ensure core documents are consistent, generate a final report within the plan, and archive the plan.
* **How to Use:**
    * Once all steps in `.cursor/rules/implementation-plan-[feature-name].mdc` are marked 'Completed', including `Step n: Clean Up and Testing`. Use within an AI-assisted editor.
    * Specify the `[feature-name]`.
    * The AI will: 1. Verify all plan steps are complete, 2. Assess core documents, 3. Update core documents if needed, 4. Create a final report at the end of the plan document, 5. Move the plan to `.cursor/archive/`.
* **Your Next Steps:**
    * Review the final report appended to the plan in the archive.
    * Confirm core documents are accurately updated.
    * The feature development cycle for this plan is now complete.

---

*This workflow guide was inspired by the concepts presented in the "Ultimate Guide to Vibe Coding" by Nicolas Zullo (@https://github.com/EnzeD/vibe-coding). Styles guides cam from awesome-cursorrules by PatrickJS and other contributors. Thanks for your work!!*