# majadobelsek.com – Arbeitsanleitung für Claude

Persönliche, berufliche Website von Dr. med. Maja Dobelsek (Website: „Dr. med.“; im CV: „dr. med. (Univ. Ljubljana)“ – so von Maja entschieden) (Psychiatrie, KI, Forschung).
Maja pflegt die Seite ausschliesslich über Claude: Sie schreibt Wünsche auf Deutsch, Claude setzt um.
Maja codet und designt nicht selbst. Antworten an Maja immer auf Deutsch, kurz, ohne Technikjargon.
Die Website ist zweisprachig: Englisch (Hauptsprache, Root) und Deutsch (Ordner `de/`). **Jede inhaltliche Änderung
immer in BEIDEN Sprachen machen** (Maja schickt Änderungen meist nur in einer Sprache).

## Wie die Seite funktioniert

- Reine statische HTML-Seite, kein Build-Schritt, kein Framework, kein JavaScript ausser Kleinigkeiten.
- Hosting: GitHub Pages aus dem Branch `main`, Ordner `/` (Root). Jeder Push auf `main` ist nach ca. 1 Minute live.
- Repository: `majadobelsek-lgtm/webseite`.
- Herkunft: Design-Artifact „majadobelsek.com Homepage" (Claude Design). Die Seiten wurden 1:1 daraus übernommen.
  Massgeblich ist ab jetzt dieses Repository, nicht mehr das Artifact.

## Ablauf bei jedem Änderungswunsch

1. Repository holen bzw. `git pull`.
2. Änderung machen, dabei die Konventionen unten einhalten.
3. Lokal prüfen: `python3 -m http.server` und Screenshots mit Playwright (Chrome unter `/usr/bin/google-chrome`),
   Desktop 1440 px **und** Mobil 390 px. Kein horizontales Scrollen auf Mobil.
4. Maja einen Screenshot der geänderten Stelle zeigen und kurz beschreiben, was sich ändert.
5. **Erst nach Majas OK** committen und auf `main` pushen. Ausnahme: reine Tippfehler, wenn Maja „direkt live" sagt.
6. Nach dem Push melden: „Ist in ca. 1 Minute live unter majadobelsek.com/…"

Commit-Messages auf Englisch, kurz und beschreibend („Add article: …", „Update about page: …").

## Dateistruktur

```
index.html                  Startseite
research.html               Studie S012026
about.html                  Über mich, Lebenslauf
speaking.html               Vorträge & Zusammenarbeit
courses.html                Swiss Psychiatry Entry Roadmap (vorerst VERSTECKT: alle Links auskommentiert mit `<!-- COURSES HIDDEN ... -->`)
contact.html                Kontakt
impressum.html, datenschutz.html   Rechtliches (Deutsch)
404.html                    Fehlerseite (nutzt absolute Links "/", weil sie unter jeder URL erscheint)
articles/index.html         Artikelübersicht
articles/<slug>.html        Einzelne Artikel. Sonderfall: `ai-scribes-clinical-workflow.html` hat ein eigenes, interaktives Layout
                            (eigenes Stylesheet im <head>, auf Website-Farben/-Schriften umgestellt; Script für Workflow-Animation; PDSQI-8 als statische Bewertungsanleitung)
css/site.css                Gemeinsame Basis-Styles (Body, Fokus, Mobile-Hero)
css/fonts.css, fonts/       Selbst gehostete Schriften (DSGVO: kein Google-Fonts-Abruf)
images/                     Bilder (hero.jpg, portrait.jpg, articles/<slug>.jpg)
favicon.svg
```

Links sind **relativ** (`about.html`, `../about.html` aus `articles/`), damit die Seite sowohl unter
der GitHub-Pages-Adresse als auch unter der eigenen Domain funktioniert. Ausnahme: `404.html`.

## Gestaltung (aus dem Design übernommen)

Alle Layout-Styles stehen **inline** im HTML, so wie im Design. Neue Abschnitte baut man, indem man einen
bestehenden, ähnlichen Abschnitt kopiert und den Text ersetzt. Keine neuen Farben, Schriften oder Abstände erfinden.

Farben:
- Ink `#14212F` – Text, dunkle Bänder, Footer
- Alpine teal `#1D5E5A` – Akzent, Primär-Buttons, aktive Navigation
- Teal tint `#E4EFED` – Kurs-/Abo-Band, Status-Chips
- Copper `#8A5A3F` – Eyebrows (kleine Überzeilen), Nummernringe; auf Dunkel `#DAB49A`
- Paper `#F7F7F4` – Seitenhintergrund · White `#FFFFFF` – Karten, alternierende Abschnitte
- Slate `#4D5966` – Sekundärtext · Line `#DCE1DE` – Rahmen, Trennlinien · Fliesstext `#2F3B48`

