# Prompt-Template: Fehlgeschlagenen Test reparieren

**Verwende dieses Template, wenn ein Test fehlschlägt.**

---

Repariere den fehlgeschlagenen Test in qr3.app. **WICHTIG: Fixe den Code,
NIEMALS den Test löschen oder abschwächen.**

## Schritte

1. Führe `pnpm --filter {PACKAGE} test --reporter=verbose 2>&1 | grep -A 10 "FAIL"` aus
2. Lies den Fehler genau — ist es ein:
   - **Assertion-Fehler** → Code-Logik ist falsch
   - **TypeError (DB/KV undefined)** → Unit-Test trifft auf DB-Zugriff — Validierung muss davor
   - **ESLint-Fehler** → Code-Style-Problem
   - **TypeScript-Fehler** → Typen anpassen

3. Fix implementieren
4. `pnpm test` — ALLE Tests müssen grün sein

## Goldene Regel

```
Tests fehlschlagen → Code fixen
Tests fehlschlagen → NICHT Tests ändern
```

Ausnahme: Der Test ist **nachweislich falsch** (falscher erwarteter Wert,
dokumentiert) → dann mit Kommentar erklären, warum. Ein eigener Fehler im Test
wird transparent gemacht, nicht durch Aufweichen des Tests versteckt.
