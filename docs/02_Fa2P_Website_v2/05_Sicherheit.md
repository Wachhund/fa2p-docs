# Sicherheit & Externe Dienste

## Sicherheitsmaßnahmen

| Maßnahme | Beschreibung |
|----------|-------------|
| **Telegram OAuth** | Admin-Login über Telegram-Bot mit HMAC-SHA256 Hash-Verifizierung |
| **reCAPTCHA v3** | Kontaktformular-Schutz gegen Bots |
| **SQL-Injection-Schutz** | PDO Prepared Statements |
| **XSS-Schutz** | `htmlspecialchars()` für Formulareingaben |
| **HSTS** | Strict-Transport-Security Header |
| **Referrer-Policy** | `no-referrer` / `same-origin` |
| **Externe Links** | `rel="noopener"` auf allen externen Links |
| **Sensitive Dateien** | Konfigurationsdateien in `.gitignore` ausgeschlossen |

## Externe Dienste

| Dienst | Zweck |
|--------|-------|
| **Telegram Bot** (@Fa2P_WebBOT) | Admin-Authentifizierung + Event-Benachrichtigungen |
| **Google reCAPTCHA v3** | Kontaktformular-Spam-Schutz |
| **Cookiebot** | Cookie-Consent-Management (DSGVO) |
| **Pretix** | Event-Registrierung (URL-Integration) |
| **eTracker** | Website-Analytics |
| **Google Analytics** | Zusätzliches Tracking |

## Deployment

- **Hosting**: Apache auf Shared Hosting (cPanel)
- **SSL/TLS**: HSTS erzwungen
- **Kein Docker**: Direkte Datei-Uploads auf den Live-Server
- **URL-Rewrites**: `.htaccess` entfernt `www`-Subdomain und erzwingt HTTPS
