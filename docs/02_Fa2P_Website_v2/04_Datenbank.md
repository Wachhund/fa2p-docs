# Datenbank

MariaDB 10.6 mit dem Schema `freundea_website`.

## Tabellen

### Inhalte

| Tabelle | Beschreibung |
|---------|-------------|
| `hp_sites` | Seiteninhalte (Titel, Beschreibung, Meta-Tags, Content) |
| `hp_events` | Vereins-Events (Name, Beschreibung, Datum, Ort, Veranstalter) |
| `hp_events_fremd` | Externe Events |
| `hp_characters` | Charaktere (Nickname, Bild-URLs, Beschreibung, Sichtbarkeit, Sortierung) |
| `hp_header` | Header-Bilder (inkl. Sonder-Event-Varianten) |
| `hp_downloads` | Download-Dateien mit Zähler |
| `hp_newsletter` | Newsletter-Artikel |
| `hp_press` | Presseartikel |

### FAQ

| Tabelle | Beschreibung |
|---------|-------------|
| `hp_qa` | FAQ Allgemein |
| `hp_qa_rd` | FAQ RhönDance |
| `hp_qa_rent_suit` | FAQ Suitverleih |

### Fursuit-Verleih

| Tabelle | Beschreibung |
|---------|-------------|
| `rent_fursuits` | Inventar (Name, Mietpreise, Verfügbarkeit, Gewichts-/Größenlimits) |
| `rent_fursuit_type` | Suit-Typen |
| `rent_fursuit_parts` | Suit-Bestandteile |
| `rent_fursuit_x_parts` | Suit-zu-Teile-Zuordnung |

### Fotos

| Tabelle | Beschreibung |
|---------|-------------|
| `event_link_archive` | Foto-Archiv-Metadaten |
| `event_links` | Foto-Links pro Event |

### Benutzer & Sicherheit

| Tabelle | Beschreibung |
|---------|-------------|
| `users` | Benutzersystem (Telegram-Auth mit Metadaten) |
| `hp_userdata` | Altes Benutzersystem (Nickname, Telegram-Handle) |
| `security_keys` | API-/Sicherheitsschlüssel |

### Zahlungen

| Tabelle | Beschreibung |
|---------|-------------|
| `Zahlungsbestaetigung` | Zahlungsbestätigungen (Badge-Nummern, Status) |
| `Zahlungserinnerungen` | Zahlungserinnerungen/Rechnungen |

## Konfiguration

Die Datenbankverbindung wird in `lib/config.php` (INI-Format) definiert:

```ini
db[driver] = 'mysql'
db[user] = ''
db[password] = ''
db[server] = 'localhost'
db[port] = '3306'
db[database] = 'freundea_website'
```