Schriften: Überschriften Libre Baskerville 400/500 (negatives letter-spacing), Text DM Sans 400–700.
(Am 8.11.2026 Alternativen getestet, u. a. Plus Jakarta Sans; Maja hat sich bewusst für diese Originalschriften entschieden.)
Eyebrow: DM Sans 11 px, 700, uppercase, letter-spacing 0.2em, Copper.
Buttons: Höhe 50 px, Radius 6 px, ein Primär-Button pro Abschnitt. Karten: Radius 8 px, 1 px Rahmen, keine Schatten.
Inhaltsbreite 1200 px, Artikeltext 680 px.

## Gemeinsame Bausteine (bei Änderung auf ALLEN Seiten anpassen)

- **Header/Navigation**: zwischen `<!-- SITE HEADER -->` und `<!-- /SITE HEADER -->`. Auf jeder Seite gleich,
  nur der aktive Menüpunkt hat `aria-current="page"` und Teal-Farbe. Pfade relativ zur Seite.
- **Footer**: `<!-- SITE FOOTER ... -->`. Grosse Variante nur auf der Startseite, sonst klein.
- Nach Änderungen an diesen Bausteinen mit `grep` prüfen, dass alle Seiten gleich sind.

## Neuen Artikel hinzufügen

Maja schickt Titel, Text (oft als Word/PDF/Docs-Artifact oder eingefügt) und ggf. Bild.

1. Slug wählen: kurz, englisch, Kleinbuchstaben mit Bindestrichen (z. B. `deskilling-next-generation`).
2. Den neuesten Artikel (z. B. `articles/psychiatrist-work-ai-everywhere.html`) nach `articles/<slug>.html` kopieren.
   Referenzliste (`<section aria-label="References">`) bei Bedarf aus dem Git-Verlauf der
   früheren Vorlage `vision-ai-psychiatry.html` übernehmen.
3. Ersetzen: `<title>`, `<meta name="description">`, Eyebrow (Typ · Thema), `<h1>`, Untertitel,
   Datum und Lesezeit (≈ Wörter / 220, aufrunden), Artikeltext, Referenzen, „Cite this essay",
   Share-Links (Slug + URL-kodierter Titel), „Keep reading" (zwei andere Artikel/Seiten).
4. Im Artikeltext nur diese Elemente verwenden, mit den Inline-Styles aus der Vorlage:
   `<p>`, `<h2>`, `<blockquote>` (Pull-Quote), `<ul>/<ol>`, `<sup>` für Referenznummern.
   Absätze kurz halten (3–5 Sätze). Majas Text nicht inhaltlich umschreiben; nur Formatierung,
   Tippfehler auf Rückfrage.
5. Bild (optional): nach `images/articles/<slug>.jpg`, max. 2000 px breit, JPEG Qualität ~80, `alt`-Text setzen.
   Ohne Bild den Bild-Block entfernen.
6. `articles/index.html`: Featured-Block oben auf den neuen Artikel umstellen (immer der neueste) und einen
   neuen `<li>` zuoberst in „All articles" einfügen. Falls der Artikel unter „In preparation" stand, dort entfernen.
7. Optional auf der Startseite verlinken, falls Maja das möchte.
8. Prüfen (Desktop + Mobil), Vorschau zeigen, nach OK pushen.

Mögliche Themen-Filter (Chips oben auf der Artikelseite) sind derzeit nur optisch.

## Bilder

Bilder vor dem Einchecken verkleinern (Pillow): max. 2400 px Breite, JPEG ~80 %, Ziel < 500 KB.
Keine Fotos anderer Personen ohne Majas ausdrückliche Bestätigung, dass sie die Rechte hat.

## Offene Punkte vor dem Launch auf der eigenen Domain

- [x] Echte Fotos: `images/hero.jpg` (Terrasse, Querformat) und `images/portrait.jpg` – eingefügt am 8.11.2026
- [ ] Platzhalter in eckigen Klammern füllen: `grep -rn "\[" --include=*.html .` (Jahre, Datum, Kurs-Sessions, Preis, Sprache, Vorträge, Artikeltext)
- [x] Links: LinkedIn, ORCID, CV-PDF (8.11.2026). Kein Google-Scholar-Profil (Links entfernt).
- [x] Speaker-Kit: Porträt-Download + CV-Link (8.11.2026)
- [x] Kontaktformular über Formspree angebunden (Endpoint `https://formspree.io/f/mzeeqnek`, Konto von Maja, Zustellung an Gmail), 8.11.2026.
      Kurs-Interesse-Formular auf der (versteckten) Kursseite ist noch NICHT angebunden (soll später über Wix laufen).
