# Luna User Journey - Web und App
## Figma Screen Map und Flow Design
**Version:** 1.0 | **Datum:** 2026-03-08 | **Status:** Draft

---

## 1. Uebersicht

Luna wird ueber zwei Kanaele vermarktet:

| Kanal | Modell | Bezahlung | Zielgruppe |
|-------|--------|-----------|------------|
| **A - Etisalat DCB** | B2B2C | Mobilfunkrechnung (DCB) | e& Kunden UAE |
| **B - Direkt** | B2C | Kreditkarte / Apple Pay / Google Pay | Weltweit |

---

## 2. Kanal A - Etisalat DCB (B2B2C)

### Flow: A1 bis A9

```
A1 Landing -> A2 Tier-Auswahl -> A3 MSISDN-Eingabe -> A4 OTP-Push
-> A5 OTP-Confirm -> A6 Bestaetigung -> A7 App-Download -> A8 Onboarding -> A9 Dashboard
```

### A1 - Landing Page (WebView in e& App)

- **Typ:** Responsive Web (iFrame/WebView im e& App)
- **Inhalt:**
  - Hero-Banner: Luna Logo + Tagline "Your Personal AI Assistant"
  - Feature-Highlights (3-4 Cards): Anrufe, Kalender, Tasks, Chat
  - CTA-Button: "Jetzt starten" / "Get Started"
- **Design:** dd.AI Toolkit 9.0.0 Header Template + Inner Page Hero Banner
- **Varianten:** EN, AR (RTL)

### A2 - Tier-Auswahl

- **Inhalt:**
  - 5 Pricing Cards (horizontal scrollbar auf Mobile):
    - Trial: 0 AED / 7 Tage
    - Basic: 100 AED/Monat
    - Plus: 200 AED/Monat
    - Pro: 350 AED/Monat
    - Assistant: 600 AED/Monat
  - Feature-Vergleichstabelle (expandable)
  - CTA pro Card: "Waehlen" / "Select"
- **Varianten:** EN, AR

### A3 - MSISDN-Eingabe

- **Inhalt:**
  - Textfeld: Mobilnummer (+971 prefix)
  - Hinweis: "Bezahlung ueber Ihre Etisalat-Rechnung"
  - Button: "OTP senden" / "Send OTP"
- **API-Call:** pushOTP an pt7.etisalat.ae/PushOTP
  - Parameter: serviceId, msisdn (AES-128-ECB encrypted), transactionId, languageId
  - Max 3 Pushes/Tag pro MSISDN
- **Varianten:** EN, AR

### A4 - OTP-Push (Warte-Screen)

- **Inhalt:**
  - Spinner/Animation
  - Text: "OTP wird an Ihr Handy gesendet..."
  - Timer: 5 Minuten Countdown
  - Link: "OTP erneut senden" (nach 60s)
- **Fehler-States:**
  - pushOTP fehlgeschlagen: Fehlermeldung + Retry
  - Max Versuche erreicht: "Bitte versuchen Sie es morgen erneut"
- **Varianten:** EN, AR

### A5 - OTP-Eingabe und Bestaetigung

- **Inhalt:**
  - 6-stelliges OTP-Eingabefeld (Auto-Focus, Numpad)
  - Button: "Bestaetigen" / "Confirm"
  - Link: "OTP erneut senden"
- **API-Call:** confirmOTP an pt7.etisalat.ae/confirmOTP
  - Parameter: serviceId, msisdn, otp, transactionId, languageId, amount
  - Max 5 Validierungen/Tag
- **Fehler-States:**
  - Falsches OTP: "Ungueltig, bitte erneut versuchen" (verbleibende Versuche)
  - Abgelaufen: "OTP abgelaufen" + Neues senden
- **Varianten:** EN, AR

### A6 - Abo-Bestaetigung

