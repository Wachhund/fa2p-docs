# CI/CD & Infrastruktur

## GitHub Actions Workflows

### 1. PHP-Analyse (`php.yml`)

Läuft bei Push/PR auf `master`:

- PHP 8.3 Syntax-Check aller `.php`-Dateien
- **PHPStan Level 6** Analyse (512 MB Memory-Limit)
- **Composer Audit** (AC-3 Vulnerability Scanning)
- **Gitleaks** Secret Scanning

### 2. Tests (`tests.yml`)

Läuft bei Push auf `master`/`dev` und PRs auf `master`:

- MySQL 8.0 Service-Container mit Health Checks
- Test-Config wird inline generiert
- Schema-Import aus `admin/create_sql.sql`
- Unit- und Integration-Tests mit PHPUnit (5 Min Timeout)

### 3. Docker Smoke Tests (`docker-smoke.yml`)

Umfassende Integrationstests:

- Docker-Image bauen mit Cache
- **HTTP-Smoke-Tests**: 200er-Status auf 12+ Seiten (/, /downloads, /events, /kontakt, /mitglied, ...)
- **Content-Checks**: OG-Tags, Preload-Hints, CSRF-Meta-Tags auf Formularen
- **Social-Redirect-Validierung**: 8 Plattformen → 302 + Location-Header
- **API-Struktur-Checks**: 13 JSON-Endpunkte
- **Admin-Asset-Prüfung** und 404-Handling

## Apache .htaccess Security Headers

```text
Content-Security-Policy: default-src 'self'; script-src 'self' (Admin)
Strict-Transport-Security: max-age=31536000
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
```

### Session-Hardening

- HttpOnly, SameSite=Lax, Secure
- `gc_maxlifetime`: 3600s

### Caching

- Statische Assets: 30 Tage (Bilder, Fonts, WOFF2), 7 Tage (CSS/JS)
- GZIP Deflate aktiviert

## Docker-Setup

Drei Container:

| Container | Basis | Port |
|-----------|-------|------|
| **web** | PHP 8.3 + Apache | 15080 |
| **mcp** | PHP 8.3 Built-in Server | 8888 |
| **db** | MySQL 8.0 | 13306 |

## Datenbank-Schema

13 Tabellen (InnoDB, utf8mb4):

| Tabelle | Beschreibung |
|---------|-------------|
| `admin` | Admin-Benutzer (inkl. Brute-Force-Felder: `failed_attempts`, `locked_until`) |
| `permissions` + `admin_permissions` | 11 granulare Berechtigungen |
| `events` | Events (inkl. Pretix: `pretix_event_slug`, `pretix_quota_id`) |
| `charaktere` | Bilingual (title_de/en, text_de/en), positionsbasierte Sortierung |
| `faq` | Konsolidiert mit Type-Spalte (`allgemein`, `rhoen_dance`, `fursuits`) |
| `fursuits` + `fursuit_types` + `fursuit_parts` + `fursuit_part_map` | Verleihsystem mit Preisstufen und Maßbeschränkungen |
| `downloads` | Download-Zähler + tägliche Statistiken |
| `event_photo_links` | Fotografen-Attribution + Quellen-Tracking |
| `social_links` | 8 Plattformen |
| `rhoen_dance` | Plätze/Kapazität |
| `mail_config` | SMTP-Konfiguration |
| `contact_form` + `contact_request_log` | Kontaktanfragen mit IP-Hashing für Spam-Tracking |

### Pretix-Integration

- `PretixAPI`-Klasse mit 5-Minuten-Cache (SHA256-gehashte JSON-Dateien in `cache/pretix/`)
- Endpoints: Quota-Verfügbarkeit und Event-Liste
- API-Server: `https://pretix.eu/api/v1`
- Token-basierte Authentifizierung
