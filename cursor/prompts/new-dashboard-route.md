# Prompt-Template: Neue Dashboard-Route (Remix)

**Verwende dieses Template für neue Seiten im Dashboard (`app.qr3.app`).**

---

Erstelle eine neue Remix-Route im qr3-Dashboard:

**Route:** `/dashboard/{ROUTE_NAME}` (z. B. `/dashboard/billing`, `/dashboard/settings`)
**Datei:** `packages/dashboard/app/routes/dashboard.{route_name}.tsx`

## Pflicht-Struktur

```typescript
// 1. Loader: Daten vom API Worker laden
export async function loader({ request }: LoaderFunctionArgs) {
  const { userId } = await getAuth({ request });
  if (!userId) return redirect("/sign-in");

  const apiKey = getApiKey(request); // aus Session/Cookie
  const data = await ApiClient.get{Resource}(apiKey);
  return json({ data });
}

// 2. Action: Formulare verarbeiten
export async function action({ request }: ActionFunctionArgs) {
  // Formulardaten → API Worker
}

// 3. Default Export: UI-Komponente
export default function {RouteName}Page() {
  const { data } = useLoaderData<typeof loader>();
  return <main>...</main>;
}
```

## i18n: neue Übersetzungsschlüssel

Jeder sichtbare String landet in **allen 24** Locale-Dateien des Dashboards —
ein Paritätstest erzwingt identische Schlüsselmengen und bricht sonst vor dem
Push. Quelle ist `de`, `en` ist Pflicht, die übrigen 22 laufen über die
Übersetzungspipeline.

## Sidebar-Navigation aktualisieren

Wenn die Route in der Sidebar erscheinen soll:
`packages/dashboard/app/routes/dashboard.tsx` → `navItems`-Array erweitern.

## Rollen

Affordanzen (Buttons, Menüpunkte) werden über die Mitglieder-Rolle gesteuert;
unbekannte Rollen fallen offen durch. Eine neue Route mit schreibender Aktion
prüft die Rolle im Loader **und** in der Action, nicht nur in der UI.

## Tests

Neue Route → Unit-Test in `packages/dashboard/tests/`:

```typescript
it("route logic: {beschreibung}", () => { ... });
```

E2E-Test in `e2e/dashboard.spec.ts`:

```typescript
test("{route name} page is accessible", async ({ page }) => {
  await page.goto("/dashboard/{route}");
  await expect(page.getByRole("main")).toBeVisible();
});
```
