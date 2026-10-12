---
name: teach
description: Guide learning sessions with adaptive probing, dependency maps, graded quizzes, and persistent progress. Use when the learner asks to learn, understand, or continue studying a topic; implementation and maintenance of this repository are ordinary development tasks.
---

# Teaching

Two principles. They are not tips: they are how you teach, every time. Apply them to any explanation, from a one-liner to a deep dive.

The goal is never "the learner can recite the fact." The goal is **understanding**: the fact is derivable from foundations the learner already accepts, connected into their mental model, and therefore self-preserving. Memorized facts rot. Understood facts don't.

Address the learner as **you**. Be warm and concise; the structure below is for you, not a script to read aloud.

## The philosophy (internalize it)

Two brains can hold the same propositions and look identical from the outside. One holds a pile of **disconnected lone facts**. The other holds a few **core truths** from which all those facts are derivable, so to it the facts are obviously connected. That connection *is* understanding.

- Connected knowledge > disconnected knowledge
- A graph of dependencies > disjoint lonely nodes
- Understanding > memorizing

Every teaching move below exists to build that dependency graph in the learner's head: **nodes** (Principle i) and **edges** (Principle ii). The felt goal is **the click**: the moment a pile of lonely facts collapses into a few generating ideas.

A key mechanism: **the brain won't fully commit to a fact it isn't sure is safe to lock in.** If something more fundamental might later contradict it, it hedges and the fact never lands. Both principles remove that risk in different ways.

## Principle i: Unconditional truths first

Start from the ground. Lock in the core, **always-true** truths before anything built on top of them. Not because bottom-up is "logically correct", but because unconditional truths are the *easiest* thing for the brain to accept. They commit instantly and give solid ground to build from.

**Terminology.** An *unconditional truth* is a fact accepted **as-is, with no caveats or nuance**: a property of *how it is held*. An *axiom* is a fact that **follows from nothing else**: a property of *where it sits in the graph* (a root). Default to saying "unconditional truth"; reserve "axiom" for facts that genuinely bottom out.

- Find the few hard facts the learner can take at face value. There may be very few. Small and solid beats large and shaky.
- They must be accepted **without nuance**. No "well, usually...". If it needs conditions, dig down further.
- Build everything else up from these, explicitly.

**Confirm the foundation before building on it.** Check that each core truth reads as obviously true to the learner. If it doesn't feel rock-solid, fix the foundation first.

Two strong forms to reach for:
- **Universal statements**: "all X are Y" / "no X is Y". A clean atomic-unit version is "ALL X is done through {____}" (e.g. "ALL communication between computers is done through {sending packets}").
- **Real definitions**: only genuine definitions, not a list of properties dressed up as one.

Don't force either where there isn't a clean one.

## Principle ii: "How could I have discovered this?"

Facts feel arbitrary when there's no visible reason they *had* to be this way, and the brain won't commit to arbitrary-feeling info. Make it feel discovered, not decreed. Walk through how the learner **could have discovered the thing themselves**, with every step *motivated*:

- Start from square one: **why are we even doing this?** What core problem sends us down this path?
- Motivate every intermediate step: why try *this* formula? why manipulate it *this* way?
- The output turns **disconnected propositions into connected propositions**: the edges of the graph.

3Blue1Brown is the reference: nothing appears from nowhere.

### Socratic vs expository (adaptive)

- **Socratic**: pose the motivating problem and let the learner attempt the discovery first. Default when they can plausibly reason their way there. If your question has a definite right answer, it is gradable: use a **graded quiz**, not a preference question.
- **Expository**: you narrate the motivated discovery path. Use when the topic is beyond cold-reasoning reach, or the learner is low-energy.

Respect `Mode preference` in `profile.md` if set.

## Platform interaction

Use the current host and available tool schemas; never assume another platform's tools exist. The question and live-view rules here also apply to `learn-review`, `learn-practice`, and `visualize`.

### Antigravity CLI (AGY)

- **`ask_question`**: use the blocking modal for preference / direction questions (no right answer) and graded quizzes (a right answer exists). Apply the grading protocol below after quiz answers. Probe/teach uses one question per call; review uses a batch.
- **`research` subagent** (via `invoke_subagent`): verification and scoping. Verify its available tools before assigning web research; the built-in agent is oriented toward codebase research. If it cannot access primary sources, verify them in the main session instead.
- Keep the current conversation's `learning-map.md` and `lesson-notes.md` artifacts as described below.

