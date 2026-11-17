# qr3-skills

Öffentliches Agent-Plugin von qr3.app (Spezifikation Agent Plugins v1.0.0, Skills-only): `plugin.json` ist das Manifest, `skills/<name>/SKILL.md` sind sieben Skills (`qr3-api`, `qr3-create`, `qr3-dpp`, `qr3-event`, `qr3-gastro`, `qr3-quickstart`, `qr3-retail`) für QR-Codes, REST-API und EU Digital Product Passports. Kein Build, keine Tests, kein Workflow.

- Die `SKILL.md`-Dateien sind Programmeingabe für Agent-Clients, kein Handbuch für dieses Repo: Der Inhalt wirkt bei jedem Nutzer, der das Plugin installiert (`npx skills add ostheimer/qr3-skills`, laut README). Frontmatter `name` entspricht dem Ordnernamen.
- Versionierung: Git-Tag entspricht `version` in `plugin.json` (bisher `v1.0.0`, Tag existiert).
- Bewusst kein `mcp.json`: Der Remote-MCP `https://mcp.qr3.app/mcp` ist eine getrennte, clientverwaltete Verbindung mit eigenem Bearer-Key. Zugangsdaten gehören nie ins Repo.
- Falle: Die API-Basis-URL ist `https://qr3.app/v1`, nicht `api.qr3.app` (steht so in `skills/qr3-api/SKILL.md`).
