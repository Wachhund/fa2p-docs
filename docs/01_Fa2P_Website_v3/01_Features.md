# Website-Funktionen

## Startseite

- Zufälliges Header-Bild aus einer Auswahl von 20 Motiven (klickbar zum Vergrößern)
- Vorstellung des Vereins mit Einladung zur Kontaktaufnahme
- Inspirierendes Zitat
- Übersicht aller Partnervereine und -organisationen mit Verlinkung
- Sidebar mit den nächsten drei anstehenden Events und Social-Media-Links

## Events

- Auflistung aller kommenden Veranstaltungen mit Datum, Ort und Beschreibung
- **Filterfunktion**: Vereinsevents, externe Events oder alle gleichzeitig
- **Pretix-Integration**: Bei Events mit Ticketverkauf wird ein Fortschrittsbalken (belegte/verfügbare Plätze) und ein direkter Link zum Ticketshop angezeigt
- Verlinkung zu externen Event-Seiten
- Anzeige von Veranstalter und Anmeldehinweisen
- **Google-Maps-Karte** mit allen bisherigen Veranstaltungsorten
- **iCal-Export**: Alle kommenden Events als `.ics`-Datei herunterladen

## Event-Archiv

- Chronologische Auflistung vergangener Veranstaltungen
- Gleiche Darstellung wie die aktuelle Events-Seite, aber rückwärts sortiert

## Event-Fotos

- Sammlung von Foto-Links zu vergangenen Events, gruppiert nach Veranstaltung
- Angabe des Fotografen und der Foto-Plattform (z.B. Google Photos, Flickr)
- Optional filterbar nach einzelnem Event

## Charaktere

- Bildergalerie aller Vereinscharaktere mit Name und Beschreibung
- Aufklappbare Detailansicht pro Charakter
- Die ersten sechs Charaktere sind fixiert; die restlichen werden bei jedem Seitenaufruf zufällig angeordnet
- Bilder können per Klick vergrößert betrachtet werden (Viewer.js)

## Fursuit-Verleih

- Übersicht aller aktuell verfügbaren Fursuits zum Ausleihen
- Pro Fursuit: Vorschaubild (klickbar zum Vergrößern), aufklappbare Details mit:
  - Name, Fursuittyp, Bestandteile
  - Preise für Kurz-, Mittel- und Langzeitmiete
  - Maximales Gewicht, Größe und Kopfumfang
- Direktlinks zu den Vertrags-AGB (PDF) und dem Anfrage-Formular (Google Forms)
- Link zur FAQ-Seite für den Suitverleih

## RhönDance

- Informationsseite über die vereinseigene Tanzveranstaltung "RhönDance"
- Bildergalerie der vergangenen Veranstaltungen
- Anzeige der verfügbaren Plätze und Kapazität

## Kontakt

- Kontaktmöglichkeiten: Telefonnummer und Telegram-Link
- Links zu allen Social-Media-Kanälen (Discord, Facebook, Instagram, Telegram, X, YouTube, Twitch, Bluesky)
- **Kontaktformular** mit Feldern für Anrede, Vor-/Nachname, E-Mail, Telefon und Nachricht
- Eingegangene Anfragen werden per E-Mail und Telegram an den Vorstand weitergeleitet

## Vereinsinformationen

- **Mitglied werden** (`/mitglied`): Vorstellung des Vereins, Vereinszweck und Informationen zur Mitgliedschaft
- **Online-Beitrittsantrag** (`/beitritt`): Formular zum digitalen Ausfüllen eines Beitrittsantrags; generiert automatisch ein ausgefülltes PDF zum Ausdrucken und Unterschreiben (Fördermitgliedschaft mit optionaler SEPA-Lastschrift)

## Downloads

- Downloadbereich für Vereinsdokumente (z.B. Satzung, Beitrittsformular)
- Dateien werden als PDF bereitgestellt und können direkt heruntergeladen werden
- Dynamisch aus der Datenbank geladen; Reihenfolge und Sichtbarkeit über den Admin steuerbar

## FAQ

Drei separate FAQ-Bereiche mit auf-/zuklappbaren Fragen und Antworten:

- **FAQ Allgemein** (`/faq-allgemein`): Fragen zum Verein und zur Furry-Community
- **FAQ RhönDance** (`/faq-rd`): Fragen zur RhönDance-Veranstaltung
- **FAQ Fursuitverleih** (`/faq-rs`): Fragen zum Ausleihprozess

## Presse

- Auflistung von Presseartikeln und Medienberichten über den Verein
- Verlinkung zu externen Artikeln

## Weitere Funktionen

- **Zweisprachigkeit**: Alle Seiten in Deutsch und Englisch verfügbar; Sprachumschaltung in der Sidebar
- **Social-Media-Redirects**: Kurz-URLs wie `/discord`, `/instagram`, `/youtube` leiten auf die jeweiligen Vereinsprofile weiter
- **Responsive Design**: Optimiert für Desktop, Tablet und Smartphone mit Bootstrap 5
- **SEO**: Sitemap (`/sitemap.xml`), Open-Graph-Tags, Structured Data (JSON-LD), Hreflang-Tags, kanonische URLs
- **404-Fehlerseite**: Eigene Fehlerseite bei nicht gefundenen Inhalten
- **Datenschutz & Impressum**: Rechtlich erforderliche Seiten
