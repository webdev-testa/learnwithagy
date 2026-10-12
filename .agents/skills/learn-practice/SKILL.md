---
name: learn-practice
description: Give applied exercises for taught topic nodes, including transfer problems, language writing corrections, and vocabulary drills. Use when the learner asks to practice, requests exercises, or chooses practice from the start card.
---

# Practice

Understanding is built by `teach`; practice makes it usable. Read `../teach/SKILL.md`, including its platform interaction and quiz rules. Resolve the topic and restore its three files and both live views using `teach` first. Only practice **taught** nodes (`locked-in` or `shaky`); never practice `untaught` ones. If there are no taught nodes, offer to start or resume teaching.

## Core loop

1. **Pick** nodes: prefer `shaky`, then recently taught, then the learner's request. Use the platform's preference interaction if their preference matters (focus area, difficulty, how many).
2. **Pose one exercise at a time**, pitched just above the learner's recorded floor (see `profile.md`) and below their ceiling. Prefer **transfer** (apply the idea in a new setting) over repeating the taught example.
3. **Let them attempt it first.** For open-ended answers, let the learner answer in chat; for exercises with a clear right answer, a graded multiple-choice question using the platform's quiz interaction is fine. In Work, log the pending exercise, end the turn, and wait for the actual attempt before showing feedback or a solution.
4. **Give feedback that points back to the graph**: name the node the mistake comes from, and re-derive instead of just stating the correct answer. Hints before answers if they're stuck.
5. **Log it** immediately in `log.md` (exercise gist, outcome, misconception). For an independently completed, graded exercise, update each assessed node's status and dates using `topics/README.md`; misses reset spacing even if the node was already shaky. Hints or a shown solution are not evidence of independent mastery. Repeated exercises advance spacing at most once per node per local day; successful corrective retries stay at 1 day. Keep Mermaid classes and the next node in sync, then refresh both live views.

Keep difficulty adaptive: right answers escalate, misses narrow in.

## Languages

For a language topic (`profile.md` goal mentions a language):

- **Writing practice:** give a short prompt using the taught grammar and vocabulary (e.g. "describe your morning using the past tense"). Return the learner's text corrected: mark each error, explain *why* using the taught rule (link it to the grammar node), then show a corrected version. Keep corrections focused on the taught nodes first, not every slip.
- **Vocabulary drills:** keep vocabulary as small nodes (a theme or a root family per node) in `map.md`; drill them through `learn-review`'s spaced schedule, with recall questions both directions (meaning → word, word → meaning), and use roots and morphology so words connect rather than stay isolated.
- **Limits:** you cannot assess spoken pronunciation. Say so, and suggest a human tutor or a speech tool for that part rather than guessing.

## Wrap up

End with a short recap (what went well, which node needs more work) and suggest a `learn-review` or the next node from `teach`.
