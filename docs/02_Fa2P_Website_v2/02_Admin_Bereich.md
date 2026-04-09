# Admin-Bereich

Der Admin-Bereich existiert in zwei Versionen (`mau/` und `mauV2/`).

## Authentifizierung

Der Login erfolgt über **Telegram OAuth** mit dem Vereins-Bot (@Fa2P_WebBOT):

- Telegram Login Widget im Browser
- Server-seitige HMAC-SHA256 Hash-Verifizierung gegen Manipulation
- Auth-Token-Gültigkeit: 24 Stunden
- Berechtigungsprüfung über Telegram-User-ID

## Admin-Panel v1 (`mau/`)

- Einfache HTML-Formulare
- Direkte Datenbankoperationen
- Cookie-basierte Session-Verwaltung

## Admin-Panel v2 (`mauV2/`)

Überarbeitete Version mit:

- Verbesserte Telegram-Authentifizierung mit Hash-Verifizierung
- Moderne Database-Wrapper-Klasse (Insert/Update/Select/Remove)
- Session-basierte Authentifizierung
- RESTful-orientierte Handler

## Verwaltungsmodule

| Modul | Beschreibung |
|-------|-------------|
| Events anlegen | Neue Veranstaltungen erstellen |
| Events bearbeiten | Bestehende Events ändern |
| Events auflisten | Übersicht aller Events |
| Charaktere anlegen/bearbeiten | Galerie-Einträge verwalten |
| Newsletter | Newsletter-Artikel veröffentlichen |
| Presse | Presseerwähnungen hinzufügen |
| FAQ-Verwaltung | Fragen und Antworten pflegen |
| RhönDance-Registrierung | Anmeldungen verwalten |
| Fursuit-Verleih | Mietanfragen bearbeiten |
| Telegram-Benachrichtigungen | Event-Benachrichtigungen an Vereinskanal senden |
