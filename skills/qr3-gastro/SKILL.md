---
name: qr3-gastro
description: Plant verlässliche qr3.app QR-Code-Workflows für Restaurants, Cafés und Gastronomie. Verwenden, wenn ein Nutzer Speisekarten, Gäste-WLAN, Bewertungen oder saisonale Angebote per QR-Code bereitstellen möchte.
---

# QR-Codes für die Gastronomie

## Bewährte Einsatzfälle

- Dynamischer `url`-Code für Speisekarte, Tagesangebot oder Reservierung
- `wifi`-Code für Gäste-WLAN mit dem dokumentierten API-Payload
- Dynamischer Bewertungs- oder Feedback-Link
- Tags zur Trennung von Tischen, Innenbereich und Außenbereich

Für Tischaufsteller einen deutlich beschrifteten, vor Ort getesteten QR-Code
verwenden. Bei Änderungen der Speisekarte das Ziel eines dynamischen Codes
aktualisieren statt ungetestete Neudrucke zu verteilen.

## Verlässliche Umsetzung

```bash
qr3 create https://restaurant.example/menu --title "Aktuelle Speisekarte" --tags gastro,menu
```

Die CLI akzeptiert einen URL-Parameter und allgemeine Optionen, aber keine
`--wifi-ssid`- oder andere typspezifische Flags. Für WLAN und andere
nicht-URL-Typen die REST-API oder das SDK mit den exakten Contracts aus
[der API-Dokumentation](https://docs.qr3.app/de/api/) verwenden.

## Auswertung

`GET /v1/codes/:id/stats` und `GET /v1/codes/:id/scans` liefern die
dokumentierte, workspace-gebundene Auswertung. Scanner-Kennzahlen sind keine
personenbezogenen Besucherprofile.

Siehe auch: Skill `qr3-create` und [docs.qr3.app/de/api](https://docs.qr3.app/de/api/).
