<!--
Template for Prompt 3.1: Continue With Clarification
Use this when a step failed validation but you want to KEEP current changes and let the AI attempt a focused fix.
Placeholders: [plan-filename.md], [step-number], [error-or-log-output], [clarification]
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

<!--DONE--> 