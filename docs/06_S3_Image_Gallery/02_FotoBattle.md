# FotoBattle

Kompetitive Foto-Battle-Plattform: Fotografen reichen Fotos ein, Voter wählen in 1v1-Duellen ihren Favoriten, ein Elo-Rating-System berechnet objektive Rankings.

## Architektur

Hybrider Ansatz: **Event Sourcing** für die kompetitiven Kerndomänen, **CRUD** für den Rest.

```text
┌─────────────────────────────────────────────┐
│           FotoBattle Domains                │
├──────────────────┬──────────────────────────┤
│   CRUD           │   Event Sourced (ES)     │
├──────────────────┼──────────────────────────┤
│ User Management  │ Battle Domain            │
│ Photo Management │ Rating Domain            │
│ Social Features  │ Infrastructure           │
│ Read-Side        │  (Event Store)           │
└──────────────────┴──────────────────────────┘
```

## Rollen

| Rolle | Beschreibung |
|-------|-------------|
| **Besucher** | Galerie, Leaderboard und Top-Fotos ansehen |
| **Voter** | Registrierung mit E-Mail/Passwort. Abstimmen, kommentieren, liken, folgen |
| **Photographer** | Muss von Admin freigeschaltet werden. Fotos hochladen und an Battles einreichen |
| **Admin** | Fotografen freischalten, Battles verwalten, Moderation, Statistiken |

## Features

### MVP (P0)

| ID | Feature | Beschreibung |
|----|---------|-------------|
| FOTO-1 | User Registration & Login | E-Mail/Passwort, bcrypt, Rate Limiting (5 Versuche → 15 Min Sperre) |
| FOTO-2 | Photographer Approval & Roles | Bewerbung, Admin-Freischaltung, Rollenwechsel |
| FOTO-3 | Photo Upload & Management | Upload in Battles, Status-Verwaltung (pending → active → withdrawn) |
| FOTO-4 | Event Store & Aggregate Infrastructure | MySQL-basiertes Event Sourcing mit Optimistic Locking |
| FOTO-5 | 1v1 Photo Battle & Voting | Paarweises Matchmaking (min. 10 Fotos), Duplikat-Schutz (24h) |
| FOTO-6 | Elo Rating Engine | K-Faktor: 40 (neu) / 20 (etabliert), Floor 100, Start 1500 |
| FOTO-7 | Ranking & Leaderboard | Foto- und Fotografen-Rankings aus `rating_history` |

### Social (P1)

| ID | Feature | Beschreibung |
|----|---------|-------------|
| FOTO-8 | Comments | Kommentare auf Fotos |
| FOTO-9 | Likes | Favoriten-Toggle (unique pro User+Entry) |
| FOTO-10 | Follow System | Fotografen folgen |
| FOTO-11 | User Profiles & Galleries | Profil mit Avatar, Bio, Foto-Galerie |

### Advanced (P2)

| ID | Feature | Beschreibung |
|----|---------|-------------|
| FOTO-12 | Categories & Tags | Battle- und Entry-Kategorisierung |
| FOTO-13 | Battle History & Statistiken | Voting-Volumen, Top-Voter, Battle-Statistiken |
| FOTO-14 | Benachrichtigungen | In-App-Notifications |

## Elo-Rating

```text
Erwartete Punktzahl:  E_A = 1 / (1 + 10^((R_B - R_A) / 400))
Neues Rating:         R'_A = R_A + K × (S - E_A)

K-Faktor:  40 (< 30 Battles)  /  20 (≥ 30 Battles)
Floor:     100 (Ratings fallen nie unter 100)
Startwert: 1500
```

## Event Store

Eigene Implementierung (kein externer EventStore):

- `append(AggregateRoot)` — Event in `event_store` Tabelle schreiben
- `getEvents(aggregateId)` — Alle Events für ein Aggregat laden
- Optimistic Locking über `aggregate_id + event_version` (Unique)
- `ConcurrencyException` bei Versionskonflikten
- Projektionen: `elo_rating` auf `battle_entries` (Cache), `rating_history` (Leaderboard)

