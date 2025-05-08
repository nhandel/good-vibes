<!--
<promptSpec>
    <goal>To create a detailed, step-by-step implementation-plan-[feature-name].mdc for a new feature, including test scaffolding and cleanup steps.</goal>
    <usage>
        <scenario>Use at the start of a new feature.</scenario>
        <tooling>Intended for a frontier AI model (e.g., browser) with broad context access (e.g., RepoPrompt).</tooling>
        <placeholders>
            <placeholder name="[feature-name]">The specific name for the feature (used in the output filename).</placeholder>
            <placeholder name="[feature-description]">A clear and detailed description of the feature.</placeholder>
            <placeholder name="[relevant-code]">Snippets of existing code relevant to the new feature.</placeholder>
        </placeholders>
        <notes>The AI is instructed to use existing .cursor/rules/ documents (architecture.mdc, design.mdc, tech-stack.mdc, rules.mdc) and style guides for context. The output plan should be saved to .cursor/rules/implementation-plan-[feature-name].mdc.</notes>
    </usage>
    <nextSteps>
        <step>Review the generated .cursor/rules/implementation-plan-[feature-name].mdc.</step>
        <step>Check for: clarity, logical flow, atomic steps, TDD (tests before code), inclusion of Step 0: Test Scaffold and Step n: Clean Up and Testing, validation criteria, risk assessment per step, and core document update identification.</step>
        <step>Manually refine the plan as needed before starting implementation.</step>
    </nextSteps>
</promptSpec>
-->
Generate a new, detailed implementation plan to be saved as `.cursor/rules/implementation-plan-[feature-name].mdc`.
Refer to the `vibe-coding/README.md` (which you must have in context) for the required structure of an implementation plan.
Feature Request:
Name: [feature-name]
Description: [feature-description]

As context, here are relevant parts of the codebase:
[relevant-code]

Generate a new, detailed implementation plan in Markdown format. The plan should:
*   The implementation plan must be saved as `.cursor/rules/implementation-plan-[feature-name].mdc` and strictly follow the structure and guidelines in `vibe-coding/README.md`.
*   Break down the plan into small, specific, atomic steps—each representing a logical unit of work that can be independently implemented and validated.
*   Each step must include:
    - **Goal:** Clear objective for the step.
    - **Actions:** List of actions, with test creation/modification (TDD) always preceding implementation. Use small code snippets only when necessary for clarity.
    - **Validation Criteria:** Explicit criteria (e.g., tests to write/pass, manual checks) to confirm the step is complete.
    - **Risks:** Potential failure paths and how validation addresses them.
    - **Core Document Updates:** Which core documents (`architecture.mdc`, `tech-stack.mdc`, `design.mdc`) provide context and which may require updates after the step.
    - **Progress Marker:** `Progress: Not Started` (to be updated as steps are completed).
*   The plan must always begin with `Step 0: Test Scaffold` (detailing creation of failing test stubs) and end with `Step n: Clean Up and Testing` (covering final code refinement, standards adherence, and full testing), following the patterns in the vibe coding guide.
*   For each step, explicitly note relevant context documents and any that might need updating.
*   If context is insufficient for robust planning, clearly state the ambiguity and make a reasoned assumption.
*   Ensure all risks and validation criteria are considered to maintain a clear, successful path from start to finish.

**This step is critical and must be completed thoughtfully. It sets the course and keeps our code author on track enabling success. The importance of this task cannot be overstated.**

Use the following implementation plan format example as a guide for your output:

