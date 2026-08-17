---
name: qr3-quickstart
description: Richtet qr3.app ein und erstellt den ersten verlässlichen URL-QR-Code über API, CLI oder SDK. Verwenden, wenn ein Nutzer qr3.app erstmals nutzt oder einen API-Key testen möchte.
---

# qr3 Schnellstart

## 1. API-Key schützen

Erstelle einen Secret Key im Dashboard und setze ihn nur lokal als
Umgebungsvariable. Niemals in ein Repository, eine Skill-Datei oder eine
Ausgabe kopieren.

```bash
export QR3_API_KEY='qr3_sk_…'
```

## 2. Ersten QR-Code erstellen

```bash
curl -X POST https://qr3.app/v1/codes \
  -H "Authorization: Bearer $QR3_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"type":"url","url":"https://example.com","is_dynamic":true}'
```

Alternativ unterstützt die aktuelle CLI URL-Codes:

```bash
qr3 create https://example.com --title "Mein erster QR-Code"
```

Oder mit dem TypeScript SDK:

```ts
import { QR3 } from "@qr3/sdk";

const client = new QR3({ apiKey: process.env.QR3_API_KEY! });
const code = await client.codes.create({ type: "url", url: "https://example.com" });
```

## 3. Ergebnis prüfen

Die Antwort enthält das QR-Code-Objekt mit Kurz-URL und Bild-Links. Scanne den
Code auf einem echten Gerät und prüfe Ziel, Lesbarkeit und erwarteten Status.
Weiterführende Endpunkte, Fehler und Rate Limits stehen in
[docs.qr3.app/de/api](https://docs.qr3.app/de/api/).

Nächste Schritte: Skill `qr3-create` oder Skill `qr3-api`.
