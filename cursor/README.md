# Bau-seitige Agenten-Konfiguration

Dieses Verzeichnis ist **nicht** Teil des installierbaren Skill-Pakets. Es
zeigt, wie KI-Agenten in der Entwicklungsumgebung von qr3.app arbeiten — ein
Ausschnitt aus dem privaten Monorepo, kuratiert für die Veröffentlichung.

| Datei | Zweck |
|---|---|
| `skills/qr3-test-runner/SKILL.md` | Testsuite ausführen, Fehler debuggen, Gegenproben fahren |
| `prompts/fix-failing-test.md` | Vorlage: fehlgeschlagenen Test reparieren, ohne ihn aufzuweichen |
| `prompts/new-dashboard-route.md` | Vorlage: neue Remix-Route im Dashboard |

Die Produkt-Skills — wie man qr3.app **benutzt** — liegen eine Ebene höher
unter `skills/` und werden per `npx skills add ostheimer/qr3-skills`
installiert. Die beiden Verzeichnisse haben verschiedene Zielgruppen und
werden nicht vermischt.

Was aus dem Monorepo bewusst nicht hier liegt: Datenbank-Migrationsskills, das
Vorgehen des Sicherheitsaudits und die Regeldatei mit internen Pfaden.
