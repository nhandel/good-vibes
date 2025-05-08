<!--
<promptSpec>
    <goal>To finalize a completed implementation plan: verify all steps, ensure core document consistency, generate a final report in the plan, and archive it.</goal>
    <usage>
        <scenario>Use within an AI-assisted editor when all steps in an implementation-plan-[feature-name].mdc are marked 'Completed', including Step n: Clean Up and Testing.</scenario>
        <tooling>AI-assisted coding editor.</tooling>
        <placeholders>
            <placeholder name="[feature-name]">The name of the feature corresponding to the completed plan.</placeholder>
        </placeholders>
        <notes>The AI will: 1. Verify plan completion, 2. Assess and update core documents if needed, 3. Append a final report to the plan, 4. Move the plan to .cursor/archive/.</notes>
    </usage>
    <nextSteps>
        <step>Review the final report appended to the plan (now in .cursor/archive/).</step>
        <step>Confirm that .cursor/rules/ core documents (architecture.mdc, design.mdc, tech-stack.mdc) are accurately updated.</step>
        <step>The feature development cycle for this plan is now complete.</step>
    </nextSteps>
</promptSpec>
-->
**Excellent! We should now have completed `.cursor/rules/implementation-plan-[feature-name].mdc`. Let's wrap this up.**

Remember to use the context documents (`architecture.mdc`, `tech-stack.mdc`, `design.mdc`) and the plan itself (`.cursor/rules/implementation-plan-[feature-name].mdc`).

Please perform the following final actions:

1.  **Verify Plan Completion:** Check `.cursor/rules/implementation-plan-[feature-name].mdc`; ensure ALL steps show 'Progress: Completed'. Where necessary update the plan to ensure everything matches what was accomplished.
2.  **Assess Core Documents:** Review `architecture.mdc`, `tech-stack.mdc`, `design.mdc`. Do they reflect all changes from the completed plan?
3.  **Update Core Documents (if needed):** Make necessary updates to `architecture.mdc`, `tech-stack.mdc`, `design.mdc` for full consistency with the implemented feature.
4.  **Final Report:** Create a detailed report of the implementation and add it to the bottom of the implementation plan:
    *   All plan steps in `.cursor/rules/implementation-plan-[feature-name].mdc` are marked 'Completed'. If not, list any exceptions.
    *   The core documents are now consistent with the completed plan. List any updates you made in step 3, or state if no updates were needed.
    *   Any known issues or gaps. Suggestions for future work or ideas to refine the system or feature. i.e. What was not accomplished.
5.  **Archive Plan**: Move `.cursor/rules/implementation-plan-[feature-name].mdc` to `.cursor/archive`

**Your thoroughness in this final report is key as it is the lasting mark of your work. Great job on reaching this point!**
