---
name: learn-review
description: Review shaky and due nodes with batched quizzes and spaced repetition, saving outcomes in the topic map and log. Use when the learner asks to review, be quizzed on a topic, or refresh earlier material.
---

# Review (spaced repetition)

Read the `teach` skill's quiz rules first: the same construction and grading protocol apply here. Review is how knowledge stays locked in, and it is the spaced-repetition piece for languages and vocabulary.

## 1. Pick the nodes

Resolve the topic and restore its state and both artifacts using `teach` first, even when review is invoked directly. Read `topics/<slug>/map.md`. Select taught nodes only, deduplicating by node id:
- all nodes with `status: shaky`, then
- nodes with `next_review` on or before today, oldest first.

If the learner named a specific node or sub-area ("review X"), restrict to that regardless of due dates.

**Use up to 8 questions, usually 5 to 8 when enough nodes are eligible.** A smaller due set gets a smaller batch; do not pad it with unrelated questions. If more are due, take the shaky and most overdue first, and tell the learner how many remain. If nothing is due and nothing is shaky, say so, show the next due date when one exists, and offer to continue or practice instead. If an explicitly requested node is untaught, offer to teach it first.

## 2. Ask, batched

Review is the one place to **batch**: put the questions in a single `ask_question` call (one question per selected node; more for a node that was shaky). Apply the quiz construction rules (bare claims, mutated distractors, randomized correct position, "Don't know" option). Frame questions to test *understanding* (why, derive, apply) rather than recall of wording, since the method is about understanding.

## 3. Grade and update

For each answered question reply with the result block from `teach` (✅/❌, answer, why, what it tells me) and log it immediately. Aggregate results for each node once: any wrong or "Don't know" answer makes the node's batch outcome a miss. Unanswered questions carry no outcome. Then update `map.md` for each assessed node using its interval from before this batch:

| Outcome | status | interval_days | last_reviewed | next_review |
|---|---|---|---|---|
| Correct, previously locked-in | `locked-in` | advance along 1 → 3 → 7 → 14 → 30 → 60, at most once per day | today | today + interval |
| Correct, previously shaky | `locked-in` | 1 | today | tomorrow |
| Wrong or "Don't know" | `shaky` | 1 | today | tomorrow |

Use the shared scheduling rules in `topics/README.md`, including recovery from shaky status. Keep the mermaid classes, next node and updated date in sync; refresh both artifacts after saving each node's outcome.

## 4. Re-teach what broke

For each missed node, don't just move on: give a brief re-explanation using the teach principles (what was the misconception, how could it have been discovered), ask one fresh follow-up question, and record the outcome. A successful corrective retry can restore `locked-in`, but stays at 1 day; it never advances the spacing interval again in the same session. If a miss reveals a foundation problem, treat it like a failed quiz-check in `teach`: fix the foundation before building on it.

## 5. Wrap up

Show a short recap: ✅ n / ❌ n, nodes now shaky, and the next review dates. Offer to continue with the next untaught node.
