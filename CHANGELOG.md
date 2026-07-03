# finFlow – Changelog

*Entwickelt von FlowStudios*

## Version 3.3.0 – Onboarding & Über finFlow

**Veröffentlicht: Juli 2026**
**Status: ✅ Latest Stable**

### Onboarding-Assistent

- Erscheint **einmalig beim ersten Start** nach EULA- und Lizenz-Dialog
- 6-Schritte-Wizard mit animierter Fortschrittsleiste:
  1. **Willkommen** – kurze Einführung, jederzeit überspringbar
  2. **Dein Unternehmen** – Name, Straße, PLZ/Ort
  3. **Kontakt & Steuern** – E-Mail, Steuernummer, Name unter Grußformel
  4. **Bankverbindung** – Bank, IBAN, BIC, Zahlungsziel
  5. **Logo importieren** – optional, mit Vorschau; überspringbar
  6. **Fertig** – Zusammenfassung und Einstiegshinweise
- Alle Eingaben werden direkt in die Datenbank gespeichert (identische Keys wie Einstellungsseite)
- „Überspringen" beendet den Assistenten jederzeit, alle Angaben bleiben erhalten
- Placeholder-Texte in Eingabefeldern verschwinden beim Tippen automatisch
- Nach Abschluss: `onboarding_done=yes` in DB → nie wieder gezeigt
- Hat der Nutzer bereits Daten (Restore/Weitergabe): erscheint der bestehende First-Run-Dialog statt des Onboardings

### „Über finFlow"-Dialog

- Kleiner **ℹ-Button** oben rechts in der Sidebar, immer erreichbar
- Zeigt:
  - App-Logo (falls importiert)
  - **finFlow + Versionsnummer**
  - Publisher: © FlowStudios · Florian Haußmann
  - **Lizenzstatus**: bei aktiver Lizenz „✓ Lizenziert" (grün) + maskierter Schlüssel (`FFLW-XXXX-****-****-****`); im Trial „Testversion" (rot) + verbrauchte Kontingente
  - Im Trial: Button „Lizenzschlüssel eingeben" → springt direkt zu Einstellungen
  - Button „Schließen"

---

## Version 3.2.0 – Trial-Modus

**Veröffentlicht: Juli 2026**
**Status: Stable**

### Trial-Modus (verkaufsfertig)

finFlow ist ab sofort kostenlos testbar – mit klaren Grenzen die zum Kauf motivieren:

| Funktion | Testversion | Vollversion |
|---|---|---|
| Kunden & Artikel | ✓ unbegrenzt | ✓ |
| Rechnungen erstellen | max. **5** | ✓ unbegrenzt |
| Angebote erstellen | max. **3** | ✓ unbegrenzt |
| PDF-Erzeugung | ✓ mit **TESTVERSION**-Wasserzeichen | ✓ ohne Wasserzeichen |
| E-Mail-Versand | ✓ | ✓ |
| Mahnwesen | 🔒 gesperrt | ✓ |
| EÜR-Export | 🔒 gesperrt | ✓ |
| Datensicherung | 🔒 gesperrt | ✓ |
| Lieferschein | 🔒 gesperrt | ✓ |

**Neue Funktionen:**
- **Trial-Statusleiste** in der Sidebar: zeigt verbleibende Rechnungen/Angebote, „→ Jetzt aktivieren"-Link führt direkt zur Lizenz-Einstellung
- **Upgrade-Dialog** bei gesperrten Features: erklärt was fehlt, bietet direkten Link zur Lizenz-Eingabe
- **Wasserzeichen** „TESTVERSION" diagonal auf allen PDFs im Trial (Rechnungen, Angebote, Lieferscheine)
- **Limit-Warnung** wenn das Rechnungs- oder Angebots-Limit erreicht ist, mit Hinweis auf Lizenz-Aktivierung
- Nach Lizenz-Eingabe: Statusleiste wechselt auf „✓ Lizenziert", alle Features sofort freigeschaltet, alle künftigen PDFs ohne Wasserzeichen

