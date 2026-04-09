# Features

## Öffentliche Seiten

| Seite | Beschreibung |
|-------|-------------|
| Startseite | Homepage mit zufälligem Header-Bild (inkl. Aprilscherz-Variante) |
| Events | Aktuelle Veranstaltungen mit Statusfilter (aktuell, geplant, extern) |
| Event-Archiv | Vergangene Veranstaltungen |
| Charaktere | Bildergalerie aller Vereinscharaktere mit Lightbox-Zoom |
| Fursuit-Verleih | Mietkatalog mit Preisstufen (Kurz-/Mittel-/Langzeitmiete), Maßen und Verfügbarkeit |
| RhönDance | Event-Seite mit Registrierungsstatus, Team-Info und Kapazitätsanzeige |
| FAQ | Drei separate FAQ-Bereiche (Allgemein, RhönDance, Suitverleih) |
| Newsletter | Newsletter-Archiv |
| Presse | Medienberichte und Presseartikel |
| Downloads | Vereinsdokumente zum Herunterladen (mit Download-Zähler) |
| Mitglieder | Vereinsinformationen und Mitgliedschaft |
| Kontakt | Kontaktformular mit Google reCAPTCHA v3 |
| Datenschutz | Datenschutzerklärung (DSGVO) |
| Impressum | Rechtliche Pflichtangaben |

## Besonderheiten

- **Sprache**: Nur Deutsch (keine Mehrsprachigkeit)
- **Responsive Design**: Bootstrap 4 Grid
- **Template-System**: Eigene String-Replacement-Engine (`#variable#` in HTML-Templates)
- **Dateibasiertes Routing**: Klassisch über `.php`-Dateien + Apache `.htaccess` Rewrites
- **Kontaktformular-Logging**: Eingehende Anfragen werden in CSV-Dateien protokolliert (`logs/log_[Monat]_[JJ].csv`)
- **Aprilscherz-Header**: Spezielle Header-Bilder für den 1. April
- **Bluesky-Integration**: Invite-Code-System für Bluesky-Einladungen (`bsky/`)
