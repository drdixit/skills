---
name: gemini-council
description: Trigger this if the user says 'gemini council'.
---

# Gemini Council

Before an audit, address outstanding PR comments, fix known issues, and run
relevant checks. Give council the updated implementation; do not defer known
fixes until reviews return. If the user wants to discard the implementation
and explore new ideas, skip fixes to code being replaced.

Bring in three delegates to help with the user's request, usually for review
or feedback. Use Gemini 3.8 Flash across three different accounts at high
reasoning effort. Do not use the account running the current thread to
balance quota.

Across review rounds, pick accounts not used in earlier rounds first, then the
least recently used, so every round still has three different accounts.

In T3 Code, use `orchestrator_capabilities` to resolve the accounts, then
`delegate_task` to start one delegate per account in parallel. Elsewhere, use
the harness's subagent tools. If you cannot reach three accounts, or a named
member is unavailable, report it instead of substituting.

Give each delegate the request and relevant context. For review or feedback,
keep their work read-only. Collect their responses and summarize the useful
findings, disagreements, and recommended next steps.

Do not wait idle. When a result arrives, check its findings against the code
at once and decide what to fix while the other delegates run. Do not edit
files while a delegate still reviews the same checkout. If the user's
instructions mark a delegate as non-blocking, do not wait for it; treat a
late result as feedback and do not record its score.

For a code review, ask each delegate for a score out of 10.
Count each full review pass as one round.
A fresh review after fixes starts a new round.

If the work ends in a pull request, put the results in the PR description.
Put this section before the model and harness line at the end:

```markdown
## Council review

Rounds: 2

| Delegate | Round 1 | Round 2 |
| --- | --- | --- |
| Gemini Account 1 | 7/10 | 9/10 |
| Gemini Account 2 | no score | - |
| Gemini Account 3 | 7/10 | 9/10 |
| Gemini Account 4 | - | 9/10 |
```

If a participating delegate gives no score, write `no score`; write `-` for
rounds a delegate did not participate in. Do not invent a score.
If the PR is open, edit the description. Do not post a comment for this.
If there is no PR, put the same results in the final reply only.

If you agree with their recommendations, make the changes and request a fresh
review after. Continue this process until all delegates are satisfied with the
outcome.

Do not let repeated reviews result in scope creep. It is your job to fight to
keep scope limited.