## Routing

### Öffentlich

| Methode | Pfad | Beschreibung |
|---------|------|-------------|
| `GET` | `/` | Startseite (abstimmbare Battles) |
| `GET/POST` | `/register` | Registrierung |
| `GET/POST` | `/login` | Login |
| `POST` | `/logout` | Abmelden |
| `GET` | `/leaderboard` | Foto-Rankings |
| `GET` | `/leaderboard/photographers` | Fotografen-Rankings |
| `GET` | `/vote/{id}` | Battle-Arena (1v1) |
| `POST` | `/api/vote` | Abstimmung (AJAX) |
| `GET` | `/entry/{id}` | Foto-Detailseite |
| `POST` | `/api/favorite/{id}` | Like-Toggle |

### Authentifiziert

| Methode | Pfad | Beschreibung |
|---------|------|-------------|
| `GET/POST` | `/apply` | Fotografen-Bewerbung |
| `GET` | `/my-entries` | Eigene Fotos |
| `GET` | `/profile/{username}` | User-Profil |
| `GET` | `/following` | Following-Liste |
| `POST` | `/api/follow/{id}` | Follow-Toggle |

### Admin

| Methode | Pfad | Beschreibung |
|---------|------|-------------|
| `GET/POST` | `/admin/battles` | Battles erstellen und verwalten (draft → open → voting → closed) |
| `GET/POST` | `/admin/photographers` | Bewerbungen freischalten |
| `GET/POST` | `/admin/users` | Benutzerverwaltung |
| `GET` | `/admin/statistics` | Statistiken und Ratings |

### Gallery-Integration (HMAC-geschützt)

| Methode | Pfad | Beschreibung |
|---------|------|-------------|
| `POST` | `/api/gallery-submit` | Foto aus Gallery an Battle senden |
| `POST` | `/api/gallery-withdraw` | Foto aus Battle zurückziehen |

## Datenbank (FotoBattle-Tabellen)

### User & Auth

| Tabelle | Beschreibung |
|---------|-------------|
| `users` | Username, E-Mail, Passwort-Hash, Rolle (voter/photographer/admin), Avatar, Bio |
| `photographer_applications` | Bewerbungen mit Status (pending/approved/rejected) |
| `login_attempts` | Rate Limiting per IP-Hash |

### Battles & Voting

| Tabelle | Beschreibung |
|---------|-------------|
| `battles` | Titel, Status (draft/open/voting/closed), Zeitfenster |
| `battle_entries` | Fotos in Battles, FK auf `images.id` (Gallery), Elo-Rating-Cache |
| `battle_votes` | Abstimmungen (left/right/winner Entry, User, IP-Hash) |

### Rating & Events

| Tabelle | Beschreibung |
|---------|-------------|
| `rating_history` | Elo-Verlauf pro Entry (alt → neu, K-Faktor, Gegner) |
| `event_store` | Event Sourcing (aggregate_id, type, data JSON, version) |

### Social

| Tabelle | Beschreibung |
|---------|-------------|
| `comments` | Kommentare auf Entries |
| `favorites` | Likes (unique pro User+Entry) |
| `follows` | Follow-Beziehungen (unique pro Paar) |
| `notifications` | In-App-Benachrichtigungen |

### Metadaten

| Tabelle | Beschreibung |
|---------|-------------|
| `categories` + `entry_categories` | Kategorien (M:N) |
| `tags` + `entry_tags` | Tags (M:N) |

## Tests

19 PHPUnit-Testdateien:

- Event Store & Aggregate (2 Tests)
- Elo-Berechnung & Rating-Service (2 Tests)
- Auth, Battle, Vote, Entry Services (4 Tests)
- Social Services: Comment, Favorite, Follow, Profile, Notification (5 Tests)
- Leaderboard, Category, Role, Statistics, Tag, RateLimiter (6 Tests)
