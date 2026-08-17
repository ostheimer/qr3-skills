---
name: qr3-retail
description: Plant verlässliche qr3.app QR-Code-Workflows für Einzelhandel, E-Commerce und Produktverpackungen. Verwenden, wenn ein Nutzer Produktinformationen, Kampagnen oder dokumentierte DPP-Links per QR-Code bereitstellen möchte.
---

# QR-Codes für Handel und Verpackung

## Geeignete Muster

- Dynamische `url`-Codes für Produktdetails, Nachfolgeprodukte und Kampagnen
- Tags für SKU, Produktlinie oder Saison
- `POST /v1/codes/batch` für bestätigte flache Bulk-Payloads mit `{ "items": [...] }`
- DPP nur über den dokumentierten `/v1/dpp`-Contract oder die registrierten
  MCP-Tools `create_dpp_qr` und `validate_dpp`

Vor dem Druck jeden repräsentativen Code scannen, das Ziel prüfen und nach
Änderungen der Produktseite erneut testen. Ein QR-Code ersetzt keine
produktspezifische Kennzeichnungs- oder Compliance-Prüfung.

## SDK und CLI richtig einsetzen

Für Batch-Erstellung im SDK heißt die Methode
`client.codes.batchCreate({ codes })`. Die aktuelle CLI kann URL-Codes mit
`qr3 create <url>` und URL-CSV-Dateien mit `qr3 bulk create --csv <datei>`
erzeugen. Für andere QR-Typen muss der vollständige, dokumentierte API- oder
SDK-Payload verwendet werden; typspezifische CLI-Flags sind nicht vorhanden.
Die SDK-Oberfläche ist nicht der REST-Wire-Contract: Ihr Adapter übersetzt
`codes` und verschachtelte SDK-Eingaben in flache REST-Felder und `items`.

## DPP und Remote MCP

Remote MCP wird separat im jeweiligen Client mit Bearer-Authentifizierung
konfiguriert. Dieses Skills-only-Paket enthält weder Zugangsdaten noch
`mcp.json`; geplante Premium-Funktionen sind keine aufrufbaren Tools.

Referenzen: Skill `qr3-dpp`, Skill `qr3-create` und
[docs.qr3.app/de/api](https://docs.qr3.app/de/api/).
