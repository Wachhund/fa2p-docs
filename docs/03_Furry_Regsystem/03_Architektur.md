# Architektur

## Dual-ID-System

Jede Ressource hat zwei IDs mit klar getrennten Zuständigkeiten:

| ID-Typ | Spalte | Format | Verwendung |
|--------|--------|--------|------------|
| Interne ID | `id` | `BIGINT AUTO_INCREMENT` | Fremdschlüssel, Joins innerhalb der Datenbank |
| Externe ID | `public_id` | UUIDv7 | Alle API-Responses, URL-Parameter, Sync-Protokoll |

Die externe API und der Sync-Layer kennen ausschließlich `public_id`. Interne PKs werden niemals nach außen gegeben.

Das Trait `HasPublicId` (`app/Models/Concerns/HasPublicId.php`) erzeugt die UUIDv7 automatisch im `creating`-Hook via `Str::uuid7()`.

## Multi-Tenancy

Organizations sind der Mandanten-Anker. Jeder `User` gehört genau einer `Organization`, jedes `Event` und jede `Registration` ebenfalls.

Der URL-Parameter `{organizationSlug}` aktiviert die `ResolveOrganizationContext`-Middleware, die den Mandantenkontext in die Request-Lifecycle einbettet. Alle organisations-scoped Controller prüfen, ob die angeforderte Ressource zur Organisation des authentifizierten Benutzers gehört (BOLA-Schutz).

Branding-Fallback: Ist kein Event-Logo vorhanden, wird das Organisations-Logo verwendet.

## JWT-Authentifizierung

```text
Login
  └─ TokenService.issueTokenSet()
       ├─ Fingerprint prüfen → bekanntes Device wiederverwenden oder neues anlegen
       ├─ Access-Token ausgeben  (HS256, 15 Min, Claims: sub, uid, dti, roles, org_id)
       └─ Refresh-Token ausgeben (opak, SHA-256 gehashed, 30 Tage)

Jede authentifizierte Anfrage
  └─ AuthenticateWithJwt-Middleware
       ├─ JWT dekodieren und iss/aud/token_use prüfen
       └─ User aus DB laden und im Request-Resolver hinterlegen

Token-Rotation
  └─ POST /api/v1/auth/refresh
       ├─ Alten Refresh-Token konsumieren und invalidieren
       └─ Neues Token-Paar ausgeben (Replay-Schutz: Wiederverwendung widerruft Device)
```

Refresh-Tokens werden als HttpOnly-Cookie übertragen. Access-Tokens (In-Memory, 15 Min) werden niemals in localStorage persistiert.

## Eloquent Models (34)

**Core**: Event, Registration, RegistrationField, RegistrationFieldOption, User, Organization

**Payment**: Payment, PaymentAttempt, PaymentRefund, PaymentReader, OrganizationPaymentConfig, AddonOrder, AddonOrderItem, EventAddon

**Compliance & Privacy**: ConsentRecord, DataSubjectRequest, LegalHold, AuditEntry

**Operational**: CheckinRecord, Device, RefreshToken, ApiCredential, ExternalEventMapping

**Messaging**: NotificationTemplate, NotificationPreference, NotificationMessage, DeliveryAttempt

**Search & Sync**: SearchDocument, SyncCheckpoint, SyncMutation

**Answers**: RegistrationAnswer, RegistrationAnswerOptionSelection

## Migrationen (43)

Wichtige Schema-Patterns:

- **Public IDs (UUIDs)** auf allen Haupt-Entitäten
- **Composite Indexes** für Performance
- **Soft Deletes** auf: Events, Registrations, Payments
- **Notification-System**: Template-Versionierung (Channel + Version), Idempotency Keys, Delivery-Attempt-Tracking
- **Compliance-first**: Consent Records, DSR-Tracking, Legal Holds mit Metadata-JSON
- **Payment-Infrastruktur**: Multi-Stage Attempts/Refunds, Reader-Mapping, SumUp-Webhook-Support
- **Addon-System**: Stock-Tracking, Soft Deletes, Sort Order
- **Audit Trail**: Device-/Actor-Tracking, User-Agent in AuditEntry
- **Sync-System**: Checkpoints, Mutations mit Device- und Idempotency-Tracking

