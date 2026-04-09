# Thüringer Furdance — Websites

Websites für die Furry-Tanzveranstaltung **Thüringer Furdance** (TFD). Das Repository enthält die Websites für zwei Ausgaben der Veranstaltung.

| Eigenschaft | Wert |
|-------------|------|
| Repository | [Wachhund/tfd-website](https://github.com/Wachhund/tfd-website) |
| Zeitraum | 09.04.2019 (init v2.0.0) – 17.09.2019 (archiviert) |
| Commits | 49 |
| Status | Archiviert |

---

## TFD 1 — Oktober 2018

Erste Ausgabe des Thüringer Furdance. Die Website wurde ursprünglich von BlueIceWolf erstellt und ist im Repository als Archiv enthalten.

### Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Backend | PHP (Server-Side Includes für Header/Footer) |
| CSS | Bootstrap, Font Awesome, ET Line, Ionicons, Animate.css |
| JavaScript | jQuery, Owl Carousel, WOW.js, Parallax.js, Waypoints, Countdown-Timer |
| Build-Tool | Gulp (SASS-Kompilierung, CSS/JS-Minifizierung, Live Reload) |
| Analytics | Google Analytics (UA-121309874-1) |

### Features

- **Zweisprachig**: Deutsch und Englisch (Flaggen-Toggle, parallele PHP-Dateien)
- **Dynamische Elemente**: Bildkarussell, Countdown-Timer, animierte Counter, Parallax-Hintergründe, Scroll-Animationen
- **Google Maps**: Eingebettete Karte mit benutzerdefiniertem Styling
- **Responsive Design**: Bootstrap-Grid, mobil optimiert
- **SEO**: Sitemap mit 11 URLs, robots.txt mit Crawler-Blockierung

### Seiten

| Seite | Beschreibung |
|-------|-------------|
| Startseite | Homepage mit Slider-Karussell |
| Anfahrt | Wegbeschreibung zum Veranstaltungsort |
| Shuttle | Shuttle-Bus-Informationen |
| Kontakt | Kontaktseite |
| Teilnehmer | Teilnehmerliste |
| Impressum | Rechtliches und DSGVO |
| Registrierung | Link zum externen Registrierungssystem |

---

## TFD 2 — Oktober 2019

Zweite Ausgabe des Thüringer Furdance mit DDR-Motto (30. Jahrestag des Mauerfalls). Von Wachhund als rein statische Website neu erstellt.

### Tech-Stack

| Schicht | Technologie |
|---------|-------------|
| Typ | Reines statisches HTML (kein Backend, kein JavaScript) |
| CSS | Bootstrap Grid, eigenes Stylesheet (~500 Zeilen) |
| Schriften | Adobe Edge Web Fonts (Montserrat, Source Sans Pro) |

### Event-Details

| Detail | Wert |
|--------|------|
| Datum | 19. Oktober 2019, 18:00–03:00 |
| Ort | Bärsaal, Rudolstädter Str. 15, 99326 Stadtilm |
| Motto | DDR (30 Jahre Mauerfall) |
| Preise | Standard €25, Sponsor €30, Sponsor+ €40 |
| Übernachtung | Schlafplätze für ca. 60 Personen (Selbstversorgung) |
| Verpflegung | All-you-can-eat Buffet + Frühstück inklusive, Getränke kostenpflichtig |
| Shuttle | Berlin–Leipzig–Weimar–Stadtilm, €30 Hin- und Rückfahrt |
| Registrierung | Externes System unter `thueringer.fur.dance` |

### Seiten

| Seite | Beschreibung |
|-------|-------------|
| `index.html` | Hauptseite mit Event-Info, Preisen, Ablauf, Registrierung |
| `teilnehmer.html` | Teilnehmerliste |
| `impressum.html` | Impressum und Kontaktinformationen |
| `datenschutz.html` | Datenschutzerklärung (DSGVO-konform) |

### Design

- **Farbschema**: Grautöne (#838383, #18181c, #f0f2f6) mit rotem Akzent (#f50136)
- **Responsive**: Mobile-first mit Breakpoints bei 480px, 1024px
- **Layout**: Zentriert, max. 960px Breite
- **Preisboxen**: CSS-Hover-Effekte mit Schatten


---

## TFD 3

Eine dritte Ausgabe war geplant (Website: `thueringer-furdance.de`), hat aber nie stattgefunden. Kein Quellcode im Repository enthalten.
