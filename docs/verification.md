# AGY learning flow verification

This checklist verifies phase 5 of the implementation plan. A file review is not evidence that modal quizzes, artifact rendering, or skill discovery work in an AGY session. Keep the live checks pending until their observations are recorded.

## Verification recorded on 2026-10-05

- All four AGY skills and the always-on rule exist; the original pi configuration is untouched by this implementation pass.
- `agy --help` succeeds on this device and exposes interactive and print modes. `agy agents` exits successfully but prints no agent listing; this does not confirm researcher or helper availability.
- The four skills pass the skill-creator frontmatter validator with Python UTF-8 mode (`python -X utf8 .../quick_validate.py <skill-directory>`).
- Static review found and corrected incomplete-session resume, lesson-note rebuilding, direct review/practice initialization, untaught-node scheduling, review batching conflicts, repeat interval advancement, and the missing optional drawio helper brief.
- Live lesson dry run, new-conversation resume, and topic switching: **pending**. No simulated learner history has been added to `topics/`.

## 1. Dry run: binary numbers

Run `agy` from the repository root. Say:

> Teach me binary numbers. My goal is to convert small nonnegative integers between decimal and binary and explain why place values work.

Answer the goal and probe questions as a learner. Include at least one correct answer and one miss or “Don't know”.

Check these observable outcomes:

1. The teaching skill activates; if it needs an explicit request to use `.agents/skills/teach/SKILL.md`, record the auto-trigger failure separately.
2. `topics/binary-numbers/{profile,map,log}.md` exist, with the goal and probe evidence saved as the session proceeds. Probe questions arrive one at a time, with grading after each answer.
3. A small dependency map and plan appear. The agent waits for approval; the profile stays in `plan` until approval is logged.
4. Approve the plan and complete two nodes. For a quiz-check on 2026-10-05, a pass gives `locked-in`, interval 1, `last_reviewed: 2026-10-05`, and `next_review: 2026-10-06`; a miss gives `shaky` with the same dates. Use the actual local date if running later.
5. After each result, the node table, Mermaid classes, log, and both artifacts agree. Inspect `learning-map.md` and `lesson-notes.md` in the current conversation's artifact directory. Record whether the graph actually renders; a fenced source block alone is not rendering evidence.
6. Say `wrap up`. The summary includes both nodes, quiz counts, misconceptions, corrections, and review dates, and is saved in the log and notes artifact.

## 2. New conversation: resume and review

Exit and run plain `agy` again to create a new conversation (do not use `--continue` or `--conversation`). Say `continue binary numbers`.

Check that the start card matches the saved files and both artifacts are rebuilt in the new conversation's directory. Previous notes must be present even though the conversation has no previous messages. The agent must not repeat the full probe or silently approve an unfinished plan.

Say `review binary numbers`. If nothing is due yet, request a specific taught node; do not move the system clock or change real learner history just to force a due date. To test a genuinely overdue batch, return on its scheduled date or use a separate disposable workspace with explicitly labeled synthetic state.

Check a batch of up to eight questions, with fewer when fewer nodes are eligible. A node that is both shaky and due appears once in the selection. Untaught nodes never appear. A correct review of a locked-in interval-1 node on a later day advances to 3 days; a shaky node's recovery stays at 1 day. A miss resets to 1 day. A successful immediate retry restores `locked-in` at 1 day rather than advancing to 3. Multiple questions about one node cause one schedule update, with any miss taking precedence. A canceled question changes no status or dates.

Also test interruption once during `probe` or before plan approval in a disposable topic: a new conversation must resume that phase using saved evidence.

## 3. Topic isolation

Record the binary topic's file contents or hashes. Say `teach me fractions`, complete its goal step, and then say `continue binary numbers`.

Check that fractions gets its own profile, map, and log. The binary state remains unchanged until an intentional binary lesson or quiz update. Both live artifacts switch back to binary content; fractions notes must not appear in them. A direct `practice binary numbers` request must initialize state and artifacts without asking the learner to choose practice again.

## Record the result

For each live check, append the date, AGY version if available, conversation IDs, relevant state/artifact paths, observed behavior, and pass/fail result here. Mark the three phase 5 boxes complete only after all their required behavior is observed. If artifact rendering fails, record the inline fallback as working separately from the artifact UI failure.

## Platform references

The compatibility review used official documentation for [rules](https://www.antigravity.google/docs/rules/), [skills](https://www.antigravity.google/docs/skills/), [subagents](https://www.antigravity.google/docs/subagents/), and [artifacts](https://www.antigravity.google/docs/artifacts/). These describe the platform capabilities; they do not prove this workspace's behavior. The built-in `research` agent is described as codebase-oriented, so web verification must check its available tools and fall back to the main session when necessary.

## ChatGPT Work support (2026-10-12)

The shared skills now select interactions by platform. AGY retains `ask_question`, research dispatch, and its two conversation artifacts. Local Work uses chat quizzes, pending-question entries in the existing log, linked map/notes files, and direct source verification. The schemas and review intervals are unchanged. `AGENTS.md` adds a local OpenAI learning entry point; direct skill selection remains the fallback.

Official OpenAI references: [Build skills](https://learn.chatgpt.com/docs/build-skills), [Projects and chats](https://learn.chatgpt.com/docs/projects), and [Get started with Work](https://learn.chatgpt.com/docs/get-started-with-work). These establish platform capabilities, not successful discovery or rendering on this device.

Static verification: all four shared skills pass the skill-creator frontmatter validator, and `git diff --check` passes. The original pi folders and AGY always-on rule have no changes. No synthetic learner state was created. Live Work interaction and cross-platform resume remain pending below.

### Live Work acceptance checks (pending)

Use a separate disposable copy of the workspace for synthetic learner answers. Do not add simulated mastery to real `topics/` files.

1. Attach that folder as the primary local Work project. Start with "teach me binary numbers" and separately with explicit selection of the repo's teach skill. Record discovery and explicit invocation separately. A repo-maintenance request must not start onboarding.
2. Supply a concrete goal, answer a probe correctly, and then miss or choose "Don't know". Each quiz must end the turn, without a solution or recommended answer, and the next reply must grade the actual answer and save it. Pending questions must not change mastery.
3. Inspect the saved map and plan. The tutor must wait for approval with phase `plan`; a default choice, elapsed time, or a new chat must not approve it. Approve and complete two nodes with a pass and a miss; verify the same status, local dates, and 1-day intervals as the AGY checklist.
4. Check map/notes links resolve to the active topic's files, and inspect preview/inline rendering. If Mermaid doesn't render, record the readable dependency-list fallback separately. No guessed AGY artifact path or duplicate cloud progress may be created.
5. Start a new Work chat with "continue binary numbers" while a quiz is pending. Recover its prompt/options and grade only after an answer. Repeat with an unfinished probe and unapproved plan; neither may silently become `teach`.
6. Invoke review and practice directly. For review, partially answer a batch, including two questions about one shaky node; resume in a new chat, explicitly skip the rest, and verify one aggregate schedule update per node, with any miss taking precedence. Skipped questions have no outcome. For practice, a hinted solution must not establish independent mastery.
7. Switch topics and back; no progress or notes may leak across topics. Then close Work and continue the same topic in AGY. Verify the same saved phase, node status, and dates, and that AGY still uses its native modal/artifact interactions. Do not run both sessions against the topic simultaneously.
8. In a task without local folder access, confirm the tutor explains the missing access before promising persistent progress.

Record the app version, execution mode, observed skill discovery, rendering, state paths, and pass/fail for each live check. This Codex maintenance chat cannot verify a separate Work UI session; leave those checks pending until observed.
