---
name: qr3-api
description: Unterstützt die verlässliche Integration der qr3.app REST-API. Verwenden, wenn ein Nutzer Authentifizierung, QR-Code-Endpunkte, Paginierung, Rate Limits oder RFC-7807-Fehler behandeln möchte.
---

# qr3 API-Integration

Die kanonische API-Dokumentation ist [docs.qr3.app/de/api](https://docs.qr3.app/de/api/). Die Basis-URL
ist `https://qr3.app/v1` — nicht `api.qr3.app`. Sende einen API-Key ausschließlich
über den Header `Authorization: Bearer …`; API-Keys gehören nie in Skill-Dateien,
Quellcode oder Logs.

```bash
curl "https://qr3.app/v1/codes?limit=20" \
  -H "Authorization: Bearer $QR3_API_KEY"
```

## Stabil dokumentierte Oberfläche

| Aufgabe | REST-Endpunkt |
|---|---|
| QR-Code erstellen/listen | `POST` / `GET` `/v1/codes` |
| Einen QR-Code lesen/ändern/löschen | `GET` / `PATCH` / `DELETE` `/v1/codes/:id` |
| QR-Codes im Batch erstellen | `POST /v1/codes/batch` mit `{ "items": [...] }` |
| Scan-Rohdaten | `GET /v1/codes/:id/scans` |
| Aggregierte Scan-Statistiken | `GET /v1/codes/:id/stats` |
| DPP erstellen/lesen/validieren | `POST` / `GET` `/v1/dpp`, `GET /v1/dpp/:id`, `POST /v1/dpp/validate` |

Listen verwenden Cursor-Paginierung. Für POST-Anfragen kann ein
`Idempotency-Key` sichere Wiederholungen ermöglichen. Erfolgreiche Antworten
liegen im Standard-Envelope `{ data, meta }`. Behandelte API-Fehler folgen
überwiegend RFC 7807 als `application/problem+json`. Schemafehler einiger
älterer Routen — darunter Listen, Erstellen und Aktualisieren von QR-Codes —
liefern dagegen Status `400` mit einem rohen Zod-Body (`success: false`,
`error`). Die OpenAPI-Referenz dokumentiert die Form pro Operation.

REST-Payloads sind flach: etwa `vcard_first_name`, `wifi_ssid`,
`wifi_password` und `wifi_encryption`. Das SDK kann davon abweichend seine
verschachtelten Eingaben und `codes.batchCreate({ codes })` anbieten; sein
Adapter übersetzt diese öffentliche SDK-Form in den REST-Contract. MCP bleibt
eine separat konfigurierte, Bearer-authentifizierte Oberfläche und ist nicht
Teil dieses noch nicht veröffentlichten Skills-only-Plugins.

## Rate Limits und Fehler

Die aktuellen API-Limits pro Minute sind: Free 30, Pro 120, Business 300,
Agency 600 und Enterprise nach Vereinbarung. Werte aus den Antwort-Headern
`X-RateLimit-Limit`, `X-RateLimit-Remaining` und `X-RateLimit-Reset` haben
Vorrang. Bei `429` nicht blind wiederholen: Warte bis zum Reset und begrenze
parallele Requests.

## Weiterführende Referenz

- Vollständiger Contract und Felder: [docs.qr3.app/de/api](https://docs.qr3.app/de/api/)
- Maschinenlesbarer Contract: `https://qr3.app/v1/openapi.json`
- SDK-Nutzung: Skill `qr3-create`
