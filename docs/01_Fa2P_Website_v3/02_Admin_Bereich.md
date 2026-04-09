# Admin-Bereich

Der Admin-Bereich ist unter `/admin` erreichbar und erfordert eine Anmeldung mit Benutzername und Passwort. Jeder Admin sieht nur die Module, für die er Berechtigungen hat.

## Dashboard

- Statistik-Karten: Anstehende Events, Anzahl Charaktere, verfügbare Fursuits, Kontaktanfragen (Monat/Gesamt)
- Übersicht der nächsten 5 Events
- Die letzten 5 Kontaktanfragen im Schnellüberblick
- Aktions-Log: Die letzten 10 Admin-Aktionen (nur für Admins mit Benutzerverwaltungsrecht sichtbar)
- Quick-Links zu häufig genutzten Modulen

## Event-Verwaltung

- Kompakte Listenansicht aller Events mit Datum, Titel, Typ und Status-Icons
- **Offcanvas-Detail-Panel**: Klick auf eine Zeile öffnet ein Seitenpanel mit allen Feldern in logischen Gruppen (Basis, Details, Medien, Pretix-Integration)
- Felder für Pretix-Integration: Pretix-Event-Slug und Ticket-Quota-ID zur automatischen Platzanzeige
- Event-Typ (Verein/Extern) und Sichtbarkeitsschalter
- Neues Event anlegen und bestehendes löschen
- **Event-Fotos**: Verwaltung von Foto-Links pro Event (Fotograf, Link-URL, Plattform, Sichtbarkeit)
- Bei neuem Event wird automatisch eine Telegram-Benachrichtigung an den Vereinskanal gesendet

## Kontaktanfragen

- Auflistung aller eingegangenen Kontaktformular-Nachrichten
- Anzeige von Anrede, Name, E-Mail, Telefon, Nachricht und Eingangszeit

## Presse-Verwaltung

- Kompakte Liste aller Presseartikel mit Datum, Text-Vorschau und Link-Icon
- Offcanvas-Panel zum Bearbeiten: Datum, Text, URL

## Charaktere-Verwaltung

- Kompakte Liste mit Thumbnail, Name und Position
- Offcanvas-Panel: Titel (DE/EN), Beschreibungstext (DE/EN), Position, Bild-Upload mit Vorschau
- Unterstützte Formate: JPG, PNG, AVIF, WebP

## Fursuitverleih-Verwaltung

- Kompakte Liste mit Vorschaubild, Name, Typ, Preis und Verfügbarkeits-Icon
- Offcanvas-Panel mit Feldgruppen: Basis, Preise, Maße, Teile (Checkboxen), Bilder
- Bild-Upload für Vorschau- und Detailansicht

## Downloads-Verwaltung

- Kompakte Liste mit Titel, Position, Sichtbarkeit und Download-Counter
- Offcanvas-Panel: Titel (DE/EN), Beschreibung (DE/EN), Datei-Upload (PDF), Sichtbarkeit, Position

## Image Manager

- Bilder hochladen mit **Modul-Zuordnung**: Zielbereich (Allgemein, Events, Charaktere, Fursuits) und Eintrags-Auswahl per AJAX-Dropdown
- Bild wird automatisch im richtigen Verzeichnis gespeichert und dem Eintrag in der Datenbank zugewiesen
- Galerie mit Modul-Badges (farbcodiert), Filter-Buttons und URL-Kopier-Funktion
- Resize-Option (max. 1920 px) bei Upload

## FAQ-Verwaltung

Drei separate Admin-Module für die FAQ-Bereiche:

- **FAQ Allgemein**: Fragen und Antworten zum Verein
- **FAQ RhönDance**: Fragen und Antworten zur RhönDance-Veranstaltung
- **FAQ Suitverleih**: Fragen und Antworten zum Fursuit-Verleih

Jeweils kompakte Liste mit Frage-Vorschau und EN-Indikator; Offcanvas-Panel mit Frage/Antwort (DE/EN) und Sortierung.

## Mailer (SMTP-Konfiguration)

- SMTP-Einstellungen für den E-Mail-Versand des Kontaktformulars konfigurieren
- Felder: SMTP-Host, Port, Benutzername, Passwort, Empfänger-Adresse

## RhönDance-Verwaltung

- Termin, Beschreibung, Kapazität und belegte Plätze für die RhönDance-Veranstaltung pflegen

## Admin-Verwaltung

- Kompakte Liste mit Benutzername und Rollen-Badges
- Offcanvas-Panel: Benutzername, Passwort, Permission-Checkboxen in 2-Spalten-Grid
- Eigene Berechtigungen können nicht geändert werden (Self-Edit-Restriction)
- **Granulares Berechtigungssystem**: Für jedes Modul einzeln einstellbar, welcher Admin Zugriff hat (Events, Kontakt, Presse, Charaktere, Fursuitverleih, Downloads, Image Manager, FAQ, Mailer, RhönDance, Admins)
