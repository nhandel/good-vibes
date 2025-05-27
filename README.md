# Good Vibes

## Introduction

The first time you use an AI coding tool feels like pure magic. You're typing at warp speed, watching features materialize before your eyes. But then your codebase grows, and the magic fades. The AI starts making changes you didn't ask for. Code that worked yesterday, breaks today. You find yourself spinning in an endless cycle of "plz fix," "DON'T CHANGE THAT," and "just make this one thing work" until you regret even trying to use the AI. The 90% curse hits hard. You get most of the way there, but that last 10% becomes an impossible slog of regressions. These are **Bad Vibes**.

This isn't the AI's fault, it's a context problem. As code bases stretch from 1k to 10k tokens things mostly work but as we move to 100k+ tokens, everything falls apart. Most modern code tools limit contexts to limit costs and burn VC dollars just a bit slower. Without a clear map of your project's architecture, design patterns, and coding standards, even the best models must make assumptions to accomplish most tasks. It's like having a brilliant new co-worker who you show one file too and expect them to make your project to work. 

**Good Vibes** fixes this. It's a simple process built on the emerging best practices for **Vibe Coding**. By establishing a solid foundation of context for your project, brekaing down big problems into small testable steps to implement features, and using structured prompts to guide implementation while maintaining context, you transform your AI from chaotic good into a true partner. The result? You stay connected to your codebase while building at the velocity you always wanted. These are **Good Vibes**

## Overview

The **Good Vibes Workflow** is built on five key principles:

- **Solid But Evolving Foundation**: Establish your project's DNA through core documents before diving into code, then evolve these documents as your project grows. These documents define architecture, design, and technology choices that guide implementation and are regularly updated to reflect new insights and decisions.
- **Developer Guides, AI Executes**: The developer remains in control, setting boundaries and success criteria while the AI acts as an implementation partner. Every step requires human review, testing, and approval, ensuring no code changes are committed without validation.
- **Test-Driven Development**: Tests precede implementation, ensuring functionality is verified and requirements are met. Every feature starts with test scaffolding and ends with comprehensive test coverage, making validation an integral part of the process.
- **Iterative Implementation with Atomic Steps**: Break features into small, manageable steps that build toward a complete feature. Each step corresponds to a discrete, logical commit with clear traceability to the implementation plan, allowing for focused development and easy tracking.
- **Clear and Consistent Protocols**: Use standardized prompts for consistent guidance, providing precise intent and adequate context. When instructions are ambiguous, clarifying questions ensure alignment with developer intent before proceeding with implementation.
At its core, this workflow follows five main steps:

1. **Set Up Foundation**: Create your project's `.goodvibes/` directory with core rules and documents.
2. **Plan Features**: Generate detailed implementation plans for each feature.
3. **Implement Iteratively**: Execute plans step-by-step with AI assistance.
4. **Validate Thoroughly**: Test and review all AI-generated code.
5. **Maintain Context**: Keep your foundation documents updated as your project evolves.

This README will guide you through setting up and using this workflow to keep the good vibes flowing in your development process.

## Quick Start