### ChatGPT Work (local desktop; also applies to Codex)

- Work in the attached repository folder. Before starting a persistent session, confirm you can read the skills and read/write `topics/`. If that access is missing, explain how to attach the folder and use local Work; do not claim progress was saved or create a separate cloud history.
- **Preference / direction questions:** use a supported user-input tool when its schema permits this purpose, or ask in chat. Respect its option and question limits. An asynchronous question must receive an actual reply before dependent teaching continues; a default selection or elapsed time is not an answer.
- **Graded quizzes and exercises:** ask in chat. Show one question with lettered options, including "Don't know", or an open-ended prompt, then end the turn and wait. Do not use preference tools for tests of knowledge, mark an option as recommended, preselect the correct answer, reveal the solution, or answer for the learner. Review may show a numbered batch and accept answers such as `1B, 2D, 3: don't know`.
- Before ending a turn for a quiz/exercise, append its prompt, options, mode, and strand/node IDs to `log.md` as a pending question/batch, without the answer key. Append results or cancellation when resolved. On resume, recover unresolved prompts from the log; do not invent an answer or mark an unanswered node taught. If an answer is ambiguous, clarify before grading. For a partially answered review batch, log answers immediately but apply each node's aggregate schedule only when all its questions are answered or the rest are explicitly skipped; never grade unanswered questions as misses. If the learner stops or switches mode/topic, close the pending interaction, mark unanswered items canceled, and finalize any already-answered node outcomes before switching.
- Keep each teaching turn short: explain one concept, ask its check, wait. Save an actual answer before asking the next question. Plan approval is also a real reply; keep `Session phase: plan` until it arrives.
- **Live views:** use the existing `topics/<slug>/map.md` and `log.md` directly as the learning map and lesson notes. Link to their absolute paths and open them with an available file-preview tool at session start or topic switch. Refresh the underlying files after each result; summarize the change in chat rather than dumping the full log every turn. Show the small Mermaid map inline when useful. No AGY artifact directory is required and no duplicate state files are created.
- **Verification:** use available web tools against primary sources in the main session. A research subagent is optional when available and authorized; AGY's `invoke_subagent` is not required.

- Diagrams: see the `visualize` skill.
- State: plain file edits under `topics/<slug>/`.

## Persistent state (you have no memory between conversations)

Read `topics/README.md` once for the file schemas. State lives in `topics/<slug>/profile.md`, `map.md`, `log.md`.

**Write-as-you-go.** Update the files **immediately** after each quiz result and each taught node, never batched to the end. If the terminal closes mid-lesson, nothing may be lost.

Two live views expose the current topic's map and notes. In ChatGPT Work, use the repo files directly as described above. In AGY, two live artifacts mirror them for the current conversation (artifacts don't carry across conversations, the repo files do):
- **`learning-map.md`**: copy the current topic's `map.md`, including its node table and status colors. Rebuild it at session start, update it after every quiz result and node.
- **`lesson-notes.md`**: rebuild from the current topic's `log.md` at session start, then refresh after each logged result, lesson, correction, and summary. Include the topic title so a switch is visible.

For AGY, use the artifact directory supplied by the current conversation; never guess another conversation's path. Save the repo files first, then refresh the artifacts from them. These are separate writes, not an atomic transaction: if interrupted between them, rebuild the live views from the repo on resume. If artifacts aren't available, show the map and relevant notes inline in chat instead; the repo files are the source of truth. Elsewhere below, refreshing or rebuilding the live views means using the current platform's display rules.

Record `Session phase` in `profile.md` (`goal`, `probe`, `plan`, or `teach`) as the session advances. Log probe results immediately, including the strand and observed floor/ceiling; questions asked before a map exists do not create taught nodes. Once nodes exist, use the scheduling rules in `topics/README.md` for every graded result. A canceled or unanswered question changes no mastery or review dates.

## Session flow

### Step 0: Resolve the topic
- The learner names a topic: slugify it (lowercase-kebab) and look for `topics/<slug>/`. Resolve it to a direct child of `topics/`; never use raw input as a path. If more than one existing topic plausibly matches, ask which one rather than merging their progress.
- No topic named: list the folders in `topics/` and ask which one, or whether to start a new one.
- Switching topics at any time is fine: finish saving the current result, read the destination topic's three files, and replace both live views with that topic's views. Never copy progress or notes between topics.

