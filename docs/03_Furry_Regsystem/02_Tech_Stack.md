# Tech-Stack

| Schicht | Technologie | Version |
|---------|-------------|---------|
| Backend-Framework | Laravel | 12.x |
| Sprache | PHP | 8.4 |
| Auth | firebase/php-jwt (HS256) | ^7.0 |
| Frontend-Framework | Vue 3 (Composition API) | ^3.5 |
| State Management | Pinia | ^2.3 |
| Routing (SPA) | Vue Router | ^4.5 |
| CSS | TailwindCSS | 4.x (Vite-Plugin) |
| Build-Tool | Vite | 7.x |
| Produktionsdatenbank | MariaDB | — |
| Entwicklungs-/Offline-DB | SQLite | — |
| Queue / Cache | Redis (Produktion), Sync/File (lokal) | — |
| Deployment | Docker (Multi-Stage Build) | — |
| Schriftarten | @fontsource/space-grotesk, @fontsource/ibm-plex-mono | — |

## Quality Gates

Alle drei Gates müssen vor jedem Commit grün sein.

### Tests (Pest + PHPUnit)

```bash
composer test
# entspricht: php artisan config:clear && php artisan test
```

Testtreiber ist SQLite `:memory:`. Testdateien liegen unter `tests/Feature/` und `tests/Unit/`.

### Statische Analyse (PHPStan Level 8)

```bash
vendor/bin/phpstan analyse --memory-limit=1G
```

Level 8 erfordert vollständige Typ-Annotationen — alle Eloquent-Relationen sind mit generischen PHPDoc-Typen annotiert.

### Code Style (Laravel Pint)

```bash
vendor/bin/pint --test    # nur prüfen
vendor/bin/pint           # automatisch fixen
```
