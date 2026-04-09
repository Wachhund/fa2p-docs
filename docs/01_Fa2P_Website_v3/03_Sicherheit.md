# Sicherheitsvorkehrungen

## Öffentlicher Bereich

| Maßnahme | Beschreibung |
|----------|-------------|
| **CSRF-Schutz** | Kontaktformular und Beitrittsantrag sind durch ein CSRF-Token geschützt. Anfragen ohne gültiges Token werden abgelehnt. |
| **Rate-Limiting** | Pro IP-Adresse maximal 5 Kontaktanfragen innerhalb von 15 Minuten. Weitere Anfragen werden stillschweigend verworfen. |
| **Honeypot-Feld** | Ein unsichtbares Formularfeld fängt automatisierte Spam-Bots ab. |
| **Eingabevalidierung** | Alle Formulareingaben werden auf Länge, Format (z.B. E-Mail) und Pflichtfelder geprüft. |
| **XSS-Schutz** | Alle Benutzereingaben werden vor der Ausgabe im HTML kodiert. |
| **SQL-Injection-Schutz** | Sämtliche Datenbankzugriffe erfolgen über parametrisierte Abfragen (Prepared Statements). |
| **Upload-Validierung** | Hochgeladene Dateien werden über MIME-Typ-Prüfung und Dateiendungs-Whitelist validiert. |

## Admin-Sicherheit

| Maßnahme | Beschreibung |
|----------|-------------|
| **Authentifizierung** | Zugang nur mit gültigem Benutzernamen und Passwort. Alle Seiten prüfen den Login-Status. |
| **Brute-Force-Schutz** | Nach 3 fehlgeschlagenen Anmeldeversuchen innerhalb von 10 Minuten wird der Account für 60 Minuten gesperrt. |
| **Sichere Passwort-Speicherung** | Passwörter werden mit bcrypt gehasht. Veraltete Hashes werden bei erfolgreicher Anmeldung automatisch aktualisiert. |
| **Session-Sicherheit** | Nach erfolgreichem Login wird die Session-ID neu generiert (Session-Fixation-Schutz). |
| **CSRF-Schutz** | Alle Admin-Formulare und AJAX-Anfragen sind durch CSRF-Tokens geschützt. |
| **Berechtigungssystem** | Granulare Berechtigungen pro Modul. Jeder Admin sieht und nutzt nur die Funktionen, für die er freigeschaltet ist. |
| **Aktions-Log** | Änderungen durch Admins werden in einem Audit-Log protokolliert und sind im Dashboard einsehbar. |
| **Upload-Validierung** | Bild- und PDF-Uploads werden serverseitig über MIME-Typ und Dateigröße geprüft. |
