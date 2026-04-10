# Sicherheit

## Passkey/WebAuthn (Gallery)

- Einmal-Passwort-Registrierung (`S3G_REGISTRATION_OTP` Umgebungsvariable)
- Attestation-Verifizierung via lbuchs/webauthn
- Counter-Validierung bei Authentifizierung (Schutz gegen geklonte Authenticators)
- Transports als JSON gespeichert

## Session-Management

- `secure` — HTTPS-only in Produktion (konfigurierbar via `APP_SECURE_COOKIES`)
- `httponly` — Kein JavaScript-Zugriff
- `samesite=Lax` — CSRF-Mitigation
- `session_regenerate_id(true)` bei Login/Logout

## CSRF-Schutz

- Tokens per Session: `bin2hex(random_bytes(32))`
- Timing-sichere Validierung via `hash_equals()`
- Pflicht für: Login, Logout, Upload, Löschen, Galerie-Erstellung

## HTTP-Headers (Nginx)

```text
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
Content-Security-Policy: default-src 'self'; script-src/style-src 'unsafe-inline'
```

## Gallery-FotoBattle-Integration

- HMAC-SHA256-Tokens für die Submit/Withdraw-API
- Shared Secret (`BATTLE_SUBMIT_SECRET`) zwischen beiden Containern
- Timing-sichere Validierung via `hash_equals()`

## FotoBattle-spezifisch

- Passwort-Hashing mit bcrypt
- Rate Limiting: 5 fehlgeschlagene Logins → 15 Min Sperre per IP-Hash
- Optimistic Locking im Event Store (Versionskonflikte)

## Eingabevalidierung

- MIME-Type-Prüfung (nur image/jpeg, image/png, image/webp)
- Dateiendungs-Whitelist
- Date-Format-Validierung (Event-Galerien)
- Directory-Traversal-Schutz (`basename`/Path-Normalisierung)

## S3-Sicherheit

- Credentials ausschließlich über Umgebungsvariablen
- Bild-Proxy: Client greift nie direkt auf RustFS zu
- ETag-Headers für Client-seitiges Caching
