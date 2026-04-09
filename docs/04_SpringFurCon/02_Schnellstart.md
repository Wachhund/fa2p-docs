# Schnellstart

## Ersteinrichtung

```bash
# 1. Abhängigkeiten installieren, .env anlegen, Migrations ausführen, Assets bauen
composer setup

# 2. Storage-Symlink erstellen (einmalig)
php artisan storage:link

# 3. Entwicklungsserver starten (Server + Queue + Log-Tail + Vite parallel)
composer dev
```

Der Admin-Bereich ist nach dem Start unter `http://localhost:8000/admin` erreichbar.

## Befehle

| Befehl | Zweck |
|--------|-------|
| `composer dev` | Entwicklungsserver starten |
| `composer test` | Config-Cache leeren und Tests ausführen |
| `npm run build` | Frontend-Assets für Produktion bauen |
| `./vendor/bin/pint` | Code-Style prüfen/korrigieren |
| `php artisan migrate` | Datenbankmigrationen ausführen |
| `php artisan migrate:fresh` | Datenbank zurücksetzen und neu migrieren |

## Produktion

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
npm run build
```

## Git-Workflow

- `main` — Produktionszweig
- `dev` — Arbeitszweig
- Feature-Branches: `feature/PROJ-<id>-<slug>`
- Commit-Präfix: `PROJ-<id>: beschreibung`
