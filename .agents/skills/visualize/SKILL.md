---
name: visualize
description: Add a correct, minimal diagram to a lesson when an idea is genuinely clearer as a picture - a dependency graph, flow, sequence, state machine, tree, or comparison. Mermaid inline by default; optional drawio or image generation for richer visuals. Use when teaching and structure or relationships matter more than prose.
---

# Visualize

A picture earns its place only when it shows something words can't: structure, direction, relationship. This skill adds ONE such picture, and makes sure it is **correct**.

For a learning session, read `../teach/SKILL.md`'s platform interaction rules first. AGY keeps its native artifact display; ChatGPT Work uses inline diagrams and linked repo files. Use only tools available on the current host.

## When to visualize (and when not to)

This teaching system builds a **dependency graph in the learner's head**. A visual is powerful exactly when it makes that structure visible. Reach for one when:

- The idea is a **structure or relationship**: dependencies, a system with parts and arrows, a pipeline, a sequence of exchanges, a state machine, a tree, a comparison, a containment.
- A topic's overall shape (the learning map) needs to be seen.

Do NOT visualize when prose or a single equation already carries it. A decorative diagram that restates the sentence next to it adds noise and a chance to be wrong. When in doubt, don't: a missing visual is cheaper than a false one.

## Default: inline mermaid

Write a fenced `mermaid` block directly in your reply. Types: `graph TD`/`LR` (dependencies, flows), `sequenceDiagram`, `stateDiagram-v2`, `erDiagram`, `mindmap`, `timeline`, `classDiagram`. Foundations at the top flowing down to conclusions is usually the natural shape for this system. If the current view does not render Mermaid, show the small map as a readable dependency list and keep the Mermaid source in `map.md`; do not claim an unobserved render succeeded.

## Rules: one idea, fewest elements

The most common failure is **cramming**. Every extra label makes the picture harder to read and easier to get wrong.

1. Before drawing, prune to the fewest elements that carry the idea. For each: *"if I delete this, is the idea still clear?"* If yes, delete it. Aim for **7 nodes or fewer**.
2. Keep labels short: a term or short phrase, not a sentence. Quote labels containing special characters: `id["Label (extra)"]`.
3. **Correctness before beauty.** Re-read the finished diagram: is every arrow pointing the right way? Is every dependency true? A wrong arrow is a failure even if it renders perfectly. If unsure whether an edge is true, omit it (or use `teach`'s platform verification route).
4. Draw only what you've established in the lesson. Don't invent content to fill space.
5. Introduce the diagram in a sentence, then let it carry the idea. Don't narrate every element back in prose.

## Optional: richer visuals

Use only when mermaid genuinely can't express the idea (exact geometry, plots, dense architecture diagrams):

- **`drawio` MCP** (`open_drawio_mermaid`, `open_drawio_xml`): for polished editable diagrams. Read the schema in the MCP directory first.
- **Image generation:** in AGY, use the `image-generator` subagent when available; in Work, use an available image-generation tool and its skill. Only for illustrative images (not for exact or data-bearing diagrams; generated images can be wrong, so never use them for anything that must be factually precise).

### Optional drawio helper

In AGY, when drawio is connected and a richer editable diagram is useful, create a transient helper with `define_subagent`, then dispatch it with `invoke_subagent`. Read those tools' current schemas and the connected MCP tool schemas rather than guessing arguments. Give it only the required drawio capabilities; Mermaid needs no helper. In Work, use connected drawio tools directly when available. If those tools or drawio are unavailable, use inline Mermaid or a verified SVG instead.

Use this self-contained helper brief, filling in the established concept and elements:

```text
Create one editable drawio diagram for <concept>.
Established elements: <short labels and relationships, at most seven nodes>.
Use the connected drawio MCP tools. Show only these elements; do not research,
add lesson content, or edit topics/ state. Check every arrow and label against
the brief and inspect the rendered result if the tools provide a preview.
Return the editable diagram location, a short correctness check, and any
unverified rendering limitation. If you cannot produce a correct diagram,
return RESULT: NONE and the reason.
```

The parent checks the returned diagram before including it in the lesson. A helper without a preview must report rendering as unverified.

Geometry that needs exact coordinates can be hand-written as an SVG file; view it before showing it to confirm it is right.

PNG rendering of mermaid/SVG into a `viz/` folder is **not set up yet** (deferred). If Obsidian embeds are on in `profile.md`, use `![[file.png|500]]` in saved notes; otherwise use a normal markdown image link. In Work chat, always display saved images using standard Markdown with an absolute file path, even when the saved notes use Obsidian syntax.