- **Inhalt:**
  - Erfolgs-Icon (Checkmark)
  - Abo-Details: Tier, Preis, naechste Abrechnung
  - "Wird ueber Ihre Etisalat-Rechnung abgebucht"
  - CTA: "App herunterladen" / "Download App"
  - Sekundaer: "Im Browser fortfahren"
- **API-Call:** Content Push via pt6.etisalat.ae/mlogin.aspx
  - Bestaetigungs-SMS an User
- **Varianten:** EN, AR

### A7 - App Download

- **Inhalt:**
  - App Store Badge (iOS) + Google Play Badge (Android)
  - QR-Code fuer Desktop
  - Deep Link fuer Mobile
  - "Oder im Browser fortfahren" Link
- **Varianten:** EN, AR

### A8 - Onboarding (In-App)

- **Inhalt:** 4 Screens (Swipeable)
  1. "Hallo, ich bin Luna" - Avatar-Intro
  2. "Ich manage Ihre Anrufe" - Telefon-Feature
  3. "Ich organisiere Ihren Tag" - Kalender + Tasks
  4. "Ich erledige Aufgaben fuer Sie" - Agent-Features
- **Abschluss:**
  - Berechtigungen anfragen (Mikrofon, Benachrichtigungen, Kalender)
  - Optional: Google/Microsoft Kalender verbinden
- **Varianten:** EN, AR

### A9 - Dashboard (Active User)

Siehe Abschnitt 4 (Shared App Screens)

---

## 3. Kanal B - Direkt (B2C)

### Flow: B1 bis B9

```
B1 Website -> B2 Tier-Auswahl -> B3 Account-Erstellung -> B4 Trial/Payment
-> B5 Onboarding -> B6 Dashboard -> B7 Upgrade -> B8 Payment -> B9 Active
```

### B1 - Marketing Website

- **Typ:** Responsive Web (luna-ai.com)
- **Inhalt:**
  - Hero: Luna Video/Animation + Tagline
  - Feature-Sektionen (Scroll)
  - Social Proof / Testimonials
  - Pricing-Tabelle
  - CTA: "Kostenlos testen" / "Start Free Trial"
- **Varianten:** EN, DE, AR

### B2 - Tier-Auswahl

- Identisch zu A2, aber mit Kreditkarten-Hinweis statt DCB
- Trial-Option prominent

### B3 - Account-Erstellung

- **Inhalt:**
  - Email + Passwort Formular
  - Oder: "Weiter mit Google" / "Weiter mit Apple"
  - AGB-Checkbox + Datenschutz-Link
  - Button: "Account erstellen"
- **Varianten:** EN, DE, AR

### B4 - Trial Start / Payment

- **Bei Trial:**
  - Sofortiger Zugang, keine Zahlungsdaten noetig
  - "7 Tage kostenlos, danach 100 AED/Monat"
  - Trial-Countdown im Dashboard sichtbar
- **Bei Paid:**
  - Stripe Checkout / Apple Pay / Google Pay
  - Rechnungsadresse
  - Bestaetigung
- **Varianten:** EN, DE, AR

### B5 - Onboarding

- Identisch zu A8

### B6 - Dashboard

Siehe Abschnitt 4

### B7 - Upgrade (In-App)

- **Inhalt:**
  - Aktueller Plan markiert
  - Feature-Vergleich
  - "Upgrade" Button pro hoeherer Tier
- **Varianten:** EN, DE, AR

### B8 - Payment (Upgrade)

- Stripe Checkout
- Bestaetigungs-Screen

### B9 - Active User

Siehe Abschnitt 4

---

## 4. Shared App Screens

### 4.1 Dashboard (Home)

- **Top Bar:**
  - Luna Logo (links)
  - Benachrichtigungs-Glocke (rechts)
  - Profil-Avatar (rechts)
- **Inhalt:**
  - Begruessung: "Hallo [Name], wie kann ich helfen?"
  - Quick Actions (horizontal): Anruf, Termin, Suche, Email
  - Letzte Aktivitaeten (Timeline):
    - Verpasster Anruf + Zusammenfassung
    - Gebuchter Termin
    - Erledigte Aufgabe
  - Luna Status Card: "Luna ist aktiv und hoert zu"
