# Galerie

## Features

| Feature | Beschreibung |
|---------|-------------|
| Verzeichnisnavigation | Zweistufige Hierarchie: Jahr > Datum + Eventname |
| Thumbnail-Grid | Paginiert (60 Bilder pro Seite) mit Vollbild-Ansicht |
| S3-Speicher | RustFS (Apache 2.0) statt MinIO (AGPL) |
| Passkey-Auth | WebAuthn/FIDO2 via Einmal-Passwort-Registrierung |
| Bild-Upload | Multi-File, MIME-Validierung (JPEG, PNG, WebP), automatische Thumbnails mit EXIF-Rotation |
| Löschen | Einzelne Bilder und ganze Galerien (inkl. S3-Cleanup) |
| Battle-Einreichung | Fotos aus der Galerie direkt an FotoBattle senden |
| Darkroom-UI | Dunkles Design mit Amber-Akzenten, WCAG 2.1 AA, responsive |

## Routing

| Methode | Pfad | Beschreibung |
|---------|------|-------------|
| `GET` | `/` | Startseite (Jahresverzeichnisse) |
| `GET` | `/browse/{id}` | Verzeichnis mit Unterordnern + Bildergrid |
| `GET` | `/image/{id}` | Bild-Proxy aus S3 (mit ETag-Caching) |
| `GET/POST` | `/register` | Passkey-Registrierung via OTP |
| `POST` | `/register/challenge` | WebAuthn-Registrierungs-Challenge |
| `POST` | `/register/complete` | WebAuthn-Registrierung abschließen |
| `GET/POST` | `/login` | Passkey-Login |
| `POST` | `/login/challenge` | WebAuthn-Login-Challenge |
| `POST` | `/login/complete` | WebAuthn-Login abschließen |
| `POST` | `/logout` | Abmelden (CSRF-geschützt) |
| `GET/POST` | `/gallery/create` | Event-Galerie anlegen (Auth) |
| `GET/POST` | `/upload` | Bild-Upload (Auth) |
| `POST` | `/image/{id}/delete` | Bild löschen (Auth, CSRF) |
| `POST` | `/gallery/{id}/delete` | Galerie löschen (Auth, CSRF) |
| `POST` | `/battle/withdraw/{image_id}` | Battle-Rückzug (HMAC-Token) |

## Services

| Service | Beschreibung |
|---------|-------------|
| `GalleryService` | Verzeichnis-/Bild-CRUD, Breadcrumbs, Event-Galerie-Erstellung |
| `PasskeyService` | WebAuthn-Registrierung und -Authentifizierung, OTP-Verwaltung |
| `UploadService` | Multi-File-Upload, MIME-Validierung, Thumbnail-Workflow |
| `ThumbnailGenerator` | GD-basierte Resize (max 400×300px) mit EXIF-Rotation |
| `BucketScanner` | S3-Bucket in Datenbank katalogisieren |
| `S3ClientFactory` | AWS SDK S3Client für RustFS-Endpoint |
| `DatabaseFactory` | PDO-Verbindung aus Umgebungsvariablen |

## CLI-Werkzeuge

```bash
# Bucket scannen und Bilder in DB eintragen
docker compose exec php php bin/scan.php

# Thumbnails für alle Bilder ohne Thumbnail erzeugen
docker compose exec php php bin/thumbs.php
```

## Datenbank (Gallery-Tabellen)

| Tabelle | Beschreibung |
|---------|-------------|
| `dirs` | Verzeichnishierarchie mit `parent_id`-Selbstreferenz, Unique auf `dirname + bucket` |
| `images` | Bilddateien mit S3-Metadaten (Name, Größe, Hash), FK auf `dir_id` |
| `thumbs` | Thumbnail-Pfade, FK auf `image_id` (1:1) |
| `passkeys` | WebAuthn-Credentials (`credential_id`, `public_key`, `counter`, `transports` JSON) |
| `otp_status` | Single-Row-Tabelle: OTP verbraucht ja/nein |
