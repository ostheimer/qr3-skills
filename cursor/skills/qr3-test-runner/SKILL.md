---
name: qr3-test-runner
description: >-
  Runs, validates, and debugs the qr3.app test suite (Vitest + Playwright).
  Use when executing tests, checking coverage, fixing failing tests, or running
  the regression suite before a commit/deploy.
---

# qr3 Test Runner Skill

Bau-seitiger Skill aus der Entwicklungsumgebung von qr3.app. Er beschreibt,
wie die Testsuite des Monorepos ausgeführt wird — nicht, wie man das Produkt
benutzt (dafür: `skills/`).

## Schnellstart

```bash
# Alle Pakete
pnpm test

# Einzelnes Paket
pnpm --filter @qr3/api test
pnpm --filter @qr3/redirect test
pnpm --filter @qr3/dashboard test

# Mit Coverage-Report
pnpm --filter @qr3/api test:coverage

# E2E (Playwright) — Dashboard muss laufen
pnpm e2e
pnpm e2e:headed    # mit sichtbarem Browser
pnpm e2e:ui        # interaktiver Playwright-UI-Modus
```

## Vor jedem Commit ausführen

```bash
pnpm test && pnpm typecheck && pnpm lint
```

Der Pre-Push-Hook fährt dieselbe Regression noch einmal; er ersetzt die CI
nicht und umgekehrt — deshalb nie mit `--no-verify` pushen.

## Test-Struktur

| Paket | Framework | Typ |
|---|---|---|
| `@qr3/api` | Vitest + `@cloudflare/vitest-pool-workers` | Unit + Integration (Miniflare, echte D1/KV-Bindings) |
| `@qr3/redirect` | Vitest | Unit |
| `@qr3/dashboard` | Vitest | Unit |
| `@qr3/shared` | Vitest | Unit |
| E2E | Playwright (Chromium) | E2E |

## Fehler debuggen

### Alle Fehler sehen (verbose)

```bash
pnpm --filter @qr3/api test --reporter=verbose 2>&1 | grep -E "✓|✗|FAIL"
```

### Einzelnen Test isoliert ausführen

```bash
pnpm --filter @qr3/api test -- --grep "POST /v1/codes"
```

### Eine Testdatei einzeln laufen lassen

Nach Änderungen an globaler Middleware oder modulweitem Zustand die betroffenen
Dateien **einzeln** laufen lassen — die Vollsuite kann reihenfolgeabhängig
verdecken, was im Einzellauf rot wäre.

### Gegenprobe

Ein neuer Test beweist nichts, bis man ihn hat fehlschlagen sehen: den Fehler
gezielt wieder einbauen, den Test rot sehen, dann präzise zurückrollen — nicht
per `git checkout` über die ganze Datei, wenn sie weitere Änderungen trägt.

## Pflicht-Tests pro Feature

Jede neue Funktion braucht **mindestens**:

```
Tests pro Endpoint:
1. 401 ohne Authorization-Header
2. 400 bei invalider Validierung (Zod)
3. 422 bei Business-Logik-Fehler
4. 404 wenn Ressource nicht existiert
5. Workspace-Isolation (anderer Workspace → 404)
```

## Coverage-Ziele

```
lines:     ≥ 90%
functions: ≥ 90%
branches:  ≥ 85%
```

## Playwright Auth-Setup

Für E2E-Tests mit echtem Login:

```bash
E2E_EMAIL=dein@email.com E2E_PASSWORD=passwort pnpm e2e
```

Die Session wird lokal gespeichert und ist gitignored.
