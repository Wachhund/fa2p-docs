# API

Alle Endpunkte liegen unter `/api/` und liefern JSON.

| Endpunkt | Beschreibung |
|----------|-------------|
| `GET /api/website.php?data=[filename]` | Seiteninhalt und zufälliges Header-Bild |
| `GET /api/events.php?data=[status]` | Events nach Status (`aktuell`, `geplant`, `entwurf`, `archiv`, `fremd`) |
| `GET /api/characters.php` | Alle Charaktere (sortiert + zufällig gemischt) |
| `GET /api/photo_links.php?data=[type]` | Foto-Links (`events_list`, `links_list`, `events_with_photos_list`) |
| `POST /api/mau.php` | Telegram-Admin-Status prüfen |