- **Bottom Navigation:**
  - Home | Anrufe | Chat | Tasks | Einstellungen

### 4.2 Anrufe-Tab

- **Inhalt:**
  - Anrufliste (chronologisch)
  - Pro Eintrag:
    - Anrufer-Name/Nummer
    - Datum/Uhrzeit
    - Dauer
    - Zusammenfassung (1-2 Zeilen)
    - Status-Badge: Angenommen / Verpasst / Weitergeleitet
  - Tap auf Eintrag -> Detail-Ansicht:
    - Volle Zusammenfassung
    - Audio-Player (wenn aufgezeichnet)
    - Aktionen: Zurueckrufen, Aufgabe erstellen

### 4.3 Chat-Tab

- **Inhalt:**
  - Chat-Interface (Messenger-Style)
  - Nachrichten-Bubbles: User (rechts), Luna (links)
  - Input-Leiste:
    - Textfeld
    - Mikrofon-Button (Voice-to-Text)
    - Anhaenge-Button (Dateien, Bilder)
  - Luna-Typing-Indicator
  - Beispiel-Prompts bei leerem Chat:
    - "Suche einen Flug Dubai nach Frankfurt am 4. April"
    - "Finde einen Zahnarzt-Termin fuer morgen"
    - "Zeige meine Termine fuer diese Woche"

### 4.4 Tasks-Tab

- **Inhalt:**
  - Task-Liste gruppiert nach Status:
    - In Bearbeitung (Luna arbeitet daran)
    - Warten auf Bestaetigung (User muss entscheiden)
    - Erledigt
  - Pro Task:
    - Titel + Kurzbeschreibung
    - Fortschritts-Indikator
    - Erstellt-Datum
    - Tap -> Detail mit Luna-Aktionsprotokoll

### 4.5 Einstellungen-Tab

- **Inhalt:**
  - Profil bearbeiten
  - Abo-Verwaltung (siehe Abschnitt 5)
  - Kalender-Integration (Google/Microsoft)
  - Benachrichtigungen
    - Anruf-Benachrichtigung: An/Aus
    - Dringende Anrufe: An/Aus + Schwellenwert
    - Live-Mithoeren: An/Aus
  - Luna-Verhalten
    - Sprache: DE/EN/AR
    - Stimme: Auswahl
    - Persoenlichkeit: Formell/Freundlich
  - Datenschutz und Sicherheit
  - Hilfe und Support

---

## 5. Abo-Verwaltung

### 5.1 Kanal A (DCB)

- **Abo-Status:** Aktiv/Pausiert/Gekuendigt
- **Aktueller Plan + Preis**
- **Naechste Abrechnung**
- **Plan aendern** (Upgrade/Downgrade)
- **Abo kuendigen:**
  - Bestaetigungs-Dialog
  - Kuendigung ueber CCI Deactivation API
  - Grace Period Info

### 5.2 CCI Integration (Etisalat Customer Care)

- **Inquiry API** (e& an Luna Backend):
  - Endpoint: POST XML
  - Request: serviceId, msisdn, languageId
  - Response: WebSubscription mit Status, Aktivierungs-/Ablaufdatum

- **Deactivation API** (e& an Luna Backend):
  - Endpoint: POST
  - Request: serviceId, msisdn, channel, transactionId, languageId
  - Response: Erfolgsstatus (Plain Text)
  - Luna muss Abo sofort deaktivieren

### 5.3 MSISDN Recycling

- **Taeglicher Job** (01:00 UAE Zeit):
  - Download Cease-List von pt1.etisalat.ae
  - Alle Abos fuer geceasete MSISDNs deaktivieren
  - Cleanup der User-Daten (5-Jahres-Aufbewahrung beachten)

---

## 6. Figma Screen-Liste

### Kanal A (Etisalat DCB)

