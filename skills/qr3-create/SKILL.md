---
name: qr3-create
description: Create qr3.app QR codes via the documented REST API, the SDK, or the URL-oriented CLI. Use when a user wants to generate a QR code or pick the right QR type.
---

# QR-Codes erstellen

Die unterstützten Payload-Formen und Pflichtfelder stehen in
[docs.qr3.app/de/api](https://docs.qr3.app/de/api/). Dokumentierte REST-Payloads
gibt es für `url`, `vcard`, `wifi`, `email`, `sms` und `location`. Für physische Medien ist ein dynamischer
`url`-Code meist sinnvoll, weil das Ziel später per `PATCH /v1/codes/:id`
aktualisiert werden kann.

```bash
curl -X POST https://qr3.app/v1/codes \
  -H "Authorization: Bearer $QR3_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"type":"url","url":"https://example.com/menu","is_dynamic":true}'
```

Für REST sind nicht-URL-Payloads flach: Beispielsweise verwendet vCard
`vcard_first_name` und WiFi `wifi_ssid`, `wifi_password` sowie optional
`wifi_encryption`. Die Pflichtfelder stehen im API-Contract; keine Felder aus
älteren Skill-Versionen erraten.

## SDK

```ts
import { QR3 } from "@qr3/sdk";

const client = new QR3({ apiKey: process.env.QR3_API_KEY! });
const code = await client.codes.create({
  type: "url",
  url: "https://example.com/menu",
  is_dynamic: true,
});

const batch = await client.codes.batchCreate({
  codes: [
    { type: "url", url: "https://example.com/a" },
    { type: "url", url: "https://example.com/b" },
  ],
});
```

Die SDK-Methode heißt `codes.batchCreate({ codes })`, nicht `codes.batch(...)`.
Das ist bewusst eine SDK-Oberfläche: Der SDK-Adapter überführt `codes` und
verschachtelte SDK-Eingaben in den flachen REST-Request mit `items`.

## CLI

Die aktuelle CLI erstellt über `qr3 create` einen URL-orientierten Code:

```bash
qr3 create https://example.com/menu --title "Speisekarte" --tags gastro,menu
```

Verfügbare allgemeine Optionen sind `--title`, `--tags`, `--static`, `--expires`
und `--json`; die CLI hat weder `--type` noch `--wifi-ssid`- oder
`--vcard-*`-Optionen. Für
Batch-URL-Codes gibt es `qr3 bulk create --csv codes.csv`; für andere Typen
nutze die API oder das SDK mit dem kanonischen Payload.

Siehe auch: Skill `qr3-api` und Skill `qr3-dpp`.
