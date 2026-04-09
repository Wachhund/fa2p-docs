# Fa2P Vereinswebsite v3

Entwurf für die offizielle Website des Furry-Vereins **Freunde auf 2 Pfoten e. V.**

Zweisprachig (Deutsch/Englisch), mit öffentlicher Website, geschütztem Admin-Bereich und JSON-API.

| Eigenschaft | Wert |
|-------------|------|
| Repository | [Wachhund/freundeauf2pfoten](https://github.com/Wachhund/freundeauf2pfoten) |
| Zeitraum | 12.04.2025 (init v3.0.0) – 18.03.2026 (archiviert v3.7.0) |
| Commits | 185 |
| Stack | PHP 8.3, MySQL 8.0, Bootstrap 5, jQuery 3.7 |
| Status | Archiviert |

---

## Hintergrund

- Neues Repository, weil über einen externen Dienstleister eine neue Website erstellt wurde — kein Zusammenhang mit der v2-Website
- Keine Iteration der Entwicklung durch den Dienstleister bekannt, daher als v3.0.0 initialisiert
- Keine SQL-Daten vom Dienstleister erhalten — Datenbankschema wurde reverse-engineered
- Die Entwicklung wurde nie veröffentlicht

## Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Sprache | PHP 8.3 |
| Datenbank | MySQL 8.0 (PDO, Prepared Statements) |
| Frontend | Bootstrap 5, jQuery 3.7 |
| PDF-Generierung | FPDF |
| MCP-Server | Model Context Protocol für KI-Integration |
| Deployment | Docker (Apache 2.4 + PHP Built-in Server für MCP) |
| Analyse | PHPStan Level 6 |