- Newsletter-Band am 8.11.2026 entfernt (kein Newsletter geplant). Der alte Block steht im Git-Verlauf, falls er zurückkommen soll.
- [x] Impressum: Mindmorph GmbH, Grueben 592, 9243 Jonschwil, UID CHE-144.385.556 (Handelsregister, Stand Mai 2026); E-Mail privat (Gmail)
- [x] Datenschutz: Abschnitt zu Formspree ergänzt
- Kursseite seit 8.11.2026 versteckt (Navigation, Footer, Startseiten-Band, Kontaktseite auskommentiert, Marker `COURSES HIDDEN`). Maja macht sie später fertig; Plan: Schaufenster hier, Kauf/Kurszugang über Wix (z. B. kurse.majadobelsek.com).
- [x] Deutsche Version (9.11.2026), siehe Abschnitt Zweisprachigkeit.
- [x] `noindex` entfernt (9.11.2026), ausser auf der versteckten `courses.html` (beim Freischalten der Kursseite dort auch entfernen)
- [x] Eigene Domain majadobelsek.com live (9.11.2026): Domain bei Wix gekauft, DNS bei Wix (A-Records GitHub Pages, CNAME www → majadobelsek-lgtm.github.io), Custom Domain in GitHub Pages eingetragen. Wix-Abo erst kündigen, wenn alles stabil läuft; Domain-Verlängerung bei Wix behalten.

## Was Claude nicht tun soll

- Nichts direkt pushen, ohne dass Maja die Vorschau gesehen hat (ausser sie sagt es ausdrücklich).
- Keine Tracking-, Analyse- oder externen Skripte einbauen, keine externen Schriften.
- Keine Inhalte über Patient:innen; keine Aussagen im Namen der Psychiatrie St. Gallen.
- Keine Studienergebnisse veröffentlichen, bevor Maja sie freigibt.

## Hinweis: Bilder austauschen

Browser und GitHub speichern Bilder zwischen. Wird ein Bild unter demselben Dateinamen ersetzt, an allen
Verweisen die Versionsnummer erhöhen (`images/hero.jpg?v=2` → `?v=3`), sonst sehen Besucher:innen das alte Bild.

## Lebenslauf (CV)

- Quelle: `cv/cv.html` (Englisch, A4). Daraus wird `cv/Maja-Dobelsek-CV.pdf` erzeugt (Download-Button auf `about.html`).
- Nach jeder Änderung PDF neu erzeugen: Playwright/Chrome `page.pdf(format="A4", prefer_css_page_size=True, print_background=True)`
  von `file:///…/cv/cv.html`. Ziel: 2 Seiten.
- Der CV ist öffentlich: keine Telefonnummer, keine Wohnadresse, keine Referenzpersonen.
- About-Seite und CV inhaltlich synchron halten.
- Deutsche Fassung: `cv/cv-de.html` → `cv/Maja-Dobelsek-Lebenslauf.pdf` (verlinkt auf `de/about.html` und `de/speaking.html`).
  Bei Änderungen beide Fassungen anpassen und beide PDFs neu erzeugen.

## Zweisprachigkeit (seit 9.11.2026)

- Englisch = Hauptsprache im Root, Deutsch = gleiche Struktur unter `de/` (`de/index.html`, `de/articles/…`, gleiche Dateinamen/Slugs).
- Alle Seiten inkl. beider Artikel sind übersetzt. Nicht übersetzt: Impressum/Datenschutz (nur Deutsch, im Root, von beiden Sprachen verlinkt),
  Literaturangaben und offizielle Titel von Publikationen.
- Begriffe: „Ambient Scribe“ bleibt Englisch (z. B. „KI-Ambient-Scribe“). Schweizer Rechtschreibung (ss, «»), Sie-Form,
  gendern mit Doppelpunkt (Kliniker:innen, Patient:innen).
- Sprachumschalter EN/DE im Header verlinkt jeweils dieselbe Seite in der anderen Sprache. Im `<head>` stehen
  `hreflang`-Links (absolut auf https://majadobelsek.com/…).
- Pfade in `de/`: gemeinsame Ressourcen liegen eine Ebene höher (`../css/`, `../images/`, `../cv/`, `../impressum.html`; in `de/articles/` entsprechend `../../`).
- Script-Texte sind mitübersetzt: Kontaktformular (`de/contact.html`, Betreff „… (DE)“) und im Scribe-Artikel
  (Workflow-Labels, Buttons).
- Neuer Artikel: beide Sprachfassungen anlegen und in beiden Artikelübersichten eintragen (oder Maja fragen, ob nur eine Sprache).
- Kursseite: noch keine deutsche Fassung (Seite ist ohnehin versteckt).
