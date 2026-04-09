# Architektur

## CMS-Seitenaufbau

Content-Redakteure erstellen Seiten im Filament Admin (`/admin`) über den Fabricator Page-Builder. Seiten werden in der Tabelle `pages` mit einer `blocks`-JSON-Spalte gespeichert. Fabricator routet Seiten automatisch anhand ihres Slugs — z.B. Slug `de/faq` unter `/de/faq`.

Seiten unterstützen hierarchische Eltern-Kind-Beziehungen über `parent_id` (Self-referential FK mit Cascade Delete).

### Vorhandene Page-Blocks

| Block | Klasse | Felder |
|-------|--------|--------|
| `rich-text` | `RichTextBlock` | `heading`, `content` |
| `image` | `ImageBlock` | `image`, `alt`, `caption`, `align` |

- **RichTextBlock**: Heading (optional), Content (RichEditor mit Dateianhängen). Ausgabe wird durch einen **HTML-Sanitizer** (Symfony HtmlSanitizer) mit strikter Allowlist gefiltert — Scripts, Event-Handler und `data:`-URIs werden entfernt, Links erhalten automatisch `rel="noopener noreferrer"`.
- **ImageBlock**: Bild (JPEG/PNG/GIF/WEBP, max 10 MB), Alt-Text, Caption (optional), Alignment (left/center/right) mit Validierung gegen XSS.

### Neuen Page-Block hinzufügen

1. Klasse in `app/Filament/Fabricator/PageBlocks/` anlegen (extends `PageBlock`)
2. Formularfelder in `getBlockSchema()` definieren
3. Optional `mutateData()` für Nachverarbeitung überschreiben
4. Blade-View in `resources/views/components/filament-fabricator/page-blocks/` anlegen
5. Fabricator registriert den Block automatisch

## Mehrsprachigkeit

- **URL-basiertes Routing**: Seiten mit `/de` oder `/en` Präfix
- **Client-seitige Locale-Erkennung** im Blade-Layout: Extrahiert Sprache aus URL-Segment
- **Dynamischer Sprachwechsel-Button** mit Label-Dictionaries (18+ Schlüssel pro Sprache)
- **Seeder**: Erstellt parallele de/en-Seitenbäume mit lokalisiertem Content (~15 Seiten pro Sprache)
- **Hinweis**: Keine Laravel-Übersetzungsdateien — Labels sind aktuell im Blade-Template hardcoded

## Registrierungs-Counter

API-Endpunkt unter `/api/regcount` (Rate-Limited: 30 Req/Min), der die Anmeldezahl von der externen SpringFurCon-Registrierungsseite abruft:

- **Primär**: Regex-Pattern-Matching auf "Reg-Counter"
- **Fallback**: DOM-basierte XPath-Suche
- **Caching**: 5 Minuten mit Distributed Lock (Thundering-Herd-Schutz)
- **Graceful Degradation**: Bei Fehler wird der letzte gecachte Wert zurückgegeben
- Seiten binden den Counter über `<span data-regcount>` Platzhalter ein

## Crawl-Route

`GET /crawl/{path?}` serviert **statische HTML-Dateien aus einem `crawl/`-Verzeichnis**:

- Path-Traversal-Schutz mit `realpath()` und Prefix-Validierung
- Verzeichnisse werden automatisch auf `index.html` aufgelöst
- `Cache-Control: no-cache, no-store, must-revalidate`
- Zweck: Hosting von extern gecrawlten/gecachten Inhalten (Bilder, statische Seiten eines externen CMS)

## Custom Artisan Commands

- **`admin:create`**: Erstellt oder befördert Benutzer zum Admin
  - E-Mail-basierte Suche
  - Interaktiver Passwort-Prompt mit Auto-Generierung (16 Zeichen)
  - Validierung: E-Mail-Format, mind. 8 Zeichen Passwort
  - Verwendet `is_admin` Boolean-Flag

## Routing

- `GET /` — Redirect auf `/de`
- `GET /crawl/{path?}` — Statische HTML aus `crawl/`
- `GET /api/regcount` — Registrierungs-Counter API
- Alle anderen Seiten via Fabricator Slug-Routing
- Admin-Panel unter `/admin` (Filament, Session-Auth)

## Verzeichnisstruktur

```text
app/
├── Console/Commands/AdminCreateCommand.php     Admin-Erstellung
├── Filament/Fabricator/PageBlocks/             Block-Definitionen mit Sanitizer
├── Filament/Fabricator/Layouts/                Layout-Definitionen
├── Http/Controllers/RegcountController.php     Registrierungs-Counter API
└── Providers/Filament/AdminPanelProvider.php   Filament-Konfiguration (Emerald Theme)
resources/views/
├── components/filament-fabricator/             Blade-Views der Blocks + Layouts
└── *.blade.php                                Statische Templates
database/
├── migrations/                                Users, Pages, Cache, Jobs
└── seeders/FabricatorPageSeeder.php           Bilingualer Content (~1600 Zeilen)
```

## Uploads

Hochgeladene Dateien liegen unter `storage/app/public/`:

- `page-images/` — Bilder aus ImageBlock
- `page-attachments/` — Dateianhänge aus RichTextBlock

Der Storage-Symlink (`php artisan storage:link`) muss vor dem ersten Upload erstellt sein.

## Tests

11 Testdateien:

- **RichTextBlockTest** (13 Tests): HTML-Sanitierung, XSS-Prävention, erlaubte Tags
- **ImageBlockTest** (6 Tests): Alignment-Validierung, Datenmutation
- **ImageBlockRenderTest** (6 Tests): Page-Rendering mit Bildern, Captions, Alignment-Klassen
- **RegcountExtractionTest** (8 Tests): Regex- und DOM-Extraktion, Edge Cases

Test-Konfiguration: SQLite `:memory:`, Array-Cache, Sync-Queue.
