<!--
<promptSpec>
    <goal>To generate initial drafts for .goodvibes/rules/architecture.mdc, .goodvibes/rules/design.mdc, and .goodvibes/rules/tech-stack.mdc after gathering sufficient context through clarifying questions.</goal>
    <usage>
        <scenario>Use at project start or when adopting Good Vibes Workflow.</scenario>
        <tooling>Intended for a frontier AI model (e.g., browser).</tooling>
        <placeholders>
            <placeholder name="[.goodvibes]">Confirm you've copied the .goodvibes template directory.</placeholder>
            <placeholder name="[relevant-code]">Provide snippets/overview of existing code (if any).</placeholder>
            <placeholder name="[user-provided-context]">Add project description, goals, style guides, and any other content you have available.</placeholder>
        </placeholders>
        <notes>The prompt instructs the AI to first ask clarifying questions to gather sufficient context, then use the `good-vibes.mdc` for document structure definitions and to differentiate generation for new vs. existing projects.</notes>
    </usage>
    <nextSteps>
        <step>Review AI-generated architecture.mdc, design.mdc, tech-stack.mdc in .goodvibes/rules/.</step>
        <step>Refine for accuracy and completeness. *Crucial: Approve before proceeding.*</step>
    </nextSteps>
</promptSpec>
-->
I'll help you generate initial drafts for the core project context documents: `.goodvibes/rules/architecture.mdc`, `.goodvibes/rules/design.mdc`, and `.goodvibes/rules/tech-stack.mdc`.

User-Provided Context:

Current .goodvibes folder
[.goodvibes]

Existing Codebase Snippets/Overview (if applicable):
[relevant-code]

Project Description & Goals / Transcription (if applicable):
[user-provided-context]

Before generating these documents, you need to understand the project. Ask these questions (and any necessary follow-ups):

Project Basics:
- What is the primary purpose of your project and what problem does it solve?
- Who are the target users and what are their key needs?
- Is this a new project or are you working with existing code?

Technical Overview:
- What type of application is this (web, mobile, API, etc.)?
- What are the main components or modules you envision?
- What programming languages, frameworks, and databases will you use?

User Experience:
- What are the 2-3 most important user flows or journeys?
- Any specific UI/UX requirements or design patterns to follow?
- Are there any particular accessibility or performance considerations?

Once you have sufficient understanding, generate the following documents:
- `architecture.mdc`: System architecture, components, and their interactions
- `design.mdc`: Target user experience and interface
- `tech-stack.mdc`: Technologies, frameworks, and tools to be used
- `good-vibes.mdc`: Coding standards and practices for the project

Refer to the `good-vibes.mdc` to understand the workflow and follow the structure and guidelines for each file in the `rules/` directory.

Output each document clearly separated with code formatting. If information is limited, create structurally complete drafts with placeholders and clearly stated assumptions.
