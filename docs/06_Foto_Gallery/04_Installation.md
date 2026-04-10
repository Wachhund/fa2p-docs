# Installation

## Schnellstart

```bash
git clone https://github.com/Wachhund/S3-image-gallery.git
cd S3-image-gallery

cp .env.example .env
# Optional: .env anpassen (Ports, Credentials, OTP)

docker compose up --build -d
```

### Testdaten laden

```bash
# Bucket scannen und Bilder katalogisieren
docker compose exec php php bin/scan.php

# Thumbnails erzeugen
docker compose exec php php bin/thumbs.php
```

### Passkey einrichten

1. In `.env` ein Einmal-Passwort setzen: `S3G_REGISTRATION_OTP=mein-geheimes-passwort`
2. Container neu erstellen: `docker compose up -d --force-recreate php`
3. Unter `/register` den Passkey mit dem OTP anlegen
4. Unter `/login` mit dem Passkey anmelden

## Docker-Services

| Service | Basis | Port | Beschreibung |
|---------|-------|------|-------------|
| **nginx** | Alpine | 8080 | Reverse Proxy, Security Headers, Gzip |
| **php** | PHP 8.4 FPM | — | Anwendung (GD, EXIF, PDO) |
| **db** | MariaDB 11 | 3306 | Datenbank (persistentes Volume) |
| **storage** | RustFS | 9001 (Console) | S3-kompatibler Objektspeicher |
| **storage-init** | AWS CLI | — | Bucket-Erstellung (einmalig) |
| **battle-php** | PHP 8.4 FPM | — | FotoBattle-Anwendung |
| **battle-nginx** | Alpine | 8081 | FotoBattle-Frontend |

## FotoBattle aktivieren

FotoBattle muss als Geschwister-Verzeichnis neben der Gallery liegen:

```text
├── S3-image-gallery/
└── photo_battles/
```

```bash
# FotoBattle-Container starten
docker compose up -d --build battle-php battle-nginx
```

Erster Admin-Account:
1. Unter `http://localhost:8081/register` registrieren
2. In der Datenbank: `UPDATE users SET role='admin' WHERE email='...'`

## Umgebungsvariablen

### Gallery

| Variable | Standard | Beschreibung |
|----------|---------|-------------|
| `PHP_VERSION` | 8.5 | Docker-Image-Tag |
| `DB_HOST` | db | MariaDB-Hostname |
| `DB_NAME` | s3gallery | Datenbankname |
| `DB_USER` | s3gallery | DB-Benutzer |
| `DB_PASSWORD` | changeme | DB-Passwort |
| `S3_ENDPOINT` | http://storage:9000 | RustFS-API-Endpoint |
| `S3_ACCESS_KEY` | rustfsadmin | RustFS-Zugangsdaten |
| `S3_SECRET_KEY` | rustfsadmin | RustFS-Zugangsdaten |
| `S3_BUCKET` | gallery | S3-Bucket-Name |
| `NGINX_PORT` | 8080 | Host-Port für Gallery |
| `RUSTFS_CONSOLE_PORT` | 9001 | Host-Port für RustFS-Console |
| `APP_DEBUG` | false | Fehlerdetails anzeigen |
| `APP_SECURE_COOKIES` | true | HTTPS-only Session-Cookies |
| `S3G_REGISTRATION_OTP` | *(leer)* | Einmal-Passwort für Passkey-Registrierung |
| `S3G_RP_ID` | localhost | WebAuthn Relying Party ID |

### FotoBattle

| Variable | Standard | Beschreibung |
|----------|---------|-------------|
| `BATTLE_PORT` | 8081 | Host-Port für FotoBattle |
| `BATTLE_URL` | http://localhost:8081 | FotoBattle-Basis-URL |
| `BATTLE_SUBMIT_SECRET` | changeme | Shared HMAC-Secret (min. 32 Zeichen) |

DB- und S3-Variablen werden von der Gallery geteilt.

## Quality Gates

```bash
# Tests
docker compose exec php composer test

# Statische Analyse (PHPStan Level 6)
docker compose exec php composer analyse

# Code Style
docker compose exec php composer cs-check
```

## Constraints

- **Demo-Projekt** — Nicht für Produktion vorgesehen
- **Single-Node** — Docker Compose, kein Kubernetes
- **Kein CDN** — Bilder über PHP-Proxy direkt aus RustFS
- **Kein WebSocket** — Kein Live-Battle-Feed
- **Synchrone Projektionen** — Kein Message Broker im MVP
