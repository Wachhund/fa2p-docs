# MCP-Server

Die Website bietet einen **MCP-Server** (Model Context Protocol), der alle 12 Read-API-Endpunkte als Tools bereitstellt.

## Funktionsweise

- **Protokoll**: JSON-RPC 2.0 über Streamable HTTP Transport
- **Transport**: `POST /mcp` (JSON-RPC) + `GET /mcp` (SSE-Stream mit Endpoint-Discovery)
- **Tools**: 12 Read-Only-Tools (kein Schreibzugriff über MCP)
- **Server-Name**: `fa2p-mcp`, Version `1.0.0`

## Verfügbare MCP-Tools

| Tool | Beschreibung |
|------|-------------|
| `get_events` | Events abrufen (optionaler `type`-Filter: `archive` für vergangene) |
| `get_characters` | Vereinscharaktere (optionaler `lang`-Parameter) |
| `get_fursuits` | Verfügbare Fursuits |
| `get_fursuit_types` | Fursuit-Typen |
| `get_fursuit_parts` | Fursuit-Bestandteile |
| `get_photo_links` | Event-Foto-Links |
| `get_faq` | FAQ-Einträge (nach `type` und `lang`) |
| `get_downloads` | Öffentliche Downloads |
| `get_press` | Presseartikel |
| `get_rhoen_dance_stats` | RhönDance-Daten (Kapazität, belegte Plätze) |
| `get_social_links` | Social-Media-Links |
| `get_translations` | Übersetzungs-Strings (nach `lang`) |

## MCP-Endpunkt

- **Produktion**: `https://mcp.fa2p.de/mcp`
- **Lokal (Docker)**: `http://localhost:8888/mcp`
