# Luna Personal Assistant — Design-Dokument

**Datum:** 7. März 2026
**Projekt:** Amira.Luna — Personal AI Assistant
**Unternehmen:** AC AI Convergence LLC (Amira AI)
**Version:** 1.0

---

## 1. Vision & Zielsetzung

Luna ist heute ein intelligenter Telefon-Assistent, der Anrufe entgegennimmt, zusammenfasst, Termine bucht und den Besitzer bei wichtigen Anrufen verbindet (Whisper Mode). Das Ziel ist es, Luna zu einem **vollwertigen autonomen Personal Assistant** zu erweitern, der eigenständig Aufgaben in der realen Welt erledigt — von Flugbuchungen über Arzttermine bis hin zu Produktrecherchen.

### Kernprinzip

> Luna soll sich anfühlen wie ein echter persönlicher Assistent, der mit der Zeit immer besser wird, seinen Besitzer immer besser kennt und zunehmend autonom handeln kann.

### Zielgruppe

- **Phase 1:** B2C — Endverbraucher kaufen Luna als persönlichen Assistenten (Abo-Modell)
- **Phase 2:** B2B2C — Partner wie Etisalat vertreiben Luna in ihrem Shop an ihre Kunden

### Skalierung

- Geplant für **400.000 bis 4 Millionen Nutzer**
- Hosting auf AWS, bestehende Komponenten (LiveKit, Workflow-Engine) lokal gehostet

---

## 2. Bestehende Funktionen (Ist-Zustand)

| Funktion | Beschreibung |
|----------|-------------|
| Anrufannahme | Luna nimmt Anrufe professionell entgegen |
| Gespraechszusammenfassung | Zusammenfassung des Anliegens in Sekunden |
| Benachrichtigung | Zustellung via E-Mail, SMS oder WhatsApp |
| Kalender-Integration | Google/Microsoft Kalender anbinden, Termine buchen |
| Whisper Mode | Luna ruft den Besitzer an, waehrend der Anrufer wartet — Besitzer entscheidet ob verbinden oder nicht |
| Live-Reinhoeren | Besitzer kann in laufende Gespraeche reinhoeren |

### Bestehendes Pricing

| Tier | Preis | Inklusiv-Minuten | Kern-Features |
|------|-------|-------------------|---------------|
| Trial | kostenlos | 3 Calls | Basic-Features, E-Mail-Zustellung |
| Basic | 100 AED/Mo | 30 Min | Call Handling, Notizen, E-Mail/WhatsApp |
| Plus | 200 AED/Mo | 60 Min | + Kalender, Scheduling, Bestaetigungen |
| Pro | 350 AED/Mo | 120 Min | + Whisper Mode, Live Connection, Priority Support |

**Wichtig:** Keine offene Minutenabrechnung. Bei Verbrauch des Kontingents bucht der Kunde ein Erweiterungspaket.

---

## 3. Neues Tier: Luna Assistant

### Pricing-Struktur

| | Trial | Basic | Plus | Pro | **Assistant** |
|---|---|---|---|---|---|
| **Preis** | kostenlos | 100 AED | 200 AED | 350 AED | **~600 AED** |
| Calls annehmen | 3 Calls | 30 Min | 60 Min | 120 Min | 120 Min |
| Minuten-Erweiterung | — | +30 Min Paket | +30 Min Paket | +60 Min Paket | +60 Min Paket |
| Kalender | — | — | ja | ja | ja |
| Whisper Mode | — | — | — | ja | ja |
| **Chat mit Luna** | — | — | — | — | **ja** |
| **Luna E-Mail-Adresse** | — | — | — | — | **ja** |
| **Autonome Tasks** | — | — | — | — | **30 Tasks/Mo** |
| **Task-Erweiterung** | — | — | — | — | **+15 Tasks Paket** |
| **Web-Recherche** | — | — | — | — | **ja** |
| **Luna ruft an (Outbound)** | — | — | — | — | **ja** |
| **Browser-Aktionen** | — | — | — | — | **ja** |
| **Tiefes User-Profil** | — | — | — | — | **ja** |

