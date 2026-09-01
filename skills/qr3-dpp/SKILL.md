---
name: qr3-dpp
description: Create and validate documented qr3.app Digital Product Passports (DPP) with GS1 Digital Link. Use when a user needs battery, textile, or general DPP data, resolver URLs, or QR assets.
---

# Digital Product Passports

Der kanonische DPP-Contract steht in [der API-Dokumentation](https://docs.qr3.app/de/api/)
und im [DPP-Guide](https://docs.qr3.app/de/dpp/). Sende einen vollständigen
und validen DPP-Payload an `POST /v1/dpp`; `POST /v1/dpp/validate` prüft ihn
ohne Persistierung. Die aktuelle API unterstützt die Kategorien `battery`,
`textile` und `general`; für ihre Pflicht- und Spezialfelder hat die kanonische
DPP-Dokumentation Vorrang.

```bash
curl -X POST https://qr3.app/v1/dpp/validate \
  -H "Authorization: Bearer $QR3_API_KEY" \
  -H "Content-Type: application/json" \
  -d @passport.json
```

Ein erfolgreicher DPP enthält die kanonische `gs1_uri` und QR-Assets unter
`data.qr.svg`, `data.qr.png`, `data.qr.pdf` und `data.qr.eps`. Öffentliche
Resolver verwenden `/01/...`; ihr Verhalten hängt vom `Accept`-Header ab.

## SDK und MCP

Im TypeScript SDK heißen die stabilen Methoden `client.dpp.create`,
`client.dpp.list`, `client.dpp.get` und `client.dpp.validate`. Der separat
authentifizierte Remote-MCP kann die registrierten Tools `create_dpp_qr` und
`validate_dpp` bereitstellen. Remote-MCP-Zugriff wird im jeweiligen Client mit
Bearer-Authentifizierung eingerichtet; dieses portable Skills-Paket enthält
keine Zugangsdaten und kein `mcp.json`.

Keine Produkt- oder Compliance-Aussage aus einem Skill als Rechtsberatung oder
Registry-Nachweis behandeln. Geplante Funktionen sind keine aufrufbaren Tools.
Bei Validierungsfehlern die zurückgegebenen Feldfehler korrigieren statt Werte
zu erraten.

Referenzen: [API-Dokumentation](https://docs.qr3.app/de/api/),
[DPP-Guide](https://docs.qr3.app/de/dpp/)
und Skill `qr3-api`.
