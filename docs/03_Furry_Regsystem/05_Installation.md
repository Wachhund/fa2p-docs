# Installation

## Docker (Schnellstart)

### Voraussetzungen

- Docker Desktop mit laufendem Docker-Daemon

### Secrets vorbereiten

`APP_KEY` und `JWT_SECRET` müssen als Umgebungsvariablen gesetzt sein, bevor der Container startet.

```bash
# APP_KEY lokal generieren (einmalig)
php artisan key:generate --show

# JWT_SECRET generieren (einmalig)
openssl rand -base64 32
```

Beide Werte in eine `.env`-Datei oder als Shell-Exports setzen:

```bash
export APP_KEY="base64:DEIN_GENERIERTER_KEY=="
export JWT_SECRET="DEIN_GENERIERTES_SECRET"
```

### Starten

```bash
docker compose up --build -d
```

| Adresse | Zweck |
|---------|-------|
| `http://localhost:18080/` | Anwendung |
| `http://localhost:18080/up` | Health-Check |

Beim ersten Start legt der Entrypoint automatisch die SQLite-Datenbank an und führt alle Migrationen aus.

### Stoppen

```bash
docker compose down
```

Daten bleiben im Docker-Volume `regsys_storage` erhalten.

---

## Lokale Entwicklung

### Voraussetzungen

- PHP 8.4+ mit den Extensions `pdo_sqlite`, `zip`
- Composer 2
- Node.js 24+ mit npm

### Ersteinrichtung

```bash
cd RegSys
composer setup
```

`composer setup` führt aus: `composer install`, `.env` kopieren, `APP_KEY` generieren, Datenbankmigrationen, `npm install` und Frontend-Build.

### Entwicklungsserver

```bash
composer dev
```

Startet parallel: Laravel-Backend (Port 8000), Queue-Worker, Pail-Log-Viewer und Vite HMR.

Alternativ einzeln:

```bash
php artisan serve          # Backend auf Port 8000
npm run dev                # Vite mit Hot Module Replacement
php artisan queue:listen   # Queue-Worker
```

### Einzelnen Test ausführen

```bash
php artisan test --filter=EventTest
```
