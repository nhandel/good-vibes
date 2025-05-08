<!-- 
Template for Prompt: Generate Feature Implementation Plan
Use this with a Planning AI to create a new implementation plan.
Replace [feature-name], [feature-description], [relevant-code] placeholders.
-->

Follow instructions in `.cursor/rules/` using the workflow (`vibe-coding.mdc`), rules (`rules.mdc`)  and context documents (`architecture.mdc`, `tech-stack.mdc`, `design.mdc`).

Based on these documents and the following feature request:
Name: [feature-name]
Description: [feature-description]

As context, here are relevant parts of the codebase:
[relevant-code]

Generate a new, detailed implementation plan in Markdown format. The plan should:
*   Be saved as `.cursor/rules/implementation-plan-[feature-name].mdc`.
*   Include small, specific, logical steps. Each step should represent an atomic unit of work that can be implemented and validated independently.
*   For each step, include specific validation criteria (e.g., tests to write/pass, manual checks). Define test creation/modification *before* implementation actions within the step (TDD approach).
*   Include a 'Progress: Not Started' marker for each step which we will then update as the steps are completed.
*   Explicitly note which core documents (`architecture.mdc`, `tech-stack.mdc`, `design.mdc`) should serve as context and which might need updating after each step is successfully validated.
*   The generated plan **must** begin with `Step 0: Test Scaffold`** as the first step, following the pattern described in the vibe coding guide (`vibe-coding.mdc`).
*   The generated plan **must** end with `Step n: Clean Up and Testing`** as the ffinalirst step, following the pattern described in the vibe coding guide (`vibe-coding.mdc`).
*   Small code snippets are acceptable as sub-bullet points in the actions section, but limit there use to only when words are not sufficient to describe logic.
*   Consider failure paths given all of the available context. List them as risks and provide validation such that we do not deviate from a clear path from beginning to end.

**This step is critical and must be completed thoughtfully. It sets the course and keeps our code author on track enabling success. The importance of this task cannot be overstated.**

Use the following implementation plan format:

```markdown
---
description: Example Implementation Plan for Feature Z
globs: 
alwaysApply: false
---

# Implementation Plan: Feature Z

**Goal:** [Overall goal of this plan]

**Core Documents Potentially Affected:** architecture.mdc, tech-stack.mdc

## Step 0: Test Scaffolding
*   **Goal:** Enumerate all behavioral specifications as failing test stubs.
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
