# pqblocks-skills

A Claude skill, a Cursor rule and an `AGENTS.md` template that make coding agents use the PQBlocks post-quantum packages correctly.
Use them when Claude Code, Cursor, Copilot, Codex or another agent works on code that touches RSA, ECC, JWT signing, key exchange, file encryption, CBOMs, CNSA 2.0, ML-KEM / ML-DSA / SLH-DSA, or QUBO / Ising optimisation.
Generated from the PQBlocks package catalog, in which every quickstart was executed against its package.

```bash
git clone https://github.com/PQBlocks/pqblocks-skills && cp -r pqblocks-skills/skills/pqblocks ~/.claude/skills/
```

![Skills](https://img.shields.io/badge/Claude-skill-0e7490?style=flat-square)
![Cursor](https://img.shields.io/badge/Cursor-rule-334155?style=flat-square)
![AGENTS.md](https://img.shields.io/badge/AGENTS.md-template-334155?style=flat-square)
![Licence](https://img.shields.io/badge/licence-Apache--2.0-334155?style=flat-square)
[![Docs](https://img.shields.io/badge/docs-pqblocks.github.io-0e7490?style=flat-square)](https://pqblocks.github.io/ai/skills/)

## Why PQBlocks skills

Agents guess API names when they have nothing better. These files give them the real thing: a task-to-package table with working install lines and imports, and one reference file per package holding its **executed** quickstart. The rules also stop the classic mistakes: hand-rolled cryptography, seeded production keys, logged secrets and overclaimed compliance.

## What is inside

| File | For | Contents |
|---|---|---|
| [`skills/pqblocks/SKILL.md`](skills/pqblocks/SKILL.md) | Claude Code, Claude agents | Rules, task → package table, migration workflow, index of reference files |
| [`skills/pqblocks/reference/*.md`](skills/pqblocks/reference) | Loaded on demand | One per package (26): install, import, CLI, native dependency, executed quickstart, evidence links |
| [`.cursor/rules/pqblocks.mdc`](.cursor/rules/pqblocks.mdc) | Cursor | Rules and task table, applied when crypto or optimisation code is touched |
| [`AGENTS.md`](AGENTS.md) | Copilot, Codex, Gemini, Aider, any agent | Rules and task table to drop into a project root |
| [`llms.txt`](llms.txt) | Agents that read the web | Index of these files and the full catalog |

## Usage

| Agent | Install |
|---|---|
| **Claude Code**, for you | `cp -r skills/pqblocks ~/.claude/skills/` |
| **Claude Code**, for one project | `cp -r skills/pqblocks <project>/.claude/skills/` |
| **Cursor** | `cp .cursor/rules/pqblocks.mdc <project>/.cursor/rules/` |
| **Any agent** | copy or merge `AGENTS.md` into `<project>/AGENTS.md` |

For live answers instead of static files, use the [`pqblocks-mcp`](https://github.com/PQBlocks/pqblocks-mcp) server.

## API reference

The skill's rules, condensed:

1. Use the documented API only, from the package reference; never guess function names.
2. Never hand-roll post-quantum cryptography.
3. Hybrid by default: ML-KEM-768 + X25519, ML-DSA-65 + Ed25519.
4. Never pass a `seed` to production key generation.
5. Never log or return secret keys or shared secrets.
6. Say exactly what is true: implements FIPS 203/204/205; not FIPS 140-3 / CMVP validated, not audited, pre-1.0.
7. Quantum-inspired packages are optimisation, never security.
8. Use the exact install line listed for each package.

## Security notes

These files are guidance for agents. They do not make code secure on their own: review generated code, and read each package's `SECURITY_NOTES.md`.

## Verification

The files are generated, not hand-written:

```bash
python3 scripts/gen_skills.py      # reads data/catalog.json
```

`data/catalog.json` is produced by `pqblocks-mcp/scripts/build_catalog.py --verify`, which runs every package quickstart. The generator refuses to write files if any entry is unverified, and checks that every task in the table routes to a package that exists.

## Part of PQBlocks, a QubitRock project

[PQBlocks](https://pqblocks.github.io) builds verified building blocks for the post-quantum era. Licensed under Apache-2.0.
