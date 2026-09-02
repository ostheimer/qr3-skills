# qr3.app Agent Skills

This repository publishes the portable, skills-only agent plugin for
[qr3.app](https://qr3.app). It follows the
[Agent Plugins specification v1.0.0](https://agent-plugins.org/specification)
and contains seven released skills for QR codes, API integration, and EU
Digital Product Passports.

## Installation

```bash
npx skills add ostheimer/qr3-skills
```

Your agent client handles installation, activation, and updates from there —
storage location and permissions differ per client. To inspect the package
locally, or for clients that import from the filesystem, clone it instead:

```bash
git clone https://github.com/ostheimer/qr3-skills.git
```

## What is in the package

| Path | Contents |
|---|---|
| `plugin.json` | agent plugin manifest |
| `skills/` | seven directly discoverable agent skills |
| `cursor/` | build-side configuration — not part of the installable package |
| `AGENTS.md` | instructions for agents that write code for qr3.app |

The seven skills under `skills/` are for agents that **use** qr3.app. The
`cursor/` directory is a curated look at how agents **build** it; the two have
different audiences and are kept apart.

There is deliberately no `mcp.json`. The optional remote MCP server at
`https://mcp.qr3.app/mcp` stays a separate, client-managed connection and needs
its own bearer key. **No credentials are contained in this repository.**

## The seven skills

| Skill | Use when |
|---|---|
| `qr3-quickstart` | someone is new to qr3.app or wants to test an API key |
| `qr3-create` | a QR code has to be generated, or the right QR type picked |
| `qr3-api` | working on authentication, endpoints, pagination, rate limits, errors |
| `qr3-dpp` | battery, textile, or general Digital Product Passport data is needed |
| `qr3-retail` | QR workflows for retail, e-commerce, product packaging |
| `qr3-gastro` | QR workflows for restaurants, cafes, hospitality |
| `qr3-event` | QR codes for registration, schedules, WiFi, feedback, analytics |

Each skill's `description` is the trigger text an agent matches on — it names
the situation, not just the capability.

## Documentation

- [Skill catalogue](https://docs.qr3.app/en/skills/)
- [API reference](https://docs.qr3.app/en/api/reference/)
- [MCP server setup](https://docs.qr3.app/en/api/mcp/)

## Versioning

Git tags match the version in `plugin.json`. The first public version is
`v1.0.0`. Description and documentation changes do not get their own tag —
only changes to the skills themselves do.

## Language

This repository is written in English, because its audience arrives from
English-language search and agent tooling. The qr3.app product documentation is
available in all 24 EU official languages at
[docs.qr3.app](https://docs.qr3.app/).