**Tagline:** *"Never miss an opportunity"*

**Abrechnungsprinzip:** Kontingent aufgebraucht → Luna informiert den User → User bucht Erweiterungspaket in der App → sofort weiter. Kein automatisches Nachladen, keine offene Abrechnung.

---

## 4. Neue Faehigkeiten des Assistant-Tiers

### 4.1 Aufgaben-Kategorien

| Kategorie | Beispiele |
|-----------|----------|
| Recherche | Web-Suche, Preisvergleiche, Angebote finden |
| Termine buchen | Zahnarzt, Friseur, Restaurant (per Telefon/E-Mail/Web) |
| Reisen organisieren | Fluege, Hotels, Mietwagen suchen und buchen |
| E-Mail-Management | Lesen, sortieren, beantworten im Namen des Users |
| Einkauefe/Bestellungen | Produkte suchen und bestellen |
| Erinnerungen & Follow-ups | Proaktiv erinnern, offene Aufgaben nachverfolgen |
| Dokumente erstellen | Briefe, Zusammenfassungen, Formulare |

### 4.2 Kommunikationskanaele

Luna kann folgende Kanaele **aktiv nutzen**, um Aufgaben zu erledigen:

- **E-Mail (Lunas eigene Adresse)** — z.B. luna-rainer@luna.ai, sendet/empfaengt in Lunas Namen
- **E-Mail (User-Postfach)** — Liest, sortiert und beantwortet E-Mails im verbundenen Gmail/Outlook des Users (siehe 4.5)
- **Telefon (Outbound)** — Luna ruft selbst an (z.B. Zahnarztpraxis, Restaurant)
- **Web-Suche** — Recherche im Internet
- **SMS** — Fuer einfache Kommunikation
- **Browser-Automatisierung** — Webseiten bedienen wo keine API verfuegbar ist

### 4.3 Interaktion User ↔ Luna

- **Chat in der App** — Hauptkanal, User tippt Auftraege
- **Diktierfunktion** — Spracheingabe als Diktiergeraet (kein Telefonat an Luna)
- **E-Mail an Luna** — User schreibt an Lunas E-Mail-Adresse

### 4.5 Optionale Account-Verbindung (Gmail / Microsoft)

Der User kann seinen **Google-Account** und/oder **Microsoft-Account** optional mit Luna verbinden. Dies ist ein zentraler Baustein, damit Luna als vollwertiger Assistent agieren kann.

**Unterstuetzte Provider:**

| Provider | Protokoll | E-Mail | Kalender | Kontakte |
|----------|-----------|--------|----------|----------|
| Google | OAuth 2.0 (Google Identity) | Gmail API | Google Calendar API | People API |
| Microsoft | OAuth 2.0 (MS Identity Platform) | Graph API (Mail) | Graph API (Calendar) | Graph API (Contacts) |

**Warum OAuth 2.0 statt IMAP/SMTP fuer User-Postfaecher:**
- Kein Passwort-Speichern noetig — Token-basiert, jederzeit widerrufbar
- Granulare Berechtigungen — User gibt nur frei was er will
- Google und Microsoft erfordern OAuth fuer Drittanbieter-Zugriff
- IMAP/SMTP bleibt nur relevant fuer **Lunas eigene E-Mail-Adresse** (luna-rainer@luna.ai)

**Stufenmodell — User schaltet Bereiche einzeln frei:**

```
Stufe 1 (Basis):     Kalender lesen + Kontakte lesen
Stufe 2 (Kalender):  + Termine buchen (Kalender schreiben)
Stufe 3 (E-Mail):    + E-Mails lesen
Stufe 4 (Voll):      + E-Mails senden + E-Mails verwalten
```

Jede Stufe erfordert explizites Opt-In. Luna erklaert vor jeder Erweiterung, wozu sie den Zugriff braucht.

**Einstiegspunkte fuer den Connect:**
1. Onboarding (Schritt 4): "Moechtest du deinen Account verbinden?"
2. Einstellungen → Verknuepfte Accounts
3. Chat mit Luna — Luna schlaegt es kontextbasiert vor (z.B. "Soll ich deinen Kalender pruefen? Dafuer braeuchte ich Zugriff.")