**Technische Details:**
- `db.is_licensed()` – prüft ob gültiger HMAC-Schlüssel in DB
- `db.get_trial_info()` – gibt aktuelle Nutzungszahlen und Limits zurück
- `db.trial_blocked(feature)` – gibt True zurück wenn Feature im Trial gesperrt
- `generate_invoice_pdf(..., trial=bool)` – Wasserzeichen-Parameter in allen 4 PDF-Generatoren

---

### Version 3.1.0 – Stable (aktuelle Basis)

 – Lizenzschlüssel-Manager

**Neues Tool: finFlow_KeyManager** (nur für FlowStudios intern)

- **Schlüssel-Manager-App** als eigenständige `.exe` (Windows) bzw. `.app` (macOS), die zusammen mit finFlow gebaut wird
- **Übersicht** aller ausgestellten Schlüssel: Schlüssel, Kunde, E-Mail, Erstelldatum, Status (Aktiv / Gesperrt), Notiz
- **Neuen Schlüssel erstellen**: Kundenname, E-Mail, Notiz eingeben – Schlüssel wird sofort generiert, in die Zwischenablage kopiert und in der lokalen Datenbank `keygen_keys.db` gespeichert
- **Schlüssel sperren / entsperren**: Status eines Schlüssels jederzeit ändern (Dokumentation – technische Offline-Sperrung erfordert App-Update)
- **Schlüssel prüfen**: beliebigen Schlüssel auf Gültigkeit prüfen, bei bekanntem Schlüssel wird Kunde + Status angezeigt
- **Notizen bearbeiten**: Freitextnotiz pro Schlüssel (z.B. „2. Mahnung", „Retoure")
- **Suche**: Echtzeit-Filterung nach Kundenname, E-Mail oder Schlüssel
- **CSV-Export**: alle Schlüssel als Tabelle exportieren

**Lizenzprüfung zurück in finFlow:**
- Nach EULA-Zustimmung wird einmalig ein gültiger Lizenzschlüssel verlangt (als Toplevel auf dem App-Fenster – kein zweites `tk.Tk()`)
- Eingegebener Schlüssel wird in der Datenbank gespeichert, danach kein Dialog mehr
- Lizenzschlüssel in den Einstellungen einsehbar und änderbar

**Build-Skripte aktualisiert:**
- `build_app_windows.bat`: baut jetzt `finFlow.exe` **und** `finFlow_KeyManager.exe`
- `build_app.sh`: baut jetzt `finFlow.app` **und** `finFlow_KeyManager.app`
- Klarer Hinweis im Build-Output: KeyManager enthält das Lizenz-Secret → niemals an Kunden weitergeben

---

### Bugfix 3.0.2 – Dunkelmodus Lesbarkeit

- **SecondaryButton**: Hintergrundfarbe war `BG_MAIN` (beim Programmstart fixiert) statt dynamisch – jetzt `CARD_BG` mit Hover in `ACCENT_BLUE`, funktioniert in beiden Modi
- **styled_entry** (alle Textfelder): Hintergrund war immer `#ffffff` – jetzt `#2a2e42` im Dunkelmodus
- **tk.Text-Widgets** (E-Mail-Vorlagen, Notizfelder): Hintergrund war immer `#ffffff` – jetzt theme-abhängig
- **tk.Entry direkt** (Passwortfelder, Login-Dialog): ebenfalls auf Dark-Mode-Farben umgestellt
- **ttk.Treeview**: Hintergrund war immer weiß – jetzt `#2a2e42` im Dunkelmodus, Heading-Hintergrund angepasst
- **ttk.Combobox**: Feldhintergrund war immer weiß – jetzt theme-abhängig, Textfarbe und Auswahl-Highlight korrekt
- **ttk.Scrollbar**: Hintergrund und Troughfarbe jetzt theme-abhängig
- `_setup_style()` wird jetzt nach `_apply_theme()` aufgerufen, damit alle ttk-Konfigurationen die korrekten Theme-Farben verwenden

