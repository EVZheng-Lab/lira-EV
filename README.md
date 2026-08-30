# Lira — Image Prompt Optimization (EV build)

A Claude Code skill that turns any rough request into a precise, production-ready
AI image-generation prompt — character sheets, locations/environments, prop sheets,
and frame edits (Nano Banana Pro / Seedream 4.5 / GPT Image 2) — without silently
failing on ambiguity.

This is a personal fork of the general-purpose `lira-image-prompts` skill, kept as
its own repo because the character-sheet template has one deliberate change:

| Panel | Upstream behavior | This build |
|---|---|---|
| 3/4-profile close-up (right panel) | body/face turned ~45°, gaze direction unspecified | body/face turned ~45°, **eyes looking directly into the camera lens** — never looking away/outward |

Everything else (routing table, anti-fail rules, formulas, prompt-type templates)
is unchanged from the upstream skill.

## What it produces

| Request type | Output |
|---|---|
| Character | Fixed 3-view turnaround sheet (front/back/3-4-profile, neutral grey backdrop) |
| Location / environment | Camera-anchor-first prompt with light, palette, tech block |
| Prop | Product-shot prompt routed to NBP / GPT Image 2 |
| Edit of an existing frame | Surgical CHANGE / PRESERVE EXACTLY edit prompt, NBP first |

## Install

```bash
git clone <this-repo-url> ~/.claude/skills/lira-EV
```

Or drop it into a project's `.claude/skills/lira-EV` for project-scoped use.

No runtime dependencies — it's a single `SKILL.md` read by Claude Code.

## Usage

Trigger it with any image-prompt request, in any language — you don't need to say
"Lira" by name:

- "write me a character sheet prompt for..."
- "make an NBP edit prompt for this frame"
- "rewrite this prompt for a location shot"

Say "give me the full thing" / "go" to skip clarifying questions (BASIC mode);
otherwise it defaults to asking 2-3 targeted questions on ambiguous/high-stakes
builds (DETAIL mode).

## Design notes

- Character sheets are always the fixed 3-view turnaround — the skill never asks
  whether you want a single portrait or a different backdrop; that structure is
  non-negotiable by design (it's what keeps cross-panel identity consistent).
- No model has a negative-prompt parameter in this skill's worldview — every
  anti-fail rule is phrased as "describe what you want," never "don't."
- Aspect ratio and resolution are treated as platform parameters (set in the
  generation UI), never written into the prompt text.
