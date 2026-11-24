# AGENTS Instructions

## Scope

Cofferdam's public Agent Skills: `skills/<name>/SKILL.md`, one folder per skill. This repository is public and is read by outside developers' coding agents.

## Rules

- **Do not edit `skills/` by hand.** The files are written by the publisher in Cofferdam's workspace (`scripts/skills/publish.mjs` there) from the capability catalogue, which is the source. Change the catalogue, run the publisher, review the diff here, commit here. A hand edit is overwritten by the next publish.
- A skill is thin: it holds what an agent must know before it reads the docs (what is built, what is planned, what never to do) and sends the agent to `https://cofferdam.xyz/llms.txt` for the rest. No copied documentation and no long code samples.
- One fact per bullet, tagged `[built]` or `[planned]`.
- The pattern, never the deployment: no cost, size, region, limit, incident, internal host name or roadmap date.
- No skill says an assistant hires, awards or pays for a person. An agent drafts and integrates; a person signs.
- English only. Every skill is MIT and says so in its frontmatter.
- The website republishes these skills at `/.well-known/agent-skills/` from a pinned commit of this repository, so a change reaches cofferdam.xyz when the site's pin moves.

## Git

Commit with `git commit -s`, and with a `Co-Authored-By: Claude <model> <noreply@anthropic.com>` trailer naming the model when an agent wrote the change.
