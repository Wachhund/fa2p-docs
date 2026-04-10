# S3 Image Gallery

Moderne Bildergalerie mit S3-kompatibler Objektspeicherung, Passkey-Authentifizierung und Docker-basiertem Setup. Erweitert um **FotoBattle** — eine kompetitive Foto-Battle-Plattform mit Elo-Rating und Event Sourcing.

Gedacht als Demo-Projekt und Referenzimplementierung.

| Eigenschaft | Wert |
|-------------|------|
| Repository | [Wachhund/S3-image-gallery](https://github.com/Wachhund/S3-image-gallery) |
| Zeitraum | 03/2026 – aktiv |
| Commits | 29 (Gallery) + 5 (FotoBattle) |
| Stack | PHP 8.4, Slim 4, MariaDB 11, RustFS, Docker |
| Status | Demo / Referenzimplementierung |

---

## Hintergrund

Ursprünglich für Event-Fotos von Freunde auf 2 Pfoten e. V. konzipiert. Inzwischen als eigenständiges Demo-Projekt weiterentwickelt — der FotoBattle-Bereich könnte in Zukunft für den Verein genutzt werden.

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Sprache | PHP 8.4 |
| Framework | Slim 4 (Routing, Middleware, PSR-7) |
| Datenbank | MariaDB 11 (InnoDB, utf8mb4) |
| Objektspeicher | RustFS (S3-kompatibel, Apache 2.0) |
| Auth (Gallery) | WebAuthn/FIDO2 Passkeys (lbuchs/webauthn) |
| Auth (FotoBattle) | Session-basiert (E-Mail/Passwort, bcrypt) |
| Templates | slim/php-view (Server-Side Rendering) |
| Webserver | Nginx Alpine (Reverse Proxy) |
| Tests | PHPUnit 11, PHPStan Level 6, PHP-CS-Fixer 3 |
| Deployment | Docker Compose (5–7 Services) |

## Phasen

### Phase 1 — Galerie-Infrastruktur (abgeschlossen)

12 Features (S3G-1 bis S3G-12), alle deployed:

- Docker-Infrastruktur und UI Design System
- S3-Bildverwaltung (Scan, Katalog, Thumbnails)
- Web-Galerie mit Verzeichnisnavigation
- Passkey-Registrierung via Einmal-Passwort
- Event-Galerie-Verwaltung (Jahr/Datum-Hierarchie)
- Bild-Upload und Löschfunktionen
- Battle-Einreichung aus der Galerie
- Test-Suite und QA-Audit (Ergebnis: 7.3/10)

### Phase 2 — FotoBattle (vollständig deployed)

14 Features (FOTO-1 bis FOTO-14), alle deployed:

- User Registration, Login, Rollen (Voter → Photographer → Admin)
- Photo Upload, 1v1 Battle & Voting
- Elo Rating Engine mit Event Sourcing
- Ranking & Leaderboard
- Kommentare, Likes, Follow-System
- User-Profile, Kategorien, Tags
- Battle-Historie, Statistiken, Benachrichtigungen
