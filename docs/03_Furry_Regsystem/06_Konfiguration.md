# Konfiguration

## Pflicht-Umgebungsvariablen

| Variable | Beschreibung | Beispiel |
|----------|--------------|---------|
| `APP_KEY` | Laravel-Anwendungsschlüssel (base64-kodiert) | `base64:abc...==` |
| `JWT_SECRET` | Signing-Key für JWT-Access-Tokens (HS256) | `openssl rand -base64 32` |

Der Container bricht beim Start ab, wenn eine dieser Variablen fehlt.

## Optionale Umgebungsvariablen

| Variable | Standard | Beschreibung |
|----------|---------|--------------|
| `APP_ENV` | `production` | Umgebungsmodus (`local`, `production`) |
| `APP_DEBUG` | `false` | Debug-Modus (niemals `true` in Produktion) |
| `APP_URL` | `http://localhost:8080` | Öffentliche URL der Anwendung |
| `APP_PORT` | `8000` | Interner Port des PHP-Servers |
| `DB_CONNECTION` | `sqlite` | Datenbanktreiber (`sqlite`, `mysql`) |
| `DB_DATABASE` | `/app/storage/database.sqlite` | Datenbankpfad (SQLite) oder Datenbankname |
| `CACHE_STORE` | `file` | Cache-Treiber (`file`, `redis`) |
| `SESSION_DRIVER` | `file` | Session-Treiber (`file`, `redis`, `cookie`) |
| `QUEUE_CONNECTION` | `sync` | Queue-Treiber (`sync`, `redis`) |
| `PAYMENTS_DRIVER` | — | Aktiver Zahlungsanbieter (`sumup`) |
| `SUMUP_BASE_URL` | — | SumUp-API-Basis-URL |
| `SUMUP_TIMEOUT_SECONDS` | — | HTTP-Timeout für SumUp-Requests |
| `SUMUP_WEBHOOK_TOLERANCE_SECONDS` | — | Zeitfenster für Webhook-Signatur-Prüfung |
| `SUMUP_DEFAULT_CURRENCY` | — | Standardwährung (z.B. `EUR`) |
| `SUMUP_SIMULATE` | — | Zahlungssimulation aktivieren (`true`) |

> **Hinweis:** Für den Produktionsbetrieb sollten `CACHE_STORE`, `SESSION_DRIVER` und `QUEUE_CONNECTION` auf `redis` gesetzt werden, um horizontale Skalierbarkeit und persistente Queue-Verarbeitung zu gewährleisten.