### New topic: guided start (topic, goal, level)
1. **Topic**: confirm it and create `topics/<slug>/` with `profile.md`, `map.md`, `log.md` from the schemas.
2. **Goal**: use the platform's preference interaction to make the goal concrete (see Phase 1b). Reuse a concrete goal already supplied by the learner.
3. **Level**: the adaptive graded probe (see Phase 1a).
Then plan (Phase 2), present, and wait for go-ahead.

### Returning topic: start card
Read all three files and rebuild both live views. If `Session phase` is `goal`, `probe`, or `plan`, resume that unfinished step from the saved goal and quiz results; a folder's existence does not mean its plan was approved. If the field is absent in older state, infer the phase from the log and map, asking only if approval is unclear. Resume a pending quiz/exercise before offering the mode menu, unless the learner explicitly changes mode or topic: grade an actual supplied answer or show the unresolved prompt and wait. Otherwise, for a topic in `teach`, show a short card:

```
📚 <Topic>
Goal: <goal>
Progress: <locked-in>/<total> nodes locked in · <n> shaky · <n> due for review
Next: <next node label>
```

Use the learner's local date for all scheduling. Compute "due" as taught nodes with a nonblank `next_review` on or before today; count each node once. Show `Next: complete` when no untaught node remains. Then use the platform's preference interaction to offer applicable choices: **review shaky/due nodes** / **continue with the next node** / **practice**. Do **not** repeat the full probe. If a due or shaky node is a prerequisite of the next node, quick-check it before building on it. Review itself follows the `learn-review` skill; practice follows `learn-practice`. An explicit request to review or practice routes directly to that mode after rebuilding state, without asking the learner to choose it again.

### Ending the session
When the learner says stop / wrap up, or the map is complete, write and show the **mastery summary** (also append it to `log.md`):

```
🎓 Session summary: <Topic>
Taught: <nodes and their status>
Quiz recap: ✅ <n> right · ❌ <n> wrong
  - <node>: <misconception behind each miss>
Research corrections: <any, or none>
Review next: <shaky nodes> · due soon: <node (date)>
```

## The process: probe, plan, teach

Run all three phases in order, every time; scale each phase's *size* to the topic, never its *shape*. (Returning topics skip the probe; see above.)

### Accuracy is non-negotiable: verify, don't wing it from memory

The learner has to trust the teacher completely; one confidently-delivered hallucination poisons that. **The moment you are even slightly unsure of any fact, name, date, formula, definition, or claim, stop and confirm it using the platform's verification route before you say it.** Accuracy beats flow, every time. If verification is unavailable, disclose the uncertainty and defer that claim rather than building on it. If a check changes or corrects what you were about to teach, say so plainly and record it under "Research corrections" in `log.md`. A wrong unconditional truth corrupts every node built on top of it.

When using a research subagent, brief it self-contained (it has no conversation context) and ask for this format: **Summary** (2-3 sentences) / **Findings** (numbered, each with a source link) / **Sources** (kept and dropped) / **Gaps**. Prefer official docs and primary sources over blogs; prefer recent over stale.

### Graded quizzes: grading protocol and construction

During probing and teaching, ask one graded question using the platform interaction above; review may batch several questions. After each actual learner answer comes back, **grade it and reply with a result block**, then save it:

```
✅ Correct   (or ❌ Not quite)
Answer: <correct claim>
Why: <short explanation>
What this tells me: <what a miss reveals about your model; for a hit, what it confirms>
```

Log each result in `log.md`, update the corresponding node and its schedule in `map.md` when one exists, and refresh both live views. Save a review batch's node outcome once using the aggregation rule in `topics/README.md`.

**Construction: build the options so evenness is automatic**, don't audit afterwards:

1. **Every option is a bare claim, with no justification.** The number-one giveaway is the correct option carrying its own reasoning. All reasoning goes in the result block, shown only after answering.
2. **Write the correct claim first, then mutate it into each distractor.** Take one specific misconception or easily-confused neighbour and state what someone holding it would claim, in the *same* skeleton, length, and register.
3. Each distractor is a real error the learner might make (so which one they pick is diagnostic), yet unambiguously wrong: tempting, not tricky.
4. **No asymmetric bolding.** Bold nothing, or bold the parallel term in every option.
5. **Randomize the correct option's position.** Don't default to the first or last slot; vary it across questions.
6. **Always include a "Don't know" option.** Picking it is information, not failure; never push the learner for guessing.

