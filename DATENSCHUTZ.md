# Datenschutz-Angaben — Dashboard

Stand: 2026-06-30. Quelle: Code-Review der App.

---

## 1. App-Privacy „Nutrition Label" (App Store Connect → App-Datenschutz)

> Einzureichen **auf der Website** appstoreconnect.apple.com, NICHT in Xcode.
> Pfad: Meine Apps → Dashboard → „App-Datenschutz" → „Erste Schritte".

**Frage „Erfasst du oder dein Partner Daten?"** → **Ja** (wegen Standort).

### Erfasster Datentyp: Standort → Grober Standort
- **Zweck:** App-Funktionalität (Wetteranzeige)
- **Mit Identität verknüpft:** Nein
- **Zur Verfolgung (Tracking) genutzt:** Nein

### Alles andere → NICHT deklarieren, weil es das Gerät nicht verlässt bzw. keine Personendaten sind:
| Datenfluss | Warum nicht im Label |
|---|---|
| Kalender (EventKit) | bleibt lokal, wird nicht übertragen |
| Todos / Favoriten / Einstellungen | nur lokal (SwiftData/UserDefaults) |
| Sender-UUID → radio-browser (`trackClick`) | Inhalts-Identifier, keine Personendaten |
| RSS-Feed-Abrufe | nur HTTP-Request, keine erfassten Nutzerdaten |
| Feiertags-API (Jahr) | keine Personendaten |
| System-/Netzwerk-Statistik | nur lokal angezeigt |

**Tracking:** Keines. **Konten/Login:** Keine.

---

## 2. Privacy Manifest (`PrivacyInfo.xcprivacy`) — bereits in Xcode angelegt

Liegt unter `dashboard/dashboard/PrivacyInfo.xcprivacy`, wird automatisch ins Bundle
aufgenommen. Inhalt: kein Tracking, grober Standort (App-Funktion), UserDefaults (CA92.1).
**Muss mit den Label-Antworten oben übereinstimmen** — tut es.

---

## 3. Usage-Description-Strings — bereits in Xcode (Build-Settings)

- `NSLocationWhenInUseUsageDescription` ✅
- `NSCalendarsFullAccessUsageDescription` ✅

---

## 4. Datenschutzerklärung (URL ist im App Store Connect Pflicht)

> Text irgendwo öffentlich hosten (z. B. eigene Domain) und die URL in
> App Store Connect → App-Informationen → „Datenschutzrichtlinie-URL" eintragen.

---

### Datenschutzerklärung für „Dashboard"

**Verantwortlich:** Alexander Keterling, <Kontakt-E-Mail eintragen>

**Grundsatz.** „Dashboard" ist eine lokale macOS-App. Es gibt keine Benutzerkonten,
keine Registrierung und kein Tracking. Wir betreiben keinen eigenen Server und
sammeln keine personenbezogenen Daten auf eigenen Systemen.

**Standort.** Wenn du das Wetter-Widget nutzt, fragt die App einmalig deinen groben
Standort ab und übermittelt ihn an **Apple WeatherKit** (Wetterdaten) sowie an Apples
Geocoding-Dienst (Ortsname). Es gilt Apples Datenschutzrichtlinie. Der Standort wird
nicht von uns gespeichert. Die Berechtigung kannst du in den Systemeinstellungen
jederzeit widerrufen.

**Kalender.** Das Kalender-Widget liest mit deiner Erlaubnis anstehende Termine über
das System-Framework EventKit. Diese Daten bleiben auf deinem Gerät und werden nicht
übertragen.

**Internetradio.** Beim Abspielen eines Senders werden Senderlisten und Streams über
den Dienst **radio-browser.info** geladen; dabei wird die ID des angeklickten Senders
zur Klickzählung an radio-browser übermittelt. Es werden keine personenbezogenen Daten
gesendet. Sender-Logos und die Audio-Streams werden direkt von den Servern des
jeweiligen Senders geladen; diese sehen dabei die übliche Verbindungsinformation
(z. B. IP-Adresse).

**Aktiencharts (TradingView).** Das Trading-Widget bindet die interaktiven Charts von
**TradingView** ein. Dazu lädt ein eingebetteter Webview Inhalte direkt von den Servern
von TradingView (u. a. `s3.tradingview.com`, `*.tradingview.com`, inkl. WebSocket-
Verbindung). Dabei können TradingView Verbindungsdaten (z. B. IP-Adresse) und ggf.
Cookies innerhalb des Webviews verarbeiten; es gilt die Datenschutzrichtlinie von
TradingView (https://www.tradingview.com/privacy-policy/). Wir übermitteln keine
personenbezogenen Daten an TradingView; geladen werden nur das von dir gewählte
Tickersymbol und der Zeitraum.

**Nachrichten-Feeds.** Die App lädt RSS-/Atom-Feeds direkt von den von dir gewählten
Quellen (z. B. BSI/CERT-Bund). Dabei sieht der jeweilige Feed-Server die technisch
übliche Verbindungsinformation (z. B. IP-Adresse).

**Feiertage.** Das Kalender-Widget ruft Feiertage über die öffentliche API
api-feiertage.de ab (nur das Jahr wird übermittelt, keine Personendaten).

**Lokale Speicherung.** Aufgaben, Radio-Favoriten und App-Einstellungen werden
ausschließlich lokal auf deinem Gerät gespeichert (SwiftData / UserDefaults).

**Benachrichtigungen.** Erinnerungen werden als lokale Mitteilungen auf deinem Gerät
geplant; es werden keine Daten an Push-Server gesendet.

**Kontakt.** Bei Fragen zum Datenschutz: <Kontakt-E-Mail eintragen>.

### Übersicht der genutzten Drittanbieter-Dienste und Domains

| Funktion | Anbieter | Domain(s) / Endpunkt | Übermittelte Daten |
|---|---|---|---|
| Wetter | Apple WeatherKit | (System-Framework, Apple-Server) | grober Standort |
| Ortsname | Apple Geocoding | (System-Framework, Apple-Server) | grober Standort |
| Radio-Senderliste | radio-browser.info | `de1.api.radio-browser.info`, `all.api.radio-browser.info` | Suchbegriff, Ländercode, Sender-ID (Klickzählung) |
| Radio-Streams & -Logos | jeweiliger Sender | sender-individuelle Server | nur Verbindungsdaten (IP) |
| Aktiencharts | TradingView | `s3.tradingview.com`, `*.tradingview.com` (HTTPS + WSS) | Tickersymbol, Zeitraum; Verbindungsdaten/Cookies im Webview |
| Nachrichten-Feeds | von dir gewählte Quellen (z. B. CERT-Bund) | `wid.cert-bund.de` u. a. | nur Verbindungsdaten (IP) |
| Feiertage | api-feiertage.de | `get.api-feiertage.de` | nur das Jahr |

*Letzte Aktualisierung: <Datum eintragen>*
