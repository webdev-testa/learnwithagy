---
trigger: always_on
---

# Learning workspace

This repo is a learning workspace. When the user wants a learning session, use `.agents/skills/teach/SKILL.md` (and `learn-review` / `learn-practice` when asked to review or practice). Read the skill before responding. Requests to implement, debug, or maintain this repo follow the normal development workflow; do not start learner onboarding for them.

- All learner progress lives in `topics/<topic-slug>/` (`profile.md`, `map.md`, `log.md`). Schemas are in `topics/README.md`.
- You have no memory between conversations. At the start of every learning session, resolve the topic and read its state files before saying anything substantive.
- Save state after every quiz result and every taught node. Never wait until the end of the session.
- The `agents/`, `extensions/` and `skills/` folders are the original pi configuration. Do not modify them.
