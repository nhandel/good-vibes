<!--
<promptSpec>
    <goal>To generate initial drafts for .cursor/rules/architecture.mdc, .cursor/rules/design.mdc, and .cursor/rules/tech-stack.mdc.</goal>
    <usage>
        <scenario>Use at project start or when adopting Vibe Coding.</scenario>
        <tooling>Intended for a frontier AI model (e.g., browser).</tooling>
        <placeholders>
            <placeholder name="[.cursor-copied]">Confirm you've copied the .cursor template directory.</placeholder>
            <placeholder name="[relevant-code]">Provide snippets/overview of existing code (if any).</placeholder>
            <placeholder name="[user-provided-context]">Add project description, goals, style guides, and vibe-coding/README.md content.</placeholder>
        </placeholders>
        <notes>The prompt instructs the AI to use the vibe-coding/README.md for document structure definitions and to differentiate generation for new vs. existing projects.</notes>
    </usage>
    <nextSteps>
        <step>Review AI-generated architecture.mdc, design.mdc, tech-stack.mdc in .cursor/rules/.</step>
        <step>Refine for accuracy and completeness. *Crucial: Approve before proceeding.*</step>
    </nextSteps>
</promptSpec>
-->
Generate initial drafts for the core project context documents: `.cursor/rules/architecture.mdc`, `.cursor/rules/design.mdc`, and `.cursor/rules/tech-stack.mdc`.

Use the following instructions to guide your setup:
*   Refer to the `vibe-coding/README.md` to understand each document and the overall workflow we aim to build with these documents.
*   Based on all the context provided, generate the content for `architecture.mdc`, `design.mdc`, and `tech-stack.mdc`.
*   For each document, strictly follow the structure and content guidelines defined in the `vibe-coding/README.md`.
*   Respond with a list of questions if you need more context before generating the documents.
*   Generate `architecture.mdc`: Outline high-level system overview, directory/file structure, key modules/components, data flow patterns, and design patterns. **If analyzing an existing project, focus on documenting the *current* state accurately, even if imperfect. If a new project, propose a reasonable initial structure based on the project goals and intended tech stack.** Focus on conceptual structure.
*   Generate `design.mdc`: Describe user flows/journeys, UI layout/structure (textual wireframes ok), key interactions, and accessibility considerations based on the **intended functionality and target user experience**, whether for a new project or enhancing an existing one. Avoid technology specifics.
*   Generate `tech-stack.mdc`: List programming languages, frameworks, libraries, databases, APIs, build tools, test frameworks, and deployment considerations. **If analyzing an existing project, document the *current* stack identified and explicitly incorporate any *intended future* additions, version upgrades, or deprecations mentioned in the user context. If a new project, propose or document the complete intended stack.** Ensure consistency with user input or make reasoned suggestions.
*   Output the content for each of the three documents clearly separated using code formatting.
*   If context is sparse for a section, create a structurally complete draft based on the README, using placeholders like `[Further detail needed]` or explicitly stating assumptions.
*   If user input contradicts the README's structure rules, prioritize the README's structure and note the contradiction.

User-Provided Context:

Current .cursor folder
[.cursor-copied]

Existing Codebase Snippets/Overview (if applicable):
[relevant-code]

Project Description & Goals / Transcription (if applicable):
[user-provided-context]