---

### Bugfix & Verbesserungen 3.0.1

- **Dunkelmodus – verbesserte Lesbarkeit**: Überarbeitete Farbpalette mit höherem Kontrast. Haupttext jetzt `#f0f2ff` (fast weiß), muted Text `#a8b0d0` (deutlich heller), Karten (`#1e2132`) besser von Hintergrund (`#12141c`) unterscheidbar, Sidebar noch dunkler (`#0d0f16`) für klare Trennung
- **Dunkelmodus-Karte** in Einstellungen nach oben verschoben – jetzt direkt unterhalb der Logo-Einstellung, oberhalb der E-Mail-Einstellungen
- **Logo importieren**: Neue Karte „Logo" ganz oben in den Einstellungen. Beliebiges PNG/JPG importieren per Datei-Dialog – wird dauerhaft im App-Datenordner gespeichert und sofort für alle PDFs (Rechnungen, Angebote, Mahnungen, Lieferscheine) verwendet. Live-Vorschau des aktiven Logos inkl. Dateiname. „Standard wiederherstellen" entfernt das benutzerdefinierte Logo. Alle PDF-Generatoren nutzen jetzt `db.get_logo_path()` statt eines fest kodierten Pfads – das Logo bleibt auch nach App-Updates erhalten

---

## Version 3.0.0

**Veröffentlicht: Juli 2026**
**Status: Stable**

### Dashboard / Startseite
- Neue erste Seite **Dashboard** mit drei KPI-Karten: Offene Rechnungen (Anzahl + Betrag), Überfällige Rechnungen (Anzahl + Betrag, rot markiert wenn vorhanden), Umsatz laufendes Jahr (Rechnungen + manuelle Einnahmen)
- Klick auf eine KPI-Karte springt direkt zur zugehörigen Seite (Rechnungsübersicht / Mahnwesen)
- **Letzte Aktivitäten**: tabellarische Übersicht der 8 zuletzt erstellten Rechnungen und Angebote, farblich nach Typ unterschieden
- **Schnellzugriff**: „+ Neue Rechnung" und „+ Neues Angebot" direkt vom Dashboard aus
- Dashboard wird nach jedem Speichern einer Rechnung automatisch aktualisiert
- Dashboard ist jetzt die Startseite beim Öffnen der App

### Rechnungs-Notizen (intern)
- Jede Rechnung hat jetzt ein **Internes Notizfeld** in der Rechnungsübersicht – sichtbar nur für dich, erscheint nicht im PDF
- Notiz wird beim Auswählen einer Rechnung in der Liste automatisch geladen
- Separat speicherbar per „Speichern"-Button
- Datenbank-Migration: neue Spalte `interne_notiz` wird automatisch zu bestehenden Datenbanken hinzugefügt

### Lieferschein-PDF
- **„Lieferschein PDF"**-Button in der Rechnungsübersicht
- Erzeugt einen vollständigen Lieferschein aus der gewählten Rechnung: Lieferschein-Nr. `LS-JJJJ-NNNN` (abgeleitet aus Rechnungsnummer), alle Positionen mit Menge und Einheit – ohne Preise
- Empfangsbestätigung mit Unterschriften- und Stempelfeld
- Logo, Absenderadresse, Kundendaten, Referenz auf Rechnung und Fußzeile mit Bankdaten
- PDF wird im Rechnungs-Ausgabeordner gespeichert und direkt geöffnet

