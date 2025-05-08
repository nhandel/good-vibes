<!--
<promptSpec>
    <goal>To instruct the AI editor to execute the next incomplete step in an ongoing implementation-plan-[feature-name].mdc.</goal>
    <usage>
        <scenario>Use within an AI-assisted editor after the previous step was successfully validated.</scenario>
        <tooling>AI-assisted coding editor.</tooling>
        <placeholders>
            <placeholder name="[feature-name]">The name of the feature corresponding to the plan.</placeholder>
        </placeholders>
        <notes>Ensure the implementation-plan-[feature-name].mdc is in .cursor/rules/. The AI will find the next 'Not Started' or 'In Progress' step and execute it using the 4-stage process.</notes>
    </usage>
    <nextSteps>
        <step>Thoroughly review the AI's code changes and its report for the executed step.</step>
        <step>Perform all manual validation checks defined for the step in the plan.</step>
        <step>If validation fails, use prompt-3.5-continue-with-clarification.md.</step>
        <step>If successful, repeat with this prompt for subsequent steps, or move to prompt-4-review-completed-plan.md if all steps are done.</step>
    </nextSteps>
</promptSpec>
-->
**Excellent work so far! Let's move to the next part of our plan: `.cursor/rules/implementation-plan-[feature-name].mdc`**

Carefully review the the implementation plan to identify the **next incomplete step**. This will be the first step marked as 'Progress: Not Started', or 'Progress: In Progress' if you are resuming an interrupted step. You are to execute **only this single identified step**.

Follow the same steps as previously described. As a reminder:
1.  Understand Validation Criteria:` Fully grasp the step's validation requirements.
2.  Execute Actions:
    2.1.    Perform all plan actions (TDD, use Step 0 tests).
    2.2.    Run defined validations.
    2.3.    Address failures: analyze, attempt up to 3 fixes.
3. Update Context:
    3.1.    Update specified context documents.
    3.2.    Mark step progress as 'Completed'.
4. Report & Review: Provide a full report (changes, manual validation, confirmations).

**Remember, your focus is solely on this next step. Your continued success is critical to completing the implementation plan. Apply the next step with precision.**