**Abgrenzung: Lunas E-Mail vs. User-Postfach:**

| | Lunas E-Mail (luna-rainer@luna.ai) | User-Postfach (Gmail/Outlook) |
|---|---|---|
| Zweck | Luna sendet/empfaengt in eigenem Namen | Luna liest/verwaltet User-Mails |
| Technik | IMAP/SMTP auf eigener Domain | OAuth + Gmail/Graph API |
| Sichtbar fuer Empfaenger | "Von: Luna" | "Von: [Username]" |
| Verfuegbar | Assistant-Tier | Plus+ fuer Kalender, Assistant fuer E-Mail |

**Implementierungs-Reihenfolge:**

| Phase | Was |
|-------|-----|
| Phase 1 | Google/Microsoft Kalender (readonly) |
| Phase 2 | Kalender schreiben (Termine buchen) |
| Phase 3 | Kontakte lesen (Personenerkennung) |
| Phase 4 | E-Mail lesen (Zusammenfassungen) |
| Phase 5 | E-Mail senden (im Namen des Users) — erfordert hoeheres Trust-Level |
| Phase 6 | E-Mail verwalten (Sortieren, Archivieren) |

### 4.4 Benachrichtigungen

- **Push-Notification** — immer als Standard
- **Plus User-Wahl** — SMS, E-Mail oder WhatsApp zusaetzlich
- Luna berichtet ueber Fortschritt und Ergebnisse ueber beide Kanaele

---

## 5. Architektur-Uebersicht

### 5.1 Ansatz: Micro-Agent mit Workflow-Hybrid

Kombination aus:
- **Micro-Agent-Architektur** — Spezialisierte, unabhaengig skalierbare Agents fuer jede Aufgabenart
- **Workflow-Engine** — Optimierte Workflows fuer haeufige Standard-Tasks

Inspiriert von:
- **OpenClaw** — Skill-System, Heartbeat-Mechanismus, ReAct-Loop
- **LangGraph** — Graph-basierte Orchestrierung, State-Management, Checkpoints

### 5.2 Gesamtarchitektur

```
+-----------------------------------------------------------+
|                    USER TOUCHPOINTS                        |
|   App (Chat/Diktat)  |  E-Mail an Luna  |  Push/SMS/WA   |
+----------------------------+------------------------------+
                             |
                             v
+-----------------------------------------------------------+
|                   API GATEWAY (AWS)                        |
|          Auth | Rate Limiting | Tier-Enforcement           |
+----------------------------+------------------------------+
                             |
                             v
+-----------------------------------------------------------+
|              LUNA ORCHESTRATOR (pro User-Session)          |
|                                                           |
|  +---------------+  +----------------+  +---------------+ |
|  | Intent        |  | User Memory    |  | Task Queue    | |
|  | Parser        |  | & Profil       |  | & Prioritizer | |
|  +---------------+  +----------------+  +---------------+ |
|                                                           |
|  +-----------------------------------------------------+ |
|  |         Autonomie-Manager                            | |
|  |  (entscheidet: selbst handeln oder rueckfragen?)     | |
|  +-----------------------------------------------------+ |
+----------------------------+------------------------------+
                             |  delegiert an
                             v
+-----------------------------------------------------------+
|                    MICRO-AGENTS                            |
|                                                           |
|  Research Agent  | Travel Agent    | Email Agent          |
|  Calendar Agent  | Phone Agent     | Commerce Agent       |
|  Booking Agent   | Document Agent  | Reminder Agent       |
|                                                           |
+----------------------------+------------------------------+
                             |
                             v
+-----------------------------------------------------------+
|                      TOOL LAYER                            |
|                                                           |
|  Web Search API | Browser (Playwright) | Phone (LiveKit)  |
|  Email: IMAP/SMTP (Lunas Adresse) + OAuth APIs (User-Postfach) |
|  Google/Microsoft OAuth | Calendar API | File Generation   |
+-----------------------------------------------------------+
                             |
                             v
+-----------------------------------------------------------+
|                 BESTEHENDE INFRASTRUKTUR                   |
|  LiveKit | Workflow Engine | Amira Voice AI                |
|  Google/Microsoft Calendar API | AWS Services              |
+-----------------------------------------------------------+
```