## Frontend: Vue 3 SPA

Das Frontend ist eine eigenständige Vue 3 Single Page Application unter `resources/js/spa/`.

### Komponenten (10)

**Basis-UI**: AppBadge, AppButton, AppErrorBoundary, AppFormInput, AppFormSelect, AppNotice, AppPanel

**Feature-spezifisch**: CheckinSearch, CheckinResult, QrScanner (Check-in-Modul)

### Views (14)

| View | Beschreibung |
|------|-------------|
| LoginView | Anmeldung |
| SessionView | Session-Verwaltung |
| DashboardView | Dashboard |
| EventManagementView | Event-Verwaltung |
| AttendeeManagementView | Teilnehmenden-Verwaltung |
| PublicRegistrationView | Öffentliche Registrierung |
| PublicEventsOverviewView | Öffentliche Event-Übersicht |
| MobileCheckinView | Mobiler Check-in mit QR-Scanner |
| AuditLogView | Audit-Log |
| ComplianceView | DSGVO-Verwaltung |
| SelfServiceView | Self-Service-Portal |
| PretixCompatView | Pretix-Kompatibilität |
| SearchView | Volltextsuche |
| SyncCenterView | Sync-Zentrale |

### Pinia Stores

- `auth.js` — JWT-Handling, Login/Logout, Token-Refresh, Geräteverwaltung
- `sync.js` — Offline-Sync-Status und Fortschrittsanzeige

### Axios-Client

Zentraler Client mit Request-Interceptor (Authorization-Header) und Response-Interceptor für automatischen Token-Refresh bei HTTP 401. Gleichzeitige 401-Fehler werden in einer Warteschlange gehalten und nach erfolgreichem Refresh nachgeholt.

## Offline-Synchronisation

```text
1. Bootstrap   POST /api/v1/sync/bootstrap
               → Vollständiger Datensatz (Registrierungen, Felder, Antworten)
               → Checkpoint mit Snapshot-Cursor anlegen
               → Chunking bei großen Datensätzen

2. Offline     Gerät arbeitet lokal, speichert Mutations (z.B. Check-ins)

3. Upload      POST /api/v1/sync/upload
               → Lokale Mutations an Server senden
               → ConflictResolutionService prüft Konflikte

4. Delta       POST /api/v1/sync/delta
               → Nur Änderungen seit letztem Checkpoint
               → Chunking für große Datensätze
```

Konfliktauflösung: Last-Write-Wins per Zeitstempel; abgelehnte Mutations landen mit Status `rejected` in `sync_mutations` für manuelle Überprüfung.

## Zahlungsfluss (SumUp)

```text
Online-Registrierung
  └─ POST /api/v1/public/payments/registrations/{id}/online-checkout
       ├─ PaymentCheckoutService erstellt Checkout bei SumUp
       └─ Redirect-URL zurückgeben → Teilnehmender schließt Zahlung ab

       Webhook → POST /api/v1/payments/webhooks/sumup
         └─ WebhookProcessingService verifiziert HMAC-Signatur
              └─ Payment-Status aktualisieren, Registration auf paid setzen

Terminal (Check-in)
  └─ POST .../payment/terminal
       ├─ SumUpGateway initiiert Terminal-Checkout am gekoppelten Reader
       └─ Polling oder Webhook signalisiert Abschluss

Fallback
  └─ POST .../payment/online-fallback
       → Online-Checkout für Registrierungen mit Terminal-Ausfall
```

## Service-Schicht

Die Geschäftslogik ist in 27 fachlich geschnittene Services unterteilt (`app/Services/`), gruppiert nach Domäne: Auth, Audit, Branding, Compliance, Dashboard, Events, Notifications, Payments, Registration, Search, Security, Sync, Addons, Pretix.

