<!--
<promptSpec>
    <goal>To create a detailed, step-by-step implementation-plan-[feature-name].mdc for a new feature through an iterative two-phase process: (1) thorough requirements gathering via comprehensive questions, and (2) implementation plan generation.</goal>
    <usage>
        <scenario>Use at the start of a new feature.</scenario>
        <tooling>Intended for a frontier AI model (e.g., browser) with broad context access (e.g., RepoPrompt).</tooling>
        <placeholders>
            <placeholder name="[feature-name]">The specific name for the feature (used in the output filename).</placeholder>
            <placeholder name="[feature-description]">A clear and detailed description of the feature.</placeholder>
            <placeholder name="[relevant-code]">Snippets of existing code relevant to the new feature.</placeholder>
        </placeholders>
        <notes>The AI is instructed to use existing .goodvibes/rules/ documents (architecture.mdc, design.mdc, tech-stack.mdc, rules.mdc) and style guides for context. The output plan should be saved to .goodvibes/rules/implementation-plan-[feature-name].mdc. The process is iterative, with the AI asking clarifying questions until sufficient context is gathered before generating the plan.</notes>
    </usage>
    <nextSteps>
        <step>Engage in the iterative questioning process to ensure all aspects of the feature are well understood.</step>
        <step>Review the generated .goodvibes/rules/implementation-plan-[feature-name].mdc.</step>
        <step>Check for: clarity, logical flow, atomic steps, TDD (tests before code), inclusion of Step 0: Test Scaffold and Step n: Clean Up and Testing, validation criteria, risk assessment per step, and core document update identification.</step>
        <step>Manually refine the plan as needed before starting implementation.</step>
    </nextSteps>
</promptSpec>
-->
Feature Request:
Name: [feature-name]
Description: [feature-description]

Relevant code or context:
[relevant-code]

To create an effective implementation plan, you need to understand this feature from both user and technical perspectives. Ask the following questions (and any follow-ups needed):

User & Functional Perspective:
- What specific user problem does this feature solve?
- How will users interact with this feature and what are the key workflows?
- What are the core capabilities and expected inputs/outputs?
- Are there any specific edge cases or error conditions to handle?

Technical Implementation:
- How does this feature fit into the existing architecture?
- Which existing components will be affected and how?
- What new components, if any, need to be created?
- Will this require changes to data models, APIs, or interfaces?
- Does this feature align with patterns in our architecture.mdc?
- Are there any new libraries, tools, or technologies needed?

Boundaries & Constraints:
- What is explicitly out of scope for this feature?
- Are there any technical limitations or constraints to work within?
- What existing functionality should remain unchanged?
- Are there dependencies on other features or systems?
- Are there any performance or security considerations?

Ask clarifying follow-up questions until you have a complete understanding of the feature and there are no gaps in your knowledge. Continue this iterative questioning process until you feel confident in your ability to produce a comprehensive implementation plan.

Once you have sufficient information, create a detailed implementation plan that:

- Will be saved as `.goodvibes/rules/implementation-plan-[feature-name].mdc` following the structure in `.goodvibes/rules/good-vibes.md`
- Includes a "Relevant Files" section listing all files to be created or modified with brief descriptions
- Defines clear "Success Metrics" based on the gathered information
- Breaks down the implementation into small, atomic steps that can be independently implemented and validated

Each step should include:
- Goal: Clear objective for the step
- Actions: List of actions, with test creation/modification (TDD) always preceding implementation
- Validation Criteria: Explicit criteria to confirm the step is complete
- Risks: Potential failure paths and how validation addresses them
- Core Document Updates: Which core documents provide context and which may require updates
- Progress Marker: `Progress: Not Started` (to be updated as steps are completed)

The plan should begin with "Step 0: Test Scaffold" and end with "Step n: Clean Up and Testing".

Use the following format as a guide:

```markdown
---
description: Example Implementation Plan for Feature Z
globs: 
alwaysApply: false
---

# Implementation Plan: Feature Z

**Goal:** [Overall goal of this plan, e.g., To implement a new service for processing Z-type data and exposing it via an API endpoint.]

## Implementation Context

### User & Functional Perspective
- **Problem Solved:** Feature Z allows users to process and visualize Z-type data, addressing the need for [specific user problem].
- **Key Workflows:** Users will upload Z data, configure processing parameters, and view results in an interactive dashboard.
- **Core Capabilities:** Data validation, processing pipeline, visualization, and export functionality.

### Technical Implementation
- **Architecture Fit:** Integrates with existing data processing pipeline as a new service module.
- **Affected Components:** Will enhance the existing upload service and dashboard components.
- **Data Model Changes:** Requires new Z-data schema and processing result storage.

### Boundaries & Constraints
- **Out of Scope:** Real-time processing of Z data and integration with external systems.
- **Limitations:** Initial version limited to processing files under 100MB.
- **Unchanged Functionality:** Existing data export formats and user authentication flow.

## Relevant Files

- `src/services/feature_z_service.py` - **NEW** - Core service implementing Feature Z functionality
- `src/services/feature_z_service_test.py` - **NEW** - Unit tests for Feature Z service
- `src/api/routes/feature_z_routes.py` - **NEW** - API endpoints for Feature Z
- `src/api/routes/feature_z_routes_test.py` - **NEW** - Tests for Feature Z API routes
- `src/ui/components/FeatureZComponent.jsx` - **NEW** - UI component for Feature Z
- `src/ui/components/FeatureZComponent.test.jsx` - **NEW** - Tests for Feature Z component
- `src/services/upload_service.py` - **MODIFY** - Update to handle Z-type data files
- `src/ui/components/Dashboard.jsx` - **MODIFY** - Integrate Feature Z visualization panel

## Core Documents Affected

- `architecture.mdc` - Add Feature Z service in the data processing section (Step 1)
  - Document new service interactions with existing components
  - Update data flow diagrams to include Z-data processing
- `design.mdc` - Add new user flows for Feature Z (Step 4)
  - Include wireframes for Z-data visualization panel
  - Document user interaction patterns for configuration options
- `tech-stack.mdc` - Add new visualization library dependencies (Step 2)

## Success Metrics
- Processing time for Z-data files under 5 seconds for files up to 50MB
- UI response time under 200ms when interacting with visualization
- 95% test coverage for new components
- All accessibility requirements met for visualization components

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
        *   Verify all new/modified code against the project's context documents including `tech-stack.mdc` and any style guides in `.goodvibes/rules/style-guides/`.
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