### 5.3 Orchestrator — Das Gehirn von Luna

Der Orchestrator verarbeitet jede User-Anfrage in 4 Schritten:

**Schritt 1: Intent Parsing (Gemini Flash)**
- Klassifiziert die Anfrage (TRAVEL, BOOKING, RESEARCH, EMAIL etc.)
- Extrahiert Entities (Datum, Ort, Praeferenzen)
- Schaetzt Komplexitaet (LOW, MEDIUM, HIGH)

**Schritt 2: Memory Lookup (Vector DB)**
- Sucht relevante Praeferenzen und vergangene Aufgaben
- Reichert die Anfrage mit User-Kontext an

**Schritt 3: Autonomie-Check**
- Prueft Trust-Level des Users
- Entscheidet: autonom handeln oder rueckfragen

**Schritt 4: Task-Graph erstellen (LangGraph)**
- Zerlegt die Aufgabe in Schritte
- Delegiert an den passenden Micro-Agent
- Speichert State nach jedem Schritt (Checkpoint)
- Bei Absturz: Wiederaufnahme ab letztem Checkpoint

### 5.4 Micro-Agents

Jeder Agent ist ein eigenstaendiger, skalierbarer Service:

| Agent | Aufgaben | Tools |
|-------|----------|-------|
| Research Agent | Web-Suche, Preisvergleiche, Marktrecherche | Web Search API, Browser |
| Travel Agent | Fluege, Hotels, Mietwagen suchen/buchen | Reise-APIs, Browser-Fallback |
| Calendar Agent | Termine pruefen, buchen, Konflikte erkennen | Google/MS Calendar API |
| Phone Agent | Outbound-Anrufe taetigen, IVR navigieren | LiveKit/SIP |
| Email Agent | E-Mails lesen, senden, Inbox verwalten | IMAP/SMTP |
| Commerce Agent | Produkte suchen, Preise vergleichen, bestellen | Web Search, Browser |
| Booking Agent | Termine bei Dienstleistern buchen | Browser, Phone, Email |
| Document Agent | Briefe, Zusammenfassungen, PDFs erstellen | File Generation |
| Reminder Agent | Proaktive Erinnerungen, Follow-ups, Deadlines | Heartbeat-System |

**Hybrid-Ansatz fuer externe Interaktion:**
- **API-first** — Direkte APIs nutzen wo verfuegbar (Google Flights, Booking.com etc.)
- **Browser-Automatisierung** — Playwright als Fallback fuer Webseiten ohne API
- **Telefon + E-Mail** — Fuer Dienste ohne beides (z.B. lokaler Zahnarzt)

### 5.5 Reminder Agent & Heartbeat (inspiriert von OpenClaw)

Der Reminder Agent laeuft periodisch im Hintergrund und prueft:
- Offene Tasks mit Deadline
- Kalender-Erinnerungen
- Unbeantwortete E-Mails
- Follow-ups auf laufende Vorgaenge

Er kann proaktiv den User benachrichtigen, ohne dass dieser eine Anfrage stellt.

---

## 6. User Memory & Profil-System

### 6.1 Drei Wissensquellen

**Explizites Wissen (User gibt es ein)**
- Onboarding: Name, Sprache, Standort, Praeferenzen
- Einstellungen: "Ich fliege immer Business", "Termine nur nachmittags"
- Verknuepfte Dienste: Google/Microsoft Account (OAuth 2.0) fuer Kalender, E-Mail-Postfach, Kontakte — stufenweise freigeschaltet (siehe 4.5)

**Implizites Wissen (Luna lernt aus Interaktionen)**
- Jede Aufgabe wird ausgewertet: Was hat der User bestaetigt, abgelehnt, geaendert?
- Beispiel: User korrigiert Flugvorschlag von Economy auf Business → Luna merkt sich Praeferenz
- Beispiel: User lehnt Zahnarzt ab weil zu weit weg → Luna lernt den bevorzugten Radius