```markdown
---
description: Example Implementation Plan for Feature Z
globs: 
alwaysApply: false
---

# Implementation Plan: Feature Z

**Goal:** [Overall goal of this plan, e.g., To implement a new service for processing Z-type data and exposing it via an API endpoint.]

**Core Documents Potentially Affected:** architecture.mdc, tech-stack.mdc

## Step 0: Test Scaffolding
*   **Goal:** Enumerate all behavioral specifications for Feature Z as initially failing test stubs.
*   **Actions:**
    1.  **Test Stubs:** For each requirement of Feature Z, create a corresponding failing unit test stub in `tests/features/test_featureZ_service.py`. Examples:
        *   `test_featureZ_handles_null_input()`
        *   `test_featureZ_processes_valid_data_correctly()`
        *   `test_featureZ_reports_specific_error_on_X_condition()`
*   **Validation:**
    *   All created test stubs for Feature Z are present in `tests/features/test_featureZ_service.py`.
    *   Running `pytest tests/features/test_featureZ_service.py` shows all new tests for Feature Z as *failing*.
*   **Risks:**
    *   Initial requirements for Feature Z are vague on error handling details, so test stubs for failure conditions might be incomplete.
    *   Overlooking a key external dependency for Feature Z during initial test design could lead to significant rework later.
*   **Core Document Updates:** None for this step.
*   **Progress:** Not Started

## Step 1: Setup Feature Z Module
*   **Goal:** Create the basic file structure and initial class/component for Feature Z.
*   **Actions:**
    1.  Create a new directory: `src/features/featureZ/`.
    2.  Create `src/features/featureZ/__init__.py`.
    3.  Create `src/features/featureZ/featureZ_service.py`.
    4.  Define a basic class `FeatureZService` in `featureZ_service.py` with an `__init__` method that accepts `config: AppConfig` and stores it.
    5.  **Test:** Create `tests/features/test_featureZ_service.py` (if not already done in Step 0, or add to it). Write/update a unit test that imports `FeatureZService` and successfully instantiates it with a mock config. Ensure this test passes.
*   **Validation:**
    *   Verify the directory structure and files exist.
    *   Run the unit test for instantiation; it must pass.
*   **Risks:**
    *   Choosing an overly generic name for `FeatureZService` could lead to naming conflicts if other 'Z-like' features are added later.
    *   Not defining a clear interface or contract for `FeatureZService` early on might make future mocking or alternative implementations difficult.
    *   Forgetting to add the new `src/features/featureZ/` path to `.gitignore` or linter configurations if it's a new top-level feature directory.
*   **Core Document Updates:** `architecture.mdc` (add new module description).
*   **Progress:** Not Started

## Step 2: Implement Core Logic
*   **Goal:** Implement core data processing functions for the service
*   **Actions:**
    1.  Add `process_data` method to `FeatureZService` that takes input dictionary of raw data, validates required fields and data types, returns formatted data dictionary
    2.  Add `transform_data` method that takes processed data dictionary, applies business rules and transformations, returns list of transformed data records
    3.  Add `generate_output` method that takes list of transformed data records, formats into required output string format
    4.  **Test:** Add unit tests for each method in `tests/features/test_featureZ_service.py`:
        - `test_process_data`: Test validation of required fields, data types, formatting
        - `test_transform_data`: Test business rule application and transformations
        - `test_generate_output`: Test output string formatting
*   **Validation:**
    *   All unit tests for these methods must pass.
    *   Manual verification of output format matches requirements (if applicable for this stage).
*   **Risks:**
    *   The `process_data` method might become a bottleneck if it performs complex synchronous operations on large datasets without considering asynchronous processing or batching.
    *   Business rules for `transform_data` are based on current assumptions; if these rules change frequently, the transformation logic could become brittle without a more flexible rule engine.
    *   The `generate_output` formatting could be tightly coupled to a specific consumer; if multiple output formats are needed later, this step will require significant refactoring.
*   **Core Document Updates:** `architecture.mdc` (update module description with new data processing capabilities).
*   **Progress:** Not Started

# ... subsequent steps ...

## Step n: Cleanup and Testing
*   **Goal:** Ensure the feature's codebase is pristine, adheres to all project standards (style, naming, comments), is consistent with the existing codebase, all temporary development artifacts are removed, any minor pending issues are resolved, and all feature-related tests pass successfully. The code should be commit-ready and production-quality.
*   **Actions:**
    1.  **Code Review & Refinement:** Conduct a thorough review of all code implemented for the feature.
    2.  **Remove Temporary Artifacts:** Delete all temporary logging statements (e.g., `print()`, `console.log()`), debug flags/variables, commented-out old code, and any other development-specific constructs.
    3.  **Resolve Minor Issues:** Address any minor outstanding issues, TODOs, or FIXMEs that were noted during development and deferred for this final cleanup phase.
    4.  **Adherence to Standards & Consistency Review:**
        *   Verify all new/modified code against the project's context documents including `tech-stack.mdc` and any style guides in `.cursor/rules/style-guides/`.
        *   Ensure all functions, classes, and complex logic blocks have clear and appropriate comments.
        *   Check for consistent and meaningful variable names, function names, and class names, both within new files and in relation to the existing codebase.
        *   Review for overall consistency with the architecture and patterns of the existing codebase.
        *   Perform any necessary refactoring to meet these standards and improve code clarity or maintainability.
    5.  **Comprehensive Test Execution:** Run the entire suite of tests associated with the feature. This includes all tests created in 'Step 0: Test Scaffolding' as well as any unit, integration, or other tests developed throughout the implementation steps.
    6.  **Address Test Failures (if any):** If any tests fail, diagnose the root cause and implement the necessary corrections. Re-run tests until all pass.
*   **Validation:**
    *   Manual code inspection confirms that no temporary or debug-specific code remains in the feature's modules.
    *   Code adheres to all guidelines in `c` and `tech-stack.mdc`.
    *   Functions and classes are well-commented; variable and function naming is clear and consistent.
    *   The new code is consistent with the overall architecture and patterns of the existing codebase.
    *   All automated tests related to the feature (e.g., in `tests/features/test_featureZ_service.py` and any other relevant test suites) execute and pass successfully.
    *   The feature functions as expected according to all defined requirements and acceptance criteria.
    *   Code linting and static analysis tools report no new issues for the feature's codebase.
    *   The codebase for the feature is confirmed to be in a clean, polished, and commit-ready state.
*   **Risks:**
    *   Overlooking some deeply embedded temporary code or debug flags.
    *   Introducing a regression while fixing a minor issue, refactoring, or removing temporary code.
    *   Tests might have had an implicit dependency on some temporary code, causing them to fail after cleanup.
    *   Refactoring for consistency could inadvertently alter behavior if not carefully tested.
    *   Style or naming convention debates could slow down this step if not clearly defined in guides.
*   **Core Document Updates:** `tech-stack.mdc` might need an update if any specific debugging tools or temporary libraries were used and are now confirmed to be removed. 
*   **Progress:** Not Started