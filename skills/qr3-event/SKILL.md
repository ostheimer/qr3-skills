---
name: qr3-event
description: Plant verlässliche qr3.app QR-Code-Workflows für Veranstaltungen. Verwenden, wenn ein Nutzer QR-Codes für Registrierung, Programm, WLAN, Feedback oder nachträgliche Auswertung benötigt.
---

# QR-Codes für Veranstaltungen

## Geeignete Workflows

- Dynamische `url`-Codes für Registrierung, Programm und Materiallinks
- `vcard`-Codes für Teilnehmerkontakt und `wifi`-Codes für Gästezugang
- Ziel eines bestehenden URL-Codes über `PATCH /v1/codes/:id` aktualisieren

Vor dem Druck scannen, Ziel, Kontrast und ausreichend große Darstellung prüfen.
Ein Ablaufdatum oder eine Zieländerung gehört in den dokumentierten API-Contract,
nicht in angenommene CLI-Optionen.

## Betrieb und Auswertung

Tags gruppieren Codes eines Events. Für einen einzelnen Code liefert
`GET /v1/codes/:id/stats` aggregierte Statistiken; Roh-Scans stehen unter
`GET /v1/codes/:id/scans` bereit. Scanner-Kennzahlen sind pseudonymisierte
Scanner-Tage, keine Personen.

## Umsetzung

Erstelle URL-Codes mit `qr3 create <url>` oder mit der API bzw. dem SDK. Für
vCard- und WiFi-Payloads nutze den exakten Contract aus
[der API-Dokumentation](https://docs.qr3.app/de/api/); die aktuelle CLI bietet dafür keine speziellen
`--vcard-*` oder `--wifi-*` Flags.

Siehe auch: Skill `qr3-create` und [docs.qr3.app/de/api](https://docs.qr3.app/de/api/).