**Vernetztes Wissen (aus verknuepften Daten)**
- Kalender-Analyse: Wiederkehrende Termine erkennen und respektieren
- E-Mail-Analyse: Haeufige Kontakte, laufende Vorgaenge
- Anruf-Historie: Wiederkehrende Anrufer und Themen

### 6.2 Speicherarchitektur

| Speicher | Technologie | Inhalt |
|----------|-------------|--------|
| Structured Profile | PostgreSQL (RDS Aurora) | Name, Praeferenzen, Einstellungen, Tier, Kontingente |
| Semantic Memory | Vector DB (Amazon OpenSearch Serverless) | Vergangene Aufgaben, gelernte Praeferenzen, Kontext — durchsuchbar per Similarity |
| Knowledge Graph (Phase 2) | Neptune oder Neo4j | Beziehungen: "User kennt Person X" → "Person X arbeitet bei Firma Y" |

### 6.3 Stufenweise Autonomie

Luna wird mit der Zeit autonomer, basierend auf einem Trust-Level-System:

```
Trust-Level: 0-100 (startet bei 20)

Level  0-30:  Luna fragt IMMER vor jeder Aktion
Level 30-60:  Luna handelt bei bekannten Mustern autonom
              (z.B. "gleicher Flug wie letztes Mal")
Level 60-80:  Luna handelt autonom bis zu einem Kostenlimit
Level 80+:    Luna handelt fast komplett autonom,
              fragt nur bei Erstmaligem oder hohen Betraegen

Punkte-System:
  User bestaetigt Lunas Vorschlag:    +2
  Task erfolgreich abgeschlossen:     +1
  User lehnt ab oder korrigiert:      -3
  Task fehlgeschlagen:                -5
```

---

## 7. LLM-Strategie: Multi-Model mit Claude & Gemini

Kein OpenAI. Ausschliesslich Anthropic Claude und Google Gemini.

### 7.1 Modell-Routing

| Aufgabe | Modell | Begruendung |
|---------|--------|-------------|
| Intent-Erkennung | Gemini Flash | Extrem schnell, sehr guenstig, ideal fuer Klassifikation |
| Orchestrierung | Claude Sonnet | Beste Reasoning-Qualitaet fuer Planung und Delegation |
| Komplexe Entscheidungen | Claude Opus | Staerkstes Modell fuer kritische Aufgaben |
| E-Mail/Zusammenfassungen | Gemini Flash / Claude Haiku | Guenstig, schnell, gute Qualitaet |
| Browser-Automatisierung | Claude Sonnet (Computer Use) | Ausgereifteste Loesung fuer Web-Interaktion |
| Profil-Updates extrahieren | Gemini Flash | Batch-Job, muss guenstig sein |
| Multilingual (AR/EN/DE+) | Gemini Pro / Claude Sonnet | Beide stark multilingual |

### 7.2 Routing-Logik

```
User-Nachricht eingehend
  -> Gemini Flash: Intent klassifizieren + Komplexitaet schaetzen
  -> Einfach? -> Gemini Flash/Claude Haiku erledigt direkt
  -> Komplex? -> Claude Sonnet plant, delegiert an Micro-Agent
  -> Micro-Agent braucht Entscheidung? -> Claude Opus fuer kritische Schritte
```

### 7.3 Vorteile des Multi-Model-Ansatzes

- **Kostenoptimierung**: Einfache Tasks mit guenstigen Modellen, teure nur wenn noetig
- **Kein Vendor-Lock-in**: Wenn ein Anbieter Preise aendert oder ausfaellt, kann der andere uebernehmen
- **Staerken nutzen**: Gemini Flash fuer Speed, Claude fuer Reasoning und Tool-Use

### 7.4 Sprachen

Luna erkennt automatisch die Sprache des Users und antwortet entsprechend. Keine Einschraenkung auf bestimmte Sprachen — beide LLM-Anbieter unterstuetzen breite Mehrsprachigkeit.

---

## 8. Skalierung & Infrastruktur

### 8.1 Annahmen