### Dunkelmodus
- Neuer Toggle **„Dunkelmodus aktivieren"** in den Einstellungen (Bereich „Darstellung")
- Vollständige dunkle Farbpalette: dunkler Hintergrund (#1a1d27), dunkle Karten (#22263a), helle Texte, angepasste Akzentfarben
- Einstellung wird in der Datenbank gespeichert und beim nächsten Programmstart automatisch geladen
- Hinweis „Wird beim nächsten Start aktiv" erscheint direkt nach dem Umschalten

### Technische Änderungen
- Neue DB-Spalte: `invoices.interne_notiz`
- Neue Funktion: `db.get_dashboard_data()` – aggregiert alle Kennzahlen für das Dashboard
- Neue Funktion: `pdf_invoice.generate_delivery_note_pdf()` – Lieferschein-Generator
- Neue Funktion: `app._apply_theme()` – lädt Farbpalette aus DB-Setting vor dem UI-Aufbau
- Neue Klasse: `DashboardPage`
- Neue Theme-Globals: `NAV_BG`, `NAV_TEXT`, `NAV_ACTIVE_BG`, `NAV_ACTIVE_FG`, `DARK_MODE`

---

## Version 2.0 – Stable Release

**Veröffentlicht: Juli 2026**  
**Status: Stable**

### Neue Module

#### Angebote
- Neuer Reiter **„Neues Angebot"**: Angebote mit Kunde, Datum, Gültigkeitsdatum und beliebig vielen Positionen erstellen
- Neuer Reiter **„Angebotsübersicht"**: Alle Angebote auf einen Blick, filterbar nach Status (Offen / Angenommen / Abgelehnt / Abgelaufen)
- Angebote per E-Mail versenden (Anhang auf macOS automatisch, auf Windows manuell hinzufügen)
- **„→ In Rechnung umwandeln"**: Ein Klick – alle Positionen inkl. Rabatte werden übernommen, Rechnung wird direkt angelegt
- Eigener konfigurierbarer Speicherort für Angebots-PDFs (Einstellungen)
- Eigene konfigurierbare E-Mail-Vorlage für Angebote (Einstellungen)

#### Mahnwesen
- Neuer Reiter **„Mahnwesen"**: Zeigt automatisch alle unbezahlten Rechnungen, deren Zahlungsfrist überschritten ist
- Farbliche Kennzeichnung: Orange (überfällig), Rot (länger als 30 Tage überfällig)
- **1. und 2. Mahnung als PDF** erzeugen (inkl. Mahngebühr, neuer Zahlungsfrist, Bankdaten)
- Mahnstufe wird pro Rechnung gespeichert und in der Übersicht angezeigt
- „Als bezahlt markieren" direkt aus der Mahnliste

#### Rabatte
- Optionales **Rabatt-%-Feld pro Position** in Rechnungen und Angeboten
- Rabattspalte erscheint im PDF nur wenn mindestens eine Position einen Rabatt enthält
- Bestehende Datenbanken werden automatisch migriert (Rabatt = 0)

### Sicherheit & Rechtliches

- **EULA / Datenschutzhinweis** beim ersten Programmstart: Nutzungsrecht (Einzelplatz), DSGVO-Hinweis (lokale Datenhaltung), Haftungsausschluss – muss aktiv akzeptiert werden
- Die App startet nur mit einem einzigen `tk.Tk()`-Objekt, was plattformübergreifende Stabilität (insbesondere auf Windows) verbessert

### Einstellungen

- **E-Mail-Versand von Angeboten**: Betreff, Nachrichtentext und Signatur separat konfigurierbar
- Neue Platzhalter für E-Mail-Vorlagen (Rechnungen und Angebote): `{artikel}` (kommagetrennte Positionsbezeichnungen), `{anzahl}` (Gesamtmenge)
- Beschreibungstext für E-Mail-Versand jetzt **plattformneutral** (kein macOS-spezifischer Wortlaut mehr)
- **Speicherort für Angebots-PDFs** separat konfigurierbar
- Angebots-Speicherort wird in der Datensicherung einbezogen

### Datensicherung & Plattformwechsel

- Datensicherung sichert jetzt auch den **Angebots-Ordner**
- **Automatische Pfad-Bereinigung beim Restore**: Nach dem Einspielen einer Sicherung auf einem anderen System (z.B. Mac → Windows) werden ungültige PDF-Pfade automatisch erkannt und auf den neuen Systemstandard korrigiert. Dateien die anhand des Dateinamens im Zielordner gefunden werden, werden automatisch verknüpft; nicht gefundene Pfade werden geleert (Rechnung bleibt erhalten, PDF kann neu erzeugt werden)
- **Plattformübergreifende Datenübertragung** (Mac ↔ Windows) ist damit vollständig unterstützt

### Qualität & Bugfixes

- **Kunden- und Artikel-Dropdowns** in „Neue Rechnung" und „Neues Angebot" aktualisieren sich jetzt sofort nach dem Anlegen oder Importieren – kein Neustart mehr nötig
- **EÜR-PDF Spaltenüberlappung** behoben: Lange Kategorienamen wie „Sonstige Betriebseinnahme" oder „Bareinnahme (ohne Rechnung)" werden korrekt umgebrochen und überlappen nicht mehr mit der Beschreibungsspalte. Alle Tabellen nutzen jetzt die volle Seitenbreite (170 mm)
- **Backup-Fehler** (`module 'database' has no attribute 'create_backup'`) behoben – Backup-Funktionen waren durch einen Refactoring-Schritt entfernt worden
- **PDF-Erzeugung** nach Einführung der Rabatt-Funktion repariert (`sqlite3.Row` unterstützt kein `.get()`)
- **Angebots-PDF Layout**: Kundenanschrift mit korrekter DIN-5008-Einrückung, Summenzeile ohne Zeilenumbruch
- **EULA-Dialog**: Buttons jetzt mit `side="bottom"` gepackt, sodass sie auf Windows-Systemen mit hoher DPI-Skalierung immer sichtbar sind
- **Publisher** in Windows-Installer geändert: „Plotterstueble" → **„FlowStudios"**
- **Datenbankdatei** umbenannt: `plotterstueble.db` → `finflow.db` (automatische Migration beim ersten Start)
- First-Run-Dialog bei erkannter Fremd-Installation überarbeitet

### Technische Änderungen

- Neues Modul `quote_pdf.py`: PDF-Generator für Angebote und Mahnungen
- Neue DB-Tabellen: `quotes`, `quote_items`
- Neue DB-Spalten: `invoices.mahnstufe`, `invoices.mahndatum`, `invoice_items.rabatt`
- Neue Settings-Keys: `quote_email_betreff`, `quote_email_text`, `quote_email_signatur`, `angebote_pfad`

### Bugfix 2.0.1 – Plattformübergreifender Restore (Stable)

- **Cross-Platform-Restore-Fehler behoben** (`[WinError 5] Zugriff verweigert`): Beim Einspielen eines Mac-Backups auf Windows hat `restore_backup` die Zielordner erst *nach* dem Wiederherstellen der Datenbank bestimmt – die wiederhergestellte DB enthielt dabei noch Mac-Pfade (`/Users/florence/...`), die Windows nicht anlegen durfte. Fix: Zielordner werden jetzt *vor* dem DB-Restore bestimmt; nach dem Restore werden alle plattformspezifischen Pfad-Settings (`rechnungen_pfad`, `euer_pfad`, `angebote_pfad`) automatisch geleert; zusätzliche Fallback-Sicherung für den Fall unzugänglicher Pfade

---

## Version 1.0

**Erstveröffentlichung**

### Kernfunktionen

#### Kundenverwaltung
- Kundenstamm anlegen und verwalten (Name, Firma, Adresse, Telefon, E-Mail, Notiz)
- Automatisch fortlaufende Kundennummern
- Kundensuche (Name, Firma, Ort, Ansprechpartner)
- CSV- und Excel-Import mit automatischer Spaltenerkennung (inkl. Telefon), Duplikate werden übersprungen

#### Artikelstamm
- Artikel mit Bezeichnung, Einheit, Standardpreis und Notiz verwalten
- Direkte Übernahme in Rechnungspositionen per Dropdown

#### Rechnungen
- Rechnungen mit beliebig vielen Positionen erstellen
- Automatische Rechnungsnummern (Format `RE-JJJJ-LAUFENDENUMMER`)
- Datumsauswahl per Kalender-Popup (📅) oder manueller Eingabe
- PDF-Erzeugung mit Logo, Absenderadresse, Positionstabelle, §19-UStG-Hinweis, Bankdaten und Zahlungsziel in der Fußzeile
- Rechnung bearbeiten: Positionen, Datum und Kunde nachträglich ändern, PDF wird automatisch neu erzeugt
- Jahresfilter in der Rechnungsübersicht
- Zahlungsstatus (offen / bezahlt) pro Rechnung
- **E-Mail-Versand**: Öffnet Entwurf im Standard-E-Mail-Programm mit Rechnung als Anhang (macOS automatisch, Windows manuell), Betreff und Nachrichtentext konfigurierbar mit Platzhaltern (`{kunde}`, `{rechnungsnummer}`, `{betrag}`, `{firma}`)

#### Betriebseinnahmen (manuell)
- Einnahmen erfassen, die nicht über eine Rechnung laufen (Zinserträge, Bareinnahmen, Erstattungen etc.)
- Jahresfilter und Summenanzeige
- Fließen automatisch in die EÜR ein (getrennt von Rechnungseinnahmen ausgewiesen)

#### Betriebsausgaben
- Ausgaben mit Datum, Kategorie, Beschreibung und Betrag erfassen
- Vordefinierte Kategorien (Material, Miete, Fahrtkosten, Werbung etc.)
- Jahresfilter und Summenanzeige

#### EÜR-Export
- Einnahmen-Überschuss-Rechnung als **Excel** (mehrere Blätter: Übersicht, Einnahmen, Manuelle Einnahmen, Ausgaben) und als **PDF**
- Einnahmen aus Rechnungen und manuelle Einnahmen getrennt ausgewiesen
- Ausgaben nach Kategorie aufgeschlüsselt
- Konfigurierbarer Speicherort

#### Datensicherung
- Vollständige Datensicherung (Datenbank, Rechnungs-PDFs, EÜR-Exporte) als einzelne ZIP-Datei
- Sicherung auf beliebigem Speicherort (externe Festplatte, USB, Cloud-Ordner)
- Datum der letzten Sicherung wird angezeigt
- Sicherung wiederherstellen (mit expliziter Bestätigungsabfrage)
- Datenbank zurücksetzen (mit Tipp-Bestätigung „LÖSCHEN")
- First-Run-Dialog bei erkannter bestehender Installation auf dem gleichen Rechner

#### Einstellungen
- Firmendaten (Absendername, Adresse, E-Mail, Bankverbindung, Steuernummer, Zahlungsziel, Grußformel)
- E-Mail-Vorlage für Rechnungsversand (Betreff, Text, Signatur)
- Konfigurierbarer Speicherort für Rechnungs-PDFs
- Konfigurierbarer Speicherort für EÜR-Exporte

#### Hilfe
- Durchsuchbare FAQ (16 Einträge, Akkordeon-Ansicht)
- Schnellzugriff auf Datenordner und Rechnungsordner im Finder/Explorer
- Diagnose-Infos (Datenpfade, Datensatz-Anzahlen, Systeminfo) mit Kopieren-Button

### Installer & Plattformen

- **macOS**: `.app` + `.dmg`-Installer via `build_app.sh` (PyInstaller + hdiutil)
- **Windows**: `.exe` via `build_app_windows.bat` (PyInstaller), Setup-Installer via `installer_windows.iss` (Inno Setup)
- **Unternehmensneutrale Vorlage** (`finFlow_Vorlage.zip`) zur Weitergabe an andere Unternehmen: leere Datenbank, Platzhalter-Logo, neutrale Standardwerte

---

*finFlow ist eine lokale Desktop-Anwendung – alle Daten bleiben auf dem Gerät des Nutzers, keine Cloud, keine Internetverbindung erforderlich.*
