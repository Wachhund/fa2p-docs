# JSON-API

Die Website bietet eine öffentliche JSON-API mit 12 Read-Endpunkten und 2 Write-Endpunkten.

Antwortformat: JSON mit UTF-8-Kodierung. CORS ist für die konfigurierte Domain aktiviert.

## Event-Endpunkte

| Endpunkt | Beschreibung |
|----------|-------------|
| `GET /api/events` | Kommende sichtbare Events (inkl. `photo_count` und Pretix-Ticketdaten) |
| `GET /api/events?type=archive` | Vergangene Events |
| `GET /api/events?type=all` | Alle Events |
| `GET /api/events?type=external` | Kommende externe Events |

## Charaktere & Fursuits

| Endpunkt | Beschreibung |
|----------|-------------|
| `GET /api/characters` | Vereinscharaktere (Standard: Deutsch) |
| `GET /api/characters?lang=en` | Vereinscharaktere auf Englisch |
| `GET /api/fursuits` | Verfügbare Fursuits |
| `GET /api/fursuit-types` | Fursuit-Typen |
| `GET /api/fursuit-parts` | Fursuit-Bestandteile |

## Vereinsinformationen

| Endpunkt | Beschreibung |
|----------|-------------|
| `GET /api/faq?type=allgemein` | FAQ-Einträge (Typen: `allgemein`, `rhoen_dance`, `fursuits`) |
| `GET /api/faq?type=allgemein&lang=en` | FAQ auf Englisch |
| `GET /api/downloads` | Öffentliche Downloads (Satzung etc.) |
| `GET /api/press` | Presseartikel |
| `GET /api/rhoen-dance` | RhönDance-Veranstaltungsdaten |
| `GET /api/photo-links` | Event-Foto-Links |
| `GET /api/social-links` | Social-Media-Links |
| `GET /api/translations?lang=de` | Übersetzungs-Strings (DE/EN) |

## Write-Endpunkte

| Endpunkt | Beschreibung |
|----------|-------------|
| `POST /api/contact` | Kontaktanfrage senden (Rate-Limited: 5/15 Min, Honeypot-geschützt) |
| `POST /api/beitritt-pdf` | Beitrittsantrag als PDF generieren (JSON-Body → PDF-Download, Rate-Limited: 5/15 Min) |

## Weitere Endpunkte

| Endpunkt | Beschreibung |
|----------|-------------|
| `GET /events-ical` | iCal-Export aller Events (`.ics`-Datei) |
| `GET /sitemap.xml` | XML-Sitemap |
