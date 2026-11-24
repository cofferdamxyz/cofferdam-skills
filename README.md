# Cofferdam skills

[Agent Skills](https://agentskills.io) for building with [Cofferdam](https://cofferdam.xyz). A skill is a short file that tells a coding agent what is built, what is planned, and where to read the current documentation before it writes code.

## Install

With the `skills` CLI, into Claude Code, Cursor, Codex and the other agents it supports:

```bash
npx skills add cofferdamxyz/cofferdam-skills
```

The website will publish the same skills for agents that read the well-known index, once the developer docs are live there:

```bash
npx skills add https://cofferdam.xyz
```

## Skills

| Skill | Use it when |
| --- | --- |
| [`cofferdam-sdk`](skills/cofferdam-sdk/SKILL.md) | A project adds "Sign in with Cofferdam" with `@cofferdam/sdk` or verifies a session on its server. |

## How the skills are written

- **They point at the docs, they do not copy them.** The SDK is pre-release and changes, so a skill sends the agent to <https://cofferdam.xyz/llms.txt> and to each page's Markdown (its address plus `.md`), and says to prefer those pages over what a model remembers.
- **One fact per bullet, tagged `[built]` or `[planned]`.** An agent must treat a planned fact as not available and never write code that depends on it.
- **Nothing here acts for a person.** A skill teaches an agent to write an integration. Moving money takes the person's own passkey confirmation on a Cofferdam surface.

The documentation itself, and the MCP server that searches it, are in [cofferdam-docs](https://github.com/cofferdamxyz/cofferdam-docs).

## Contribute

Corrections are welcome: open an issue. The files under `skills/` are published from Cofferdam's own catalogue, so a change made only here is overwritten by the next publish; an issue that says what is wrong, and where in [cofferdam-sdk](https://github.com/cofferdamxyz/cofferdam-sdk) or the docs the right answer is, gets it fixed at the source.

## Licence

[MIT](LICENSE).