Drei shared Controller-Traits (`Concerns/`) reduzieren Boilerplate: `ResolvesActor`, `ResolvesManagedEvent`, `BuildsAuditContext`.

## Queue-Jobs

5 asynchrone Jobs für Suchindex-Pflege (`IndexEventJob`, `IndexRegistrationJob`, `ReindexOrganizationJob`, `RemoveSearchDocumentJob`) und Benachrichtigungsversand (`SendNotificationJob`), getriggert über Eloquent-Observers und den NotificationService.

## CI/CD

### laravel.yml (Push/PR auf master/main/develop)

- PHP 8.4, Node.js 24, SQLite in-memory
- Schritte: Composer/npm install → Frontend Build → Key Gen → Migrate → Pint Lint → PHPStan → Pest Tests
- Security-Job: Composer und npm Audits

### docker-smoke.yml (nur master)

- Docker-Image bauen und Container starten
- Health-Endpoint validieren
- Migrations-Status prüfen

## Docker (Produktion)

3-Service-Architektur:

| Service | Beschreibung |
|---------|-------------|
| **web** | Nginx + PHP-FPM (Multi-Stage Build: PHP 8.4 + Node 24) |
| **queue** | Queue Worker |
| **scheduler** | Task Scheduler |

Infrastruktur:

| Service | Konfiguration |
|---------|--------------|
| **MariaDB 11** | Produktionsdatenbank |
| **Redis 7** | Cache, Sessions, Queue (LRU Eviction, 128 MB) |

Health Checks: HTTP `/up` für Web, Queue-Monitor für Worker, `redis-cli ping` für Redis.

## Tests (29 Dateien)

- **27 Feature-Tests**: Addon, Attendee, Audit Log, Auth, Branding, Compliance, Dashboard, Event Management, Payment, Registration, Search, SelfService, SyncCenter, Pretix Compat, Security
- **2 Unit-Tests**
- **18 Factories** für Testdaten-Generierung
- Test-Isolation: SQLite `:memory:`, Array-Cache/Session

## Verzeichnisstruktur

```text
RegSys/
├── app/
│   ├── Enums/                           # PHP-Enums (UserRole, etc.)
│   ├── Http/
│   │   ├── Controllers/Api/V1/          # 20 REST-Controller + Auth/ + Concerns/
│   │   ├── Controllers/Api/Pretix/V1/   # Pretix-Kompatibilitäts-API
│   │   ├── Middleware/                   # JWT, Pretix-Auth, Org-Context, Security-Headers
│   │   └── Requests/Api/V1/             # FormRequests mit Validierungsregeln
│   ├── Jobs/                            # 5 Queue-Jobs (Suche, Benachrichtigungen)
│   ├── Models/                          # 34 Eloquent-Models + Concerns/HasPublicId
│   ├── Observers/                       # Event- und Registration-Lifecycle
│   └── Services/                        # 27 Services in 14 Domänen-Namespaces
├── database/
│   ├── factories/                       # 18 Model-Factories für Tests
│   └── migrations/                      # 43 Migrationen
├── docker/entrypoint.sh                 # Container-Startskript
├── resources/js/spa/                    # Vue 3 SPA
│   ├── components/                      # 10 Komponenten (UI + Check-in)
│   ├── views/                           # 14 Vue-Views
│   ├── stores/                          # Pinia: auth.js, sync.js
│   ├── router/index.js                  # Lazy-Loading + Route Guards
│   └── utils/                           # Axios-Client, Formatters
├── routes/api.php                       # Alle API-Routen
├── tests/                              # 29 Test-Dateien (Feature + Unit)
├── Dockerfile                           # Multi-Stage Build (PHP 8.4 + Node 24)
├── docker-compose.yml                   # Dev-Setup (SQLite, Port 18080)
└── docker-compose.production.yml        # Prod-Setup (MariaDB 11, Redis 7)
```
