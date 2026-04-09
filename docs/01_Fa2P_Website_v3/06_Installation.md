# Installation mit Docker

## Voraussetzungen

- [Docker](https://docs.docker.com/get-docker/) (inkl. Docker Compose)
- [Git](https://git-scm.com/)

## 1. Repository klonen

```bash
git clone git@github.com:Wachhund/freundeauf2pfoten.git
cd freundeauf2pfoten
```

## 2. Umgebungsvariablen konfigurieren

Die Datei `.env.docker` enthält alle relevanten Konfigurationswerte. Die Standardwerte funktionieren für eine lokale Entwicklungsumgebung:

| Variable | Standard | Beschreibung |
|----------|----------|-------------|
| `APP_PORT` | `15080` | Port, unter dem die Website erreichbar ist |
| `MCP_PORT` | `8888` | Port für den MCP-Server |
| `DB_PORT` | `13306` | Port für direkten MySQL-Zugriff (optional) |
| `DB_NAME` | `freundeauf2pfoten` | Name der Datenbank |
| `DB_USER` | `fa2p` | Datenbank-Benutzer |
| `DB_PASSWORD` | `fa2p` | Datenbank-Passwort |
| `DB_ROOT_PASSWORD` | `root` | MySQL-Root-Passwort |
| `MCP_BASE_URL` | `https://mcp.fa2p.de` | Öffentliche MCP-Server-URL |
| `APP_URL` | `https://freundeauf2pfoten.de` | Basis-URL für SEO und OG-Tags |
| `PRETIX_API_TOKEN` | *(leer)* | Pretix API-Token (optional) |
| `TELEGRAM_BOT_TOKEN` | *(leer)* | Telegram Bot-Token (optional) |
| `TELEGRAM_CHAT_ID` | *(leer)* | Telegram Chat-ID (optional) |

Die `config.php` wird beim Start automatisch aus diesen Variablen generiert (`FA2P_GENERATE_CONFIG=1`).

## 3. Container starten

```bash
docker compose --env-file .env.docker up -d --build
```

Das startet drei Container:

- **web**: PHP 8.3 mit Apache (Port 15080)
- **mcp**: PHP 8.3 MCP-Server (Port 8888)
- **db**: MySQL 8.0 (Port 13306)

Beim ersten Start wird die Datenbank automatisch aus `admin/create_sql.sql` initialisiert.

## 4. Website aufrufen

| Adresse | Zweck |
|---------|-------|
| `http://localhost:15080` | Website |
| `http://localhost:15080/admin` | Admin-Bereich |
| `http://localhost:8888/mcp` | MCP-Server |

## Nützliche Docker-Befehle

```bash
# Container stoppen
docker compose --env-file .env.docker down

# Container stoppen und Datenbank-Volume löschen (Neustart)
docker compose --env-file .env.docker down -v

# Logs anzeigen
docker compose --env-file .env.docker logs -f web

# Datenbank manuell zurücksetzen
docker compose --env-file .env.docker exec -T db mysql --default-character-set=utf8mb4 \
  -u fa2p -pfa2p freundeauf2pfoten < admin/create_sql.sql
```

## Externe Dienste

Die folgenden Dienste sind **optional** und werden durch ihre Tokens in `.env.docker` aktiviert:

### Pretix (Ticketverkauf)

Wenn `PRETIX_API_TOKEN` gesetzt ist, werden für Events mit hinterlegtem Pretix-Slug automatisch die verkauften/verfügbaren Tickets abgefragt und als Fortschrittsbalken angezeigt.

- Pretix-Instanz: `https://pretix.eu/fa2p/`
- Organizer-Slug: `fa2p`

### Telegram-Benachrichtigungen

Wenn `TELEGRAM_BOT_TOKEN` und `TELEGRAM_CHAT_ID` gesetzt sind, werden bei neuen Kontaktanfragen und neuen Events automatisch Benachrichtigungen an den konfigurierten Telegram-Chat gesendet.