- 400.000 bis 4.000.000 Nutzer
- Ca. 5-10% gleichzeitig aktiv
- 400k User → 20k-40k concurrent Sessions
- 4M User → 200k-400k concurrent Sessions

### 8.2 AWS-Architektur

| Komponente | AWS Service | Begruendung |
|------------|-------------|-------------|
| Orchestrator | ECS Fargate | Pro Session, auto-skalierend |
| Micro-Agents (kurze Tasks) | Lambda | Serverless, zahlt nur bei Nutzung |
| Micro-Agents (lange Tasks) | ECS Fargate | Browser-Automatisierung, Telefonate |
| Vector DB | OpenSearch Serverless | Skaliert automatisch, kein Cluster-Management |
| Task Queue | SQS + Step Functions | Zuverlaessig, skalierbar |
| State Store | DynamoDB | Schnell, skaliert unbegrenzt |
| User Profiles | RDS Aurora (PostgreSQL) | Relationale Daten, bewaehrt |
| Browser Pool | ECS mit Playwright-Containern | Pool von 500-2000 Instanzen, shared |
| Phone | LiveKit Cloud / SIP Trunk | Bestehende Infrastruktur |
| API Gateway | Amazon API Gateway | Auth, Rate Limiting, Tier-Enforcement |

### 8.3 Kostenschaetzung pro User

```
Durchschnittliche AI-Kosten pro Task:
  Einfache Recherche:   ~$0.03 (~10k Tokens)
  Komplexe Buchung:     ~$0.15 (~50k Tokens)
  Browser-Automation:   ~$0.30 (~100k Tokens)
  Phone-Call:           ~$0.05 + Telco-Kosten

Bei 30 Tasks/User/Monat, Durchschnitt ~$0.10/Task:
  AI-Kosten:    ~$3/User/Monat
  Infra-Kosten: ~$1/User/Monat
  Gesamt:       ~$4/User/Monat

Bei 600 AED (~$163) Abo-Preis → sehr gesunde Marge
```

---

## 9. Technologie-Stack (Zusammenfassung)

| Bereich | Technologie |
|---------|-------------|
| Orchestrierung | LangGraph (Python) |
| LLMs | Anthropic Claude (Sonnet, Opus, Haiku) + Google Gemini (Flash, Pro) |
| Voice AI | LiveKit + Amira (besteht bereits) |
| Browser-Automatisierung | Playwright |
| Workflow-Engine | Bestehende Engine (fuer Standard-Tasks) |
| Datenbank | Aurora PostgreSQL, DynamoDB, OpenSearch |
| Queue/State | SQS, Step Functions, DynamoDB |
| Hosting | AWS (ECS Fargate, Lambda) |
| App | Tech-Stack wird separat entschieden |

---

## 10. Offene Punkte (spaeter zu klaeren)

- [ ] Tech-Stack fuer Mobile App (React Native / Flutter / Native / PWA)
- [ ] Genauer Preis fuer Assistant-Tier und Erweiterungspakete
- [ ] WhatsApp Business API Einschraenkungen fuer Outbound-Nutzung
- [ ] Datenschutz-Konzept (DSGVO, UAE Data Protection)
- [ ] Google API Verification beantragen (4-6 Wochen Vorlauf)
- [ ] Microsoft App Registration + Publisher Verification
- [ ] Token-Speicherung: AES-256-GCM + AWS KMS fuer Encryption Keys
- [ ] Audit-Log fuer jeden Zugriff auf User-Postfach/Kalender
- [ ] E-Mail-Senden im User-Namen: Bestaetigung vor jedem Senden oder nur bei Erstmaligem?
- [ ] Knowledge Graph: Scope und Zeitplan fuer Phase 2
- [ ] Partner-API fuer Etisalat und weitere B2B2C-Partner
- [ ] Detailliertes Sicherheitskonzept (Verschluesselung, Zugriffskontrolle, Sandbox)
- [ ] Branding: Eigene App "Luna" vs. "Amira.Luna"

---

*Erstellt am 7. Maerz 2026 | LUNA by AMIRA — almost human*
