# qr3.app — Anweisungen für KI-Agenten

> Dieses Dokument wird von KI-Agenten gelesen (Claude Code, Codex, Copilot,
> Cursor, Devin, Windsurf). Es ist die **öffentliche, kuratierte Fassung** der
> verbindlichen Arbeitsanweisung aus dem privaten Monorepo von qr3.app.

## Zwei Zielgruppen, zwei Verzeichnisse

| Verzeichnis | Für wen | Was |
|---|---|---|
| `skills/` | Agenten, die qr3.app **benutzen** | Sieben Produkt-Skills: QR-Codes anlegen, API integrieren, Digital Product Passports, Handel, Gastronomie, Events |
| `cursor/` | Agenten, die an qr3.app **mitbauen** | Test-Runner-Skill und zwei Prompt-Vorlagen aus der Entwicklungsumgebung |

Die Produkt-Skills sind für jeden nutzbar. Die Bau-Seite ist eine Vorschau auf
die Arbeitsweise im Monorepo — das Monorepo selbst ist privat.

Installation der Produkt-Skills:

```bash
npx skills add ostheimer/qr3-skills
```

## Projekt

**qr3.app** — Developer-first QR-Code-Plattform mit EU Digital Product Passport
(DPP). Cloudflare-native Stack (Workers, D1, KV, R2), Multi-Tenant, 24
EU-Amtssprachen. Öffentliche Dokumentation: [docs.qr3.app](https://docs.qr3.app/en/).

## Kritische Regeln

Diese Regeln gelten für jeden Agenten, der Code für qr3.app schreibt. Sie
stehen hier, weil jede einzelne mindestens einmal ein Ergebnis verschlechtert
hat.

### 1. Cloudflare Workers ≠ Node.js

Workers verwenden die V8-Runtime mit Web APIs. Verboten:

- `require()`, `fs`, `path`, `Buffer` (Node.js)
- `crypto` (Node.js) → verwende `crypto.subtle` (Web Crypto API)
- `process.env` → verwende Worker-Bindings

### 2. Multi-Tenancy ist nicht verhandelbar

**Jede** Datenbankabfrage läuft über den Workspace-Scope. Eine Abfrage ohne
`workspace_id` in der `WHERE`-Klausel ist ein **Critical Security Bug** — kein
Stilproblem. Die wenigen Ausnahmen (organisationsweite und nutzerweite
Tabellen) sind im Monorepo namentlich festgehalten; im Zweifel gilt: mit Scope.

### 3. Tests vor Code (Agentic TDD)

```
1. pnpm install
2. pnpm test              ← Baseline muss grün sein
3. Tests für neue Funktion schreiben (müssen fehlschlagen)
4. Implementieren (Tests grün machen)
5. pnpm test              ← Volle Regression
6. pnpm typecheck && pnpm lint
7. Commit nur wenn ALLES grün
```

Minimum-Tests pro Feature:

- Jede Funktion: 3 Tests (Happy Path, Edge Case, Error Case)
- Jeder API-Endpoint: 5 Tests (200/201, 422, 401, 404, Workspace-Isolation)
- Jeder Bug-Fix: Regressionstest, der den Bug reproduziert

### 4. Niemals Tests löschen

Wenn Tests fehlschlagen: **Code fixen**, nicht Tests entfernen. Ausnahme: der
Test ist nachweislich falsch — dann mit Kommentar erklären, warum.

### 5. Keine Secrets im Code

Keine API-Keys, Tokens oder Passwörter im Quellcode, in Skill-Dateien oder in
Logs. Ausschließlich über die Secret-Verwaltung des Deploy-Ziels.

### 6. DSGVO: keine rohen IPs

IP-Adressen werden am Rand sofort zu zweckgetrennten, tagesgebundenen
HMAC-Pseudonymen verarbeitet, bevor irgendetwas persistiert wird. Die
Original-IP erreicht **niemals** einen Datenspeicher.

### 7. Ein gedruckter Code darf nie brechen

Dynamische QR-Codes hängen an gedruckter Verpackung, Beschilderung und
Etiketten beim zahlenden Kunden. Jede Änderung an Weiterleitung, Bildausgabe
oder Kurzcode-Auflösung ist additiv und opt-in. Ein Code, der nach einer
Änderung anders aussieht oder nicht mehr auflöst, ist kein Bug, sondern
Ausschuss.

### 8. Sprache

| Kontext | Sprache |
|---|---|
| Dokumentation (`*.md`) | Deutsch |
| Code-Kommentare | Englisch |
| Variablennamen | Englisch |
| Commit-Messages | Deutsch (Conventional Commits) |
| API-Antworten | Englisch |
| UI-Strings | über i18n, in allen 24 Sprachen |

### 9. Messen, nicht schließen

Vor „ist gesetzt", „läuft", „ist deployt" den tatsächlichen Zustand lesen —
nicht aus einem grünen Workflow oder aus dem Quelltext folgern. Eine Messung an
einer Route, einer Datei oder einem Datensatz trägt keine Aussage über „alle":
erst die Grundgesamtheit bestimmen, dann eine Stichprobe über ihre Ränder legen.

## Git-Workflow, öffentlich relevant

- Conventional Commits auf Deutsch.
- Vor dem Push läuft die volle Regression (Format, Tests, Typecheck, Lint) —
  deshalb niemals mit `--no-verify` pushen.
- Ein PR beschreibt, was gemessen wurde, nicht nur, was geändert wurde.

## Was hier bewusst fehlt

Die Fassung im Monorepo enthält zusätzlich: Datenbankschema und -namen,
Migrationspfade, Wrangler-Konfiguration, die Vorgehensweise des
Sicherheitsaudits und die vollständigen Datei-Konventionen. Das ist
Bau-Wissen für ein privates Repository und gehört nicht in eine öffentliche
Kopie.