1.  **Clone/Download the Good Vibes Repository (if you haven't already)**
    ```bash
    # This step is to get the 'template' directory and this README for context.
    # Replace with the actual URL of the good-vibes repository if different.
    git clone https://github.com/nickhandel/good-vibes.git
    ```

2.  **Set Up Your Project with the Good Vibes Template**
    ```bash
    # Navigate to your existing project's root directory or create a new one.
    # cd path/to/your/project

    # Copy the 'template' directory from the cloned 'good-vibes' repo into your project's root,
    # renaming it to '.goodvibes'.
    # Example: If 'good-vibes' was cloned into your home directory (~/good-vibes)
    # and your project is ~/my-awesome-project, the command would be:
    cp -r ~/good-vibes/template/ ~/my-awesome-project/.goodvibes/
    # Ensure 'your-project-root' now contains the '.goodvibes' directory.
    ```

    a. If you use Cursor you'll want to copy the rules.mdc file into a .cursor directory.

    ```bash
    cp ~/my-awesome-project/.goodvibes/rules.mdc ~/my-awesome-project/.cursor/rules.mdc
    ```

    b. If you use Windsurf you'll want to copy the rules.mdc file intoa a .windsurfrules file in your project directory.

    ```bash
    cp ~/my-awesome-project/.goodvibes/rules.mdc ~/my-awesome-project/.windsurfrules
    ```

3.  **Initialize Core Documents using Prompts**
    -   Navigate to the `.goodvibes/prompts/` directory *within your project*.
    -   **Action**: Use `prompt-0-context-setup.md` with your AI coding assistant (e.g., Windsurf, Cursor).
        -   This prompt will guide the AI to generate initial drafts of essential documents. You will need to provide context, including:
            -   The content of this README (`good-vibes/README.md` from the cloned repository for overall methodology context).
            -   Your project's specific description, goals, and any existing code snippets.
            -   Paths to any style guides you've prepared or wish to use from `your-project-root/.goodvibes/style-guides/`.
        -   **Expected Output Files**: `architecture.mdc`, `design.mdc`, `tech-stack.mdc`.
        -   **Location**: These generated files should be saved into `your-project-root/.goodvibes/rules/`.
    -   **Action**: Review and customize `your-project-root/.goodvibes/rules/rules.mdc`.
        -   This file is a template for your project's coding standards, AI interaction protocols, and references to style guides. It's not generated by `prompt-0` but is a critical document that you should adapt to your project's specific needs.
    -   **Action**: Remove any unnecessary style guides from `your-project-root/.goodvibes/style-guides/` or add your own.

4.  **Customize and Commit**
    -   Meticulously review and refine the AI-generated drafts in `your-project-root/.goodvibes/rules/`.
    -   Ensure they accurately reflect your project and provide a solid foundation.
    -   Update any placeholders with your specific project details.
    -   Commit these foundational documents (`architecture.mdc`, `design.mdc`, `tech-stack.mdc`, `rules.mdc`, and any relevant style guides from `.goodvibes/style-guides/`) to your project's version control.

## The Good Vibes Workflow

The Good Vibes workflow follows a cycle of planning, AI-assisted implementation, and rigorous validation:

1.  **Plan Feature (Define the What and How)**
    *   **Objective**: Clearly define the feature, its requirements, and acceptance criteria.
    *   **Action**: Use `prompt-1-generate-implementation-plan.md` (located in `your-project-root/.goodvibes/prompts/`) with your AI assistant.
    *   **Input**: Provide the feature description, relevant context from core documents (like `architecture.mdc` or `design.mdc`), and any specific constraints.
    *   **Output**: A detailed, step-by-step `implementation-plan-[feature-name].mdc` file, saved in `your-project-root/.goodvibes/rules/`. This plan should include tasks, estimated effort (optional), and validation steps for each part.
    *   **Review**: Meticulously review and refine this plan. Ensure it's logical, complete, and aligns with project goals.

2.  **Implement Iteratively (AI-Assisted Execution)**
    *   **Objective**: Execute the steps outlined in the implementation plan with AI assistance.
    *   **Action**: For each step (or a small group of steps) in the plan, use `prompt-2-implement-plan.md` or `prompt-3-continue-plan.md`.
    *   **Input**: The specific step(s) from the plan, relevant code context, and references to `rules.mdc` and style guides.
    *   **Output**: AI-generated code, tests, or other artifacts.
    *   **Review & Refine**: CRITICALLY REVIEW ALL AI OUTPUT. Test thoroughly. Ensure it adheres to standards in `rules.mdc` and style guides. Iterate with the AI if necessary until the step is correctly implemented.

3.  **Validate and Integrate**
    *   **Objective**: Ensure the implemented feature works as expected and integrates cleanly with the existing codebase.
    *   **Action**: Run all relevant tests (unit, integration, etc.). Perform manual testing if applicable. Review against acceptance criteria from the implementation plan.
    *   **Refactor**: If needed, refactor the code for clarity, performance, or adherence to architectural principles, guided by your core documents.

4.  **Finalize & Archive Plan**
    *   **Objective**: Conclude the work on the feature and document the outcome.
    *   **Action**: Once all steps in the implementation plan are complete and validated, use `prompt-4-review-completed-plan.md`.
    *   **Input**: The completed `implementation-plan-[feature-name].mdc`.
    *   **Output**: A final review summary or report. This prompt may also guide updates to core context documents (e.g., `architecture.mdc`) if the feature introduced significant changes.
    *   **Archive**: Move the completed `implementation-plan-[feature-name].mdc` and its report (if separate) to `your-project-root/.goodvibes/archive/` for future reference.

5.  **Maintain Living Documents**
    *   **Objective**: Keep core project documents up-to-date.
    *   **Action**: If a feature's implementation leads to changes in architecture, design, tech stack, or project rules, use `prompt-5-update-context-doc.md` to guide the AI in updating the relevant `.mdc` files in `your-project-root/.goodvibes/rules/`.

## Project Foundation: Files and Directories

The `.goodvibes/` directory in your project root is the heart of this methodology. It contains all the configuration, rules, prompts, and archives that structure your AI-assisted development process.

-   **`.goodvibes/rules/`**: This is where the guiding intelligence of your project lives.
    -   `architecture.mdc`: Defines the high-level system structure, components, their responsibilities, and interactions. Includes directory structure conventions.
    -   `design.mdc`: Outlines UX/UI principles, user flows, key screen layouts, and overall aesthetic guidelines.
    -   `tech-stack.mdc`: Specifies approved technologies, languages, frameworks, libraries, tools, and their versions.
    -   `rules.mdc`: Contains AI operational rules, project-specific coding standards, references to style guides, testing strategies, and other development conventions.
    -   `implementation-plan-[feature-name].mdc`: Detailed, step-by-step plans for developing specific features. These are created for each significant piece of work.

-   **`.goodvibes/prompts/`**: Contains standardized prompt templates designed for consistent and effective interaction with your AI coding assistant. Each prompt is tailored for a specific task in the workflow (e.g., generating an implementation plan, executing a plan step).
    -   `prompt-0-context-setup.md`: Initializes core project documents.
    -   `prompt-1-generate-implementation-plan.md`: Creates a new feature implementation plan.
    -   `prompt-2-implement-plan.md`: Executes the first step(s) of an implementation plan.
    -   `prompt-3-continue-plan.md`: Continues executing subsequent steps of a plan.
    -   `prompt-4-review-completed-plan.md`: Finalizes and archives a completed plan.
    -   `prompt-5-update-context-doc.md`: For targeted updates to core context documents.

-   **`.goodvibes/archive/`**: Stores completed implementation plans and their final reports. This provides a historical record of development activities and decisions.

-   **`.goodvibes/style-guides/`**: Contains language-specific or general coding style guides (e.g., `python-style-guide.mdc`, `javascript-style-guide.mdc`). These are referenced in `rules.mdc` and guide the AI in generating code that adheres to your team's preferences.

---

*This workflow guide was inspired by the concepts presented in the "Ultimate Guide to Vibe Coding" by Nicolas Zullo (@https://github.com/EnzeD/good-vibes). Styles guides cam from awesome-cursorrules by PatrickJS and other contributors. Thanks for your work!!*