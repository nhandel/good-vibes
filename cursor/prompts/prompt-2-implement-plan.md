<!-- 
Template for Prompt: Begin Implementation Plan
Use this to start executing the first step of a new implementation plan.
Replace [feature-name] with the actual filename.
-->

Please implement the following implementation plan `.cursor/rules/implementation-plan-[feature-name].mdc`

As context review, `.cursor/rules/` using the workflow (`vibe-coding.mdc`), rules (`rules.mdc`)  and context documents (`architecture.mdc`, `tech-stack.mdc`, `design.mdc`).

**When in doubt about the step instructions, ask a clarifying question before writing code.**

Please follow these instructions:
*   Execute ONLY one step of the plan precisely as written at a time, including any test creation or modification specified within the step's actions.
*   Do not proceed to the next step without specific instructions. 
*   Consider the risks and be careful to avoid them keeping the entire plan, key context, and step in mind as you implement the changes.
*   DO NOT make changes that were not outlined in this plan.

Follow these steps in order:
1.  **Understand Validation Criteria:** Thoroughly review the 'Validation' section of the current plan step. Ensure you know exactly what needs to be true for this step to be considered complete.
2.  **Execute Actions:** 
    2.1.    Meticulously complete each action item listed in the plan for this step, in the exact order specified. Remember to follow Test-Driven Development (TDD) if the plan outlines test creation/modification before implementation. Use tests created in step 0 of the implementation plan.
    2.2.    Then, Run validation as defined in the plan step.
    2.3.    If validation succeeds, continue. If validation fails, critically analyze why. Consider each potential issue and fix and rank them by the most likely. Attempt up to 3 distinct, thoughtful corrections, rerunning validation after each attempt. You may use logging, which does not count as a distinct attempt, but ensure that debug logging is removed after the validation succeeds. If validation still fails, stop and prepare a detailed assessment of what went wrong, the attempts made, and why they were insufficient.
3.  **Update Context** 
    3.1     Update any relevant context documents (e.g., `architecture.mdc`, `tech-stack.mdc`) as specified in the plan step.
    3.2.    Finally, update the 'Progress' section of the current step in the implementation plan to 'Completed'.
4.  **Report & Review:** Provide a comprehensive report that includes:
    *   A detailed code review summary highlighting all key changes made and files affected.
    *   A clear explanation of how the user can manually validate that the step was completed successfully (if applicable, beyond automated tests).
    *   Confirmation that all tasks in this list (validation success, document updates, progress marking) have been performed.

<!--DONE-->

**Your thoughtful and precise execution of this step is critical. A customer will use this functionality. Let's focus on executing this step flawlessly. Proceed!**