| Nr | Screen-ID | Name | Varianten |
|----|-----------|------|-----------|
| 1 | A1-LAND | Landing Page | EN, AR |
| 2 | A2-TIER | Tier-Auswahl | EN, AR |
| 3 | A3-MSISDN | MSISDN-Eingabe | EN, AR |
| 4 | A4-OTP-WAIT | OTP Warte-Screen | EN, AR |
| 5 | A4-OTP-ERR | OTP Push Fehler | EN, AR |
| 6 | A4-OTP-MAX | Max Versuche | EN, AR |
| 7 | A5-OTP-INPUT | OTP Eingabe | EN, AR |
| 8 | A5-OTP-WRONG | OTP Falsch | EN, AR |
| 9 | A5-OTP-EXPIRED | OTP Abgelaufen | EN, AR |
| 10 | A6-CONFIRM | Abo-Bestaetigung | EN, AR |
| 11 | A7-DOWNLOAD | App Download | EN, AR |
| 12 | A8-ONBOARD-1 | Onboarding Intro | EN, AR |
| 13 | A8-ONBOARD-2 | Onboarding Anrufe | EN, AR |
| 14 | A8-ONBOARD-3 | Onboarding Kalender | EN, AR |
| 15 | A8-ONBOARD-4 | Onboarding Agent | EN, AR |
| 16 | A8-PERMS | Berechtigungen | EN, AR |

### Kanal B (Direkt)

| Nr | Screen-ID | Name | Varianten |
|----|-----------|------|-----------|
| 17 | B1-WEB | Marketing Website | EN, DE, AR |
| 18 | B2-TIER | Tier-Auswahl | EN, DE, AR |
| 19 | B3-SIGNUP | Account-Erstellung | EN, DE, AR |
| 20 | B3-SIGNUP-SSO | SSO Login | EN, DE, AR |
| 21 | B4-TRIAL | Trial Start | EN, DE, AR |
| 22 | B4-PAY | Payment (Stripe) | EN, DE, AR |
| 23 | B7-UPGRADE | Upgrade-Ansicht | EN, DE, AR |
| 24 | B8-PAY-UP | Payment Upgrade | EN, DE, AR |

### Shared App

| Nr | Screen-ID | Name | Varianten |
|----|-----------|------|-----------|
| 25 | S-DASH | Dashboard | EN, DE, AR |
| 26 | S-CALLS | Anrufe-Liste | EN, DE, AR |
| 27 | S-CALL-DET | Anruf-Detail | EN, DE, AR |
| 28 | S-CHAT | Chat (leer) | EN, DE, AR |
| 29 | S-CHAT-CONV | Chat (Konversation) | EN, DE, AR |
| 30 | S-CHAT-TASK | Chat: Task erstellt | EN, DE, AR |
| 31 | S-TASKS | Tasks-Liste | EN, DE, AR |
| 32 | S-TASK-DET | Task-Detail | EN, DE, AR |
| 33 | S-SETTINGS | Einstellungen | EN, DE, AR |
| 34 | S-PROFILE | Profil bearbeiten | EN, DE, AR |
| 35 | S-ABO | Abo-Verwaltung | EN, DE, AR |
| 36 | S-ABO-CANCEL | Kuendigung | EN, DE, AR |
| 37 | S-NOTIF | Benachrichtigungen | EN, DE, AR |
| 38 | S-LUNA-SET | Luna-Verhalten | EN, DE, AR |
| 39 | S-KALENDER | Kalender-Integration | EN, DE, AR |

### Zusammenfassung

- **Unique Screens:** 39
- **Mit Varianten (EN/DE/AR):** ca. 101 Screens
- **Kanal A (DCB-spezifisch):** 16 Screens x 2 (EN/AR) = 32
- **Kanal B (Direkt-spezifisch):** 8 Screens x 3 (EN/DE/AR) = 24
- **Shared:** 15 Screens x 3 (EN/DE/AR) = 45
- **Gesamt:** ca. 101 Screen-Varianten

---