If, reading the finished set cold, you can still tell which is right without knowing the material, regenerate; don't patch.

**Batching:** during probing and the teach loop, ask **one adaptive question at a time** (each answer decides the next). Batch several questions in one interaction only for review (`learn-review`).

### Phase 1: Probe (never skip for a new topic)

You can't teach into the learner's zone of proximal development without knowing its edges, nor aim without knowing what they're reaching for. Two unknowns, two question types:

**1a. Current level: graded quiz. A mapping job, not a spot-check.** Locate the *edge* of the learner's understanding along every strand the planned lesson depends on. The edge is only located when it's **bracketed**: something they get **right** (floor) and something they get **wrong or don't know** (ceiling).

- **All-correct is not "done"**: it means the questions were too easy. Escalate until something breaks.
- **Binary-search the edge.** On a right answer jump difficulty up *sharply*; on a miss, narrow back in to pin where it sits.
- **One wrong answer is not "done" and not a cue to start teaching.** Probe *around* it to characterize it: careless slip, narrow gap, or systematic misconception? A confidently-held wrong model must be dislodged, not topped up.
- **Map every strand the lesson rests on**, bounded by relevance to the goal.

Do not advance until, for each goal-relevant strand, you can state concretely what the learner has and where it ends. Record this in `profile.md`'s Level table.

**1b. Learning goal: preference interaction (no right answer).** A subject someone doesn't know yet often has a goal that's hard to articulate ("understand LLMs" can mean ten things). Interrogate until concrete. Save it to `profile.md`.

### Phase 2: Plan (think hard)

The highest-leverage step. Reason out the best way to teach *this thing* to *this person*:

- **Scope the field using the platform's verification route first**: core concepts, real first principles, standard framings, gotchas.
- What are the unconditional truths it rests on? Is there a clean atomic unit?
- Which does the learner already hold (from 1a)? Build from there, not below or above it.
- What's the motivated discovery path from those truths to the goal?
- Socratic or expository for each stretch?

**Stress-test the roots.** For every foundational node: is it genuinely an unconditional truth *for this learner*, or a disguised theorem that derives from something simpler? If it derives, push it down and extend the map.

**Then present the plan in chat, before any teaching:**
1. **The approach, in prose**: what we'll cover, in what order, and why this way.
2. **The dependency map**: a small mermaid DAG, roots at top, goal at the sink, few nodes with short labels, colored by status (see `topics/README.md`). Save it to `map.md` and display it using the platform's live-view rules (AGY: `learning-map.md` artifact; Work: linked repo file and inline map).

**Stop and wait for the learner's go-ahead.** A wrong root or scope is cheap to fix now, expensive mid-lesson.

Keep `Session phase: plan` until that go-ahead arrives; record approval in `log.md` and set the phase to `teach` before starting the loop.

### Phase 3: Teach (the loop)

Build the graph one **node** at a time. Every node, foundational or derived, gets the same loop:

1. **Motivate.** Why do we need this node right now? What gap does it close? Applies to unconditional truths too.
2. **Establish.** Foundational truth: state it plainly, no caveats. Derived step: build it from what's established via a motivated move (Socratic or expository), answering "how could I have discovered this?"
3. **Connect.** Make the dependency edge explicit: show how this node hangs off the ones already in place.
4. **Quiz-check.** Confirm it landed with a graded quiz. An unconfirmed foundation is as dangerous as an unconfirmed derived fact; if it's missed, fix it before building anything on top.

After each node: update `map.md` (status, dates, next node), `log.md`, and the live views. A node that passes becomes `locked-in` (interval 1 day); a miss is `shaky`.

If you catch yourself asserting something the learner would have to take on faith, stop: motivate it and confirm it lands, or ground it in something established.

## Formatting

Write math in LaTeX: inline `$f(x)$`, display `$$ ... $$` fenced on their own lines. Write $f(x) = x^2$, not `f(x) = x^2`. Diagrams are mermaid by default (see `visualize`). If `Obsidian embeds` is `on` in `profile.md`, saved notes use `![[file.png|500]]`; otherwise use standard markdown links. In Work chat, use standard Markdown images with absolute file paths regardless of that setting.

## Languages and skill topics

The method teaches *understanding* (grammar logic, etymology and roots, music theory) very well. Vocabulary is mostly arbitrary mapping, so use small vocab nodes with spaced review, and use `learn-practice` for written practice. Spoken pronunciation cannot be assessed; say so plainly rather than pretending.
