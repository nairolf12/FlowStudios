# finFlow · Produktwebsite – flowstudios.dev

## Dateien
```
index.html          ← Hauptseite
datenschutz.html    ← Datenschutzerklärung (eigene URL)
CHANGELOG.md        ← Versionshistorie
.nojekyll           ← GitHub Pages: kein Jekyll
downloads/
  finFlow-Setup-3_3_0.exe   ← Windows Installer ✅
```

## macOS-Installer hinzufügen
1. `finFlow-3.3.0-Installer.dmg` in `downloads/` legen
2. In `index.html` im `V`-Block: `dlMac:"downloads/finFlow-3.3.0-Installer.dmg"`
3. Committen

## Neue Version
1. Neue Installer in `downloads/` ersetzen
2. `V`-Block in `index.html` aktualisieren (v, dlMac, dlWin, date)
3. `CHANGELOG.md` ersetzen + `datenschutz.html` Stand-Datum anpassen
4. Committen → GitHub Pages deployt automatisch
