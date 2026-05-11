# Mainfranken – E-Mail-Signatur Generator

Statische Web-App, mit der Mitarbeiter ihre persönliche E-Mail-Signatur nach Corporate-Design-Standard generieren und mit einem Klick in Outlook übernehmen.

## Funktionsumfang

- Live-Vorschau der Signatur beim Tippen
- Editierbare Felder: **Name, Position, Telefon, E-Mail**
- Feste Firmendaten: **Logo, Fax, Adresse, Website** (zentral in `index.html` gepflegt)
- Kopieren als Rich-Text (HTML) + Plain-Text in die Zwischenablage – direkt einfügbar in Outlook
- Logo als Base64-PNG im kopierten HTML eingebettet (volle Outlook-Kompatibilität, auch offline)
- Tabellenbasiertes Signatur-Layout mit Inline-Styles – funktioniert in Outlook 2016/2019/2021/365 Desktop, Outlook on the Web und Outlook for Mac
- Werte werden lokal im Browser gespeichert (localStorage)
- Font: **Source Sans 3** (Google Fonts) mit Fallback auf Calibri/Arial für Outlook

## Lokale Vorschau

```bash
# Einfach Datei im Browser öffnen
open index.html
# oder per lokalem Server
python3 -m http.server 8000
# → http://localhost:8000
```

## Deployment auf GitHub Pages

1. Neues Repository erstellen, z. B. `mainfranken-signature`
2. Inhalte dieses Ordners pushen:
   ```bash
   git init
   git add .
   git commit -m "Initial commit – E-Mail-Signatur Generator"
   git branch -M main
   git remote add origin git@github.com:<ORG>/mainfranken-signature.git
   git push -u origin main
   ```
3. In GitHub: **Settings → Pages**
   - Source: **Deploy from a branch**
   - Branch: **main** / Folder: **/ (root)**
   - Speichern
4. Nach ca. 1 Minute ist die Seite erreichbar unter
   `https://<ORG>.github.io/mainfranken-signature/`

> Die Datei `.nojekyll` verhindert, dass GitHub Pages die Inhalte über Jekyll verarbeitet.

## Einbindung in Outlook (Anleitung für Mitarbeiter)

1. Seite öffnen, **Daten ausfüllen** und auf **„Signatur kopieren"** klicken.
2. In Outlook: `Datei → Optionen → E-Mail → Signaturen…`
3. **„Neu"** klicken, Name vergeben (z. B. „Standard").
4. In das Bearbeitungsfeld klicken und mit `Strg + V` (Mac: `Cmd + V`) einfügen.
5. Speichern. Optional: als Standardsignatur für neue Nachrichten / Antworten festlegen.

## Firmendaten ändern

Feste Firmendaten (Fax, Adresse, Website) sind im JavaScript-Block am Anfang von `index.html` definiert:

```javascript
var COMPANY = {
  fax:        '+49 9324 9815969',
  address:    'Lorettoweg 2, 97337 Dettelbach, Germany',
  website:    'mainfranken-reinigungsdienst.de',
  websiteUrl: 'https://mainfranken-reinigungsdienst.de'
};
```

Default-Werte für neue Mitarbeiter (werden überschrieben sobald jemand etwas eingibt):

```javascript
var DEFAULTS = {
  name:     'Nenad Kunst',
  position: 'Gesellschafter',
  tel:      '+49 9324 981590',
  email:    'n.kunst@mainfranken-reinigungsdienst.de'
};
```

## Logo austauschen

Logo ist als Base64-PNG inline in `index.html` eingebettet (Variable `LOGO_DATA`). So funktioniert es auch in Outlook ohne externen Bildverweis.

Zum Austauschen:

```bash
# 1. Neues PNG in 600×252 px erzeugen (3× Anzeigegröße für Retina)
# 2. Base64-encoden:
base64 -w 0 neues_logo.png > logo_b64.txt
# 3. Wert von LOGO_DATA in index.html durch
#    "data:image/png;base64,<INHALT_VON_logo_b64.txt>" ersetzen
```

Das SVG unter `assets/logo.svg` wird nur in der Live-Vorschau im Header der Web-App verwendet, nicht im kopierten Signatur-HTML.

## Technische Hinweise

- **Keine Build-Tools nötig** – reines HTML/CSS/JS, ein einziges File deploybar.
- **Keine Dependencies** außer Google Fonts (nur für die Web-Ansicht).
- **Kompatibilität:** Chrome / Edge / Firefox / Safari (modern, mit Clipboard-API). Für ältere Browser ist ein `execCommand('copy')`-Fallback eingebaut.
- **Datenschutz:** Es werden keine Daten an einen Server übertragen. Eingaben bleiben ausschließlich im Browser des Mitarbeiters (localStorage).

## Lizenz / Eigentum

© Mainfranken Reinigungsdienste GmbH – interne Nutzung.
