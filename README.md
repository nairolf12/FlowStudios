# finFlow · Produktwebsite

Statische Website für finFlow – gehostet via GitHub Pages.

## Enthaltene Dateien

```
index.html                              ← Website
CHANGELOG.md                            ← Versionshistorie
.nojekyll                               ← GitHub Pages: kein Jekyll
downloads/
  finFlow-Setup-3_3_0.exe              ← Windows Installer (25 MB) ✅
```

## macOS-Version hinzufügen

Sobald die `.dmg`-Datei bereit ist:
1. `finFlow-3.3.0-Installer.dmg` in den `downloads/`-Ordner legen
2. In `index.html` ganz oben den `V`-Block anpassen:
   ```js
   dlMac: "downloads/finFlow-3.3.0-Installer.dmg",
   ```
3. Committen → fertig

## Deployment

1. **Repo anlegen** auf github.com (z.B. `flowstudios/finflow-website`)
2. **Alle Dateien pushen** (inkl. `.nojekyll` und `downloads/`-Ordner)
3. **Settings → Pages → Source: main / (root) → Save**
4. Ca. 1–2 Minuten warten → Seite ist live

## Neue Version veröffentlichen

1. Neue Installer-Dateien in `downloads/` ersetzen
2. `V`-Block in `index.html` aktualisieren (v, dlMac, dlWin, date)
3. `CHANGELOG.md` ersetzen
4. Committen → GitHub Pages deployed automatisch

## Impressum

Vor dem Go-Live echte Kontaktdaten eintragen:
```js
pubStreet: "ECHTE STRASSE 1",
pubCity:   "12345 ECHTE STADT",
```
