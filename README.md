# qr3.app Agent Skills

Dieses Repository veröffentlicht das portable Skills-only-Agent-Plugin von
[qr3.app](https://qr3.app). Es folgt der
[Agent-Plugins-Spezifikation v1.0.0](https://agent-plugins.org/specification)
und enthält sieben freigegebene Skills für QR-Codes, API-Integration und
EU Digital Product Passports.

## Paketinhalt

- `plugin.json` — Manifest des Agent Plugins
- `skills/` — sieben direkt erkennbare Agent Skills

Das Paket enthält bewusst kein `mcp.json`. Der optionale Remote-MCP unter
`https://mcp.qr3.app/mcp` bleibt eine getrennte, clientverwaltete Verbindung
und benötigt einen eigenen Bearer-Key. Es sind keine Zugangsdaten in diesem
Repository enthalten.

## Installation

```bash
npx skills add ostheimer/qr3-skills
```

Installation, Aktivierung und Updates verwaltet danach der jeweilige
kompatible Agent-Client — Speicherort und Freigaben unterscheiden sich je
Client. Zum lokalen Prüfen oder für Clients mit Dateisystem-Import kann das
Paket auch geklont werden:

```bash
git clone https://github.com/ostheimer/qr3-skills.git
```

Weitere Informationen: [qr3.app Skill-Katalog](https://docs.qr3.app/de/skills/)

## Versionierung

Git-Tags entsprechen der Version in `plugin.json`. Die erste öffentliche
Version ist `v1.0.0`.