## 7. Design Guidelines

### Farbpalette

| Rolle | Farbe | Hex |
|-------|-------|-----|
| Primary (Dark) | Deep Navy | #1A1B4B |
| Secondary | Luna Purple | #7C6FE0 |
| Accent | Warm Gold | #F5B731 |
| Success | Mint Green | #2ECC71 |
| Error | Coral Red | #E74C3C |
| Warning | Amber | #F39C12 |
| Background (Light) | Off-White | #F8F9FA |
| Background (Dark) | Charcoal | #1E1E2E |
| Text Primary | Dark Gray | #2D3436 |
| Text Secondary | Medium Gray | #636E72 |

### Typografie

| Element | Font | Groesse | Gewicht |
|---------|------|---------|--------|
| H1 (Hero) | Inter | 32-40px | Bold |
| H2 (Section) | Inter | 24-28px | Semibold |
| H3 (Subsection) | Inter | 18-20px | Semibold |
| Body | Inter | 14-16px | Regular |
| Caption | Inter | 12px | Regular |
| Button | Inter | 14-16px | Semibold |

### Spacing und Layout

- **Grid:** 8px Basis-Grid
- **Margins:** 16px (Mobile), 24px (Tablet), 32px (Desktop)
- **Card-Radius:** 12px
- **Button-Radius:** 8px
- **Schatten:** 0 2px 8px rgba(0,0,0,0.08)

### RTL-Support (Arabisch)

- Alle Layouts spiegeln (Links/Rechts tauschen)
- Arabische Typografie: Noto Sans Arabic
- Icons bleiben unveraendert (ausser Pfeile)
- Zahlen: Westliche Ziffern (0-9)

### e& Design System Kompatibilitaet

- Landing Page (A1) nutzt dd.AI Toolkit 9.0.0 Komponenten
- Header Template fuer Etisalat-Branding
- Bottom Tray fuer Aktionen
- Action Sheet fuer Bestaetigungen
- Statessheet fuer Fehler/Erfolg

---

## 8. Technische Notizen fuer Figma

### Komponenten-Bibliothek

1. **Atoms:** Buttons, Inputs, Badges, Icons, Avatars
2. **Molecules:** Cards, List Items, Chat Bubbles, OTP Input
3. **Organisms:** Navigation Bar, Header, Pricing Table, Chat Interface
4. **Templates:** Dashboard Layout, Onboarding Layout, WebView Layout

### Auto-Layout Regeln

- Alle Screens: Auto-Layout mit Fill Container
- Bottom Navigation: Fixed Bottom
- Top Bar: Fixed Top (Sticky)
- Chat Input: Fixed Bottom mit Keyboard-Offset
- Scrollable Content: Vertical Auto-Layout

### Prototyping Flows

1. **Flow A (DCB):** A1 > A2 > A3 > A4 > A5 > A6 > A7 > A8 > A9 (Dashboard)
2. **Flow B (Direkt):** B1 > B2 > B3 > B4 > B5 > B6 (Dashboard)
3. **Flow Chat:** Dashboard > Chat > Task erstellt > Task Detail
4. **Flow Anruf:** Dashboard > Anrufe > Anruf Detail
5. **Flow Settings:** Dashboard > Einstellungen > Abo > Kuendigung

---

## 9. Naechste Schritte

1. [ ] Figma-Projekt erstellen: "Luna - Web und App"
2. [ ] Design System anlegen (Farben, Typo, Spacing, Komponenten)
3. [ ] Kanal A Screens designen (A1-A8, 16 Screens)
4. [ ] Kanal B Screens designen (B1-B8, 8 Screens)
5. [ ] Shared App Screens designen (S-DASH bis S-KALENDER, 15 Screens)
6. [ ] AR-Varianten erstellen (RTL Mirror)
7. [ ] Prototyping Flows verlinken
8. [ ] Review mit Etisalat (dd.AI Toolkit Compliance)
9. [ ] Developer Handoff vorbereiten
