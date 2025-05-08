<!--
<promptSpec>
    <goal>To guide the AI editor in fixing issues from a previous step that failed validation, even after AI self-correction attempts.</goal>
    <usage>
        <scenario>Use within an AI-assisted editor when a step executed via prompt-2 or prompt-3 fails your manual validation.</scenario>
        <tooling>AI-assisted coding editor.</tooling>
        <placeholders>
            <placeholder name="[feature-name]">The name of the feature corresponding to the plan.</placeholder>
            <placeholder name="[step-number]">The number of the step in the plan that failed.</placeholder>
            <placeholder name="[error-or-log-output]">Relevant error messages or log outputs.</placeholder>
            <placeholder name="[explanation]">Your explanation of the problem observed.</placeholder>
            <placeholder name="[clarification]">Specific instructions or a fix request for the AI.</placeholder>
        </placeholders>
        <notes>The AI should read core documents and the implementation plan. It needs to analyze the error, make minimal changes, suggest improvements for future prompts/plans, and provide a code review summary. It should not proceed to other steps.</notes>
    </usage>
    <nextSteps>
        <step>Review the AI's suggested fix and its code review summary.</step>
        <step>Re-run all validations for the corrected step.</step>
        <step>If the step now passes, ensure its 'Progress' in the plan is updated to 'Completed' (this might need manual update or re-running part of the reporting logic of prompt-2/prompt-3).</step>
        <step>Then, proceed with prompt-3-continue-plan.md for the *next* step in the overall plan.</step>
    </nextSteps>
</promptSpec>
-->

Read all core documents in `.cursor/rules/` (architecture.mdc, tech-stack.mdc, design.mdc) and the implementation plan `.cursor/rules/implementation-plan-[feature-name].mdc`.

We attempted **Step [step-number]** but encountered the following errors or issues:

Log:
[error-or-log-output]

Explanation
[explanation]

Clarification:
[clarification]

Please answer the clarification / fix request below:




Tasks:
1. Analyse the error/logs and my question.
2. Make the minimal necessary code changes (including tests if required) to resolve the issue and satisfy the validation criteria for Step [step-number].
3. Suggest how future prompts or plan wording could prevent similar failures.
4. Provide a brief code-review summary of the changes.
5. Do NOT proceed to any other step.