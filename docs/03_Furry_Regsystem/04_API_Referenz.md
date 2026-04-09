# API-Referenz

Alle Endpunkte liegen unter `/api/`. Externe Ressourcen-IDs verwenden durchgehend UUIDv7 (`public_id`).

## Fehlerformat

```json
{
    "message": "Beschreibung des Fehlers",
    "errors": {
        "feldname": ["Validierungsfehlermeldung"]
    }
}
```

Das Feld `errors` ist nur bei Validierungsfehlern (HTTP 422) vorhanden.

---

## Präfix-Übersicht

| Präfix | Auth | Zweck | Endpunkte |
|--------|------|-------|-----------|
| `v1/auth/` | Öffentlich / JWT | Login, Token-Refresh, Geräteverwaltung | 5 |
| `v1/public/` | Keine | Öffentliche Event-Liste, Registrierung, Online-Zahlung | 5 |
| `v1/o/{orgSlug}/` | Kontextabhängig | Organisations-scoped (Branding, Payment-Config, Reader) | 13 |
| `v1/sync/` | JWT | Offline-Sync: Bootstrap, Upload, Delta | 4 |
| `v1/` | JWT | Events, Attendees, Payments, Compliance, Search, Dashboard, Audit | ~50 |
| `pretix/v1/` | API-Key | Pretix-kompatible Endpoints | 1 |

---

## Authentifizierung (`v1/auth/`)

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `POST` | `/api/v1/auth/login` | Anmelden, Token-Set ausgeben |
| `POST` | `/api/v1/auth/refresh` | Access-Token per Refresh-Token rotieren |
| `POST` | `/api/v1/auth/logout` | Aktives Gerät widerrufen |
| `GET` | `/api/v1/auth/devices` | Eigene Geräte auflisten |
| `DELETE` | `/api/v1/auth/devices/{devicePublicId}` | Einzelnes Gerät widerrufen |

## Öffentliche Endpunkte (`v1/public/`)

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `/api/v1/public/events` | Alle öffentlichen Events auflisten |
| `GET` | `/api/v1/public/branding/header` | Öffentliche Branding-Daten (Logo, Name) |
| `GET` | `/api/v1/public/events/{eventPublicId}/registration/schema` | Registrierungsformular-Schema laden |
| `POST` | `/api/v1/public/events/{eventPublicId}/registration/submit` | Registrierung einreichen |
| `POST` | `/api/v1/public/payments/registrations/{registrationPublicId}/online-checkout` | Online-Zahlung starten |

## Events

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `/api/v1/events` | Events auflisten |
| `POST` | `/api/v1/events` | Neues Event anlegen |
| `GET` | `/api/v1/events/{eventPublicId}` | Event-Details abrufen |
| `PUT` | `/api/v1/events/{eventPublicId}` | Event bearbeiten |
| `POST` | `/api/v1/events/{eventPublicId}/publish` | Event veröffentlichen |
| `POST` | `/api/v1/events/{eventPublicId}/archive` | Event archivieren |

## Registrierungsfelder

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `/api/v1/events/{eventPublicId}/registration-fields` | Felder auflisten |
| `POST` | `/api/v1/events/{eventPublicId}/registration-fields` | Feld anlegen |
| `PUT` | `.../registration-fields/{fieldPublicId}` | Feld bearbeiten |
| `POST` | `.../registration-fields/{fieldPublicId}/options` | Option anlegen |
| `PUT` | `.../registration-fields/{fieldPublicId}/options/{optionPublicId}` | Option bearbeiten |
| `GET` | `/api/v1/events/{eventPublicId}/offline/checkin-answers` | Offline-Antworten exportieren |

## Teilnehmende

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `/api/v1/events/{eventPublicId}/attendees` | Teilnehmendenliste |
| `PATCH` | `.../attendees/{registrationPublicId}/status` | Status manuell setzen |
| `GET` | `.../attendees/export.csv` | CSV-Export (Streaming) |

## Zahlungen

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `.../registrations/{registrationPublicId}/payment` | Zahlungsstatus abrufen |
| `POST` | `.../payment/terminal` | Terminal-Checkout starten |
| `POST` | `.../payment/online-fallback` | Online-Fallback-Checkout starten |
| `POST` | `/api/v1/payments/{paymentPublicId}/cancel` | Terminal-Checkout abbrechen |
| `POST` | `/api/v1/payments/{paymentPublicId}/refund` | Zahlung erstatten |
| `POST` | `/api/v1/payments/webhooks/sumup` | SumUp-Webhook empfangen |

## Addon-Katalog und -Bestellungen

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `/api/v1/events/{eventPublicId}/addons` | Addons auflisten |
| `POST` | `/api/v1/events/{eventPublicId}/addons` | Addon anlegen |
| `GET` | `.../addons/{addonPublicId}` | Addon abrufen |
| `PATCH` | `.../addons/{addonPublicId}` | Addon bearbeiten |
| `DELETE` | `.../addons/{addonPublicId}` | Addon entfernen |
| `GET` | `/api/v1/events/{eventPublicId}/addon-orders` | Bestellungen auflisten |
| `POST` | `/api/v1/events/{eventPublicId}/addon-orders` | Bestellung anlegen |
| `GET` | `.../addon-orders/{orderPublicId}` | Bestellung abrufen |
| `POST` | `.../addon-orders/{orderPublicId}/cancel` | Bestellung stornieren |

## Self-Service

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `/api/v1/me/registrations` | Eigene Registrierungen auflisten |
| `GET` | `/api/v1/me/registrations/{registrationPublicId}` | Registrierung abrufen |
| `PATCH` | `.../registrations/{registrationPublicId}/profile` | Profildaten aktualisieren |
| `POST` | `.../registrations/{registrationPublicId}/cancel` | Registrierung stornieren |

## Compliance (DSGVO)

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `POST` | `/api/v1/compliance/consents` | Einwilligung aufzeichnen |
| `POST` | `/api/v1/compliance/exports` | Datenexport anfordern |
| `POST` | `/api/v1/compliance/deletions` | Löschanfrage stellen |
| `GET` | `/api/v1/compliance/requests/{requestPublicId}` | Anfragestatus prüfen |
| `POST` | `/api/v1/compliance/legal-holds` | Legal Hold setzen |
| `DELETE` | `/api/v1/compliance/legal-holds/{legalHoldPublicId}` | Legal Hold aufheben |

## Benachrichtigungen

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `/api/v1/notifications/templates` | Vorlagen auflisten |
| `POST` | `/api/v1/notifications/templates` | Vorlage anlegen |
| `GET` | `/api/v1/notifications/templates/placeholders` | Verfügbare Platzhalter |
| `GET` | `.../templates/{templatePublicId}` | Vorlage abrufen |
| `PATCH` | `.../templates/{templatePublicId}` | Vorlage bearbeiten |
| `GET` | `/api/v1/notifications/preferences` | Eigene Präferenzen abrufen |
| `PUT` | `/api/v1/notifications/preferences` | Präferenzen aktualisieren |

## Organisations-Branding (`v1/o/{orgSlug}/`)

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `.../branding/header` | Privates Branding abrufen |
| `POST` | `.../branding/logo` | Organisations-Logo hochladen |
| `DELETE` | `.../branding/logo` | Organisations-Logo entfernen |
| `POST` | `.../events/{eventPublicId}/branding/logo` | Event-Logo hochladen |
| `DELETE` | `.../events/{eventPublicId}/branding/logo` | Event-Logo entfernen |
| `GET` | `.../payments/config` | Zahlungskonfiguration abrufen |
| `PUT` | `.../payments/config` | Zahlungskonfiguration setzen |
| `GET` | `.../payments/readers` | Terminal-Reader auflisten |
| `POST` | `.../payments/readers/pair` | Reader koppeln |
| `PATCH` | `.../payments/readers/{readerPublicId}` | Reader aktualisieren |
| `DELETE` | `.../payments/readers/{readerPublicId}` | Reader entkoppeln |
| `GET` | `.../payments/readers/{readerPublicId}/status` | Reader-Status prüfen |

## Weitere Endpunkte

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `GET` | `/api/v1/branding/header` | Privates Branding ohne Org-Kontext |
| `GET` | `/api/v1/dashboard/overview` | Dashboard-Kennzahlen |
| `GET` | `/api/v1/audit/entries` | Audit-Log-Einträge |
| `GET` | `/api/v1/search` | Volltextsuche über Events und Teilnehmende |

## Offline-Sync (`v1/sync/`)

| Methode | Pfad | Beschreibung |
|---------|------|--------------|
| `POST` | `/api/v1/sync/bootstrap` | Initialen Datensatz laden |
| `POST` | `/api/v1/sync/upload` | Lokale Mutations hochladen |
| `POST` | `/api/v1/sync/delta` | Inkrementelles Update seit letztem Checkpoint |
| `GET` | `/api/v1/sync/status/{eventPublicId}` | Sync-Status und Metriken |

## Pretix-Kompatibilität (`pretix/v1/`)

| Methode | Pfad | Auth | Beschreibung |
|---------|------|------|--------------|
| `GET` | `/api/pretix/v1/organizers/{orgSlug}/events/{eventRef}` | API-Key | Event-Metriken im Pretix-Format |
