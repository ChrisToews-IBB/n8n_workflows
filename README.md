# n8n Workflows

Eine Sammlung von vier produktiven n8n-Automatisierungsworkflows — von KI-gestützter E-Mail-Verarbeitung über Web-Scraping bis hin zu einem vollständigen HR-Chatbot mit Spracherkennung.

## Workflows

### 1. E-Mail-Bot (`E_mail_bot.json`)

Überwacht ein Gmail-Postfach und erkennt automatisch eingehende Sponsoring-Anfragen.

**Ablauf:**
1. Gmail Trigger prüft das Postfach jede Minute
2. E-Mail-Kontext (Absender, Betreff, Text) wird aufbereitet
3. Google Gemini (1.5 Flash) analysiert ob es sich um eine Sponsoring-Anfrage handelt
4. Bei Ja → Lead wird in Airtable gespeichert + KI-generierter Antwort-Entwurf in Gmail erstellt
5. Bei Nein → Keine Aktion

**Integrationen:** Gmail · Google Gemini · Airtable

---

### 2. HR & IT Helpdesk Chatbot mit Audio (`HR___IT_Helpdesk_Chatbot_with_Audio.json`)

Telegram-Chatbot für HR- und IT-Anfragen mit Sprachnachrichten-Unterstützung und RAG-Wissensbasis.

**Ablauf:**
1. Telegram Trigger empfängt Nachrichten
2. Switch erkennt ob Text- oder Sprachnachricht gesendet wurde
3. Sprachnachrichten → OpenAI Whisper transkribiert Audio zu Text
4. AI Agent (OpenAI) beantwortet Fragen anhand einer PostgreSQL Vektor-Datenbank (PGVector)
5. HR-Dokumente (PDF) werden als Wissensbasis eingelesen, per Embeddings vektorisiert und in der DB gespeichert
6. Gesprächsverlauf wird pro Nutzer in Postgres gespeichert (Chat Memory)
7. Antwort wird zurück an Telegram gesendet

**Integrationen:** Telegram · OpenAI (GPT + Whisper + Embeddings) · PostgreSQL · PGVector

---

### 3. Web Scraper (`Scraper.json`)

Extrahiert strukturierte Daten (FAQs, Kontaktinfos, Support-Inhalte) von einer Website via Firecrawl API.

**Ablauf:**
1. Manueller Start
2. Firecrawl API startet den Scraping-Job für die Ziel-URL
3. Polling-Loop prüft ob die Extraktion abgeschlossen ist
4. Bei leerem Ergebnis → kurz warten und erneut prüfen
5. Bei vollständigem Ergebnis → strukturierte Daten (FAQs, Kundensupport-Infos) werden weitergegeben

**Integrationen:** Firecrawl API · HTTP Request

---

### 4. Swoppen Automatisierung (`Automatization_2_.json`)

Vollautomatische Kundenanfragen-Pipeline per E-Mail mit KI-Qualitätsprüfung vor dem Versand.

**Ablauf:**
1. IMAP Trigger empfängt eingehende E-Mails
2. Gemini analysiert ob es eine echte Kundenanfrage ist
3. Bei Ja → Kunde wird in Airtable erfasst
4. Vector-Answer-Agent sucht passende Antwort in der PostgreSQL Vektor-Wissensbasis
5. Feedback-Agent prüft die Antwortqualität
6. Bei guter Qualität → E-Mail wird gesendet
7. Bei schlechter Qualität → Revision-Agent überarbeitet die Antwort (Schleife)

**Integrationen:** IMAP · Google Gemini (2.0 Flash + Embeddings) · Airtable · PostgreSQL · PGVector

---

## Voraussetzungen

- [n8n](https://n8n.io) (Self-hosted oder Cloud)
- API-Zugänge je nach Workflow:

| Workflow | Benötigte Credentials |
|---|---|
| E-Mail-Bot | Gmail OAuth2, Google Gemini API, Airtable Token |
| HR Chatbot | Telegram Bot Token, OpenAI API, PostgreSQL |
| Scraper | Firecrawl API Key |
| Automatisierung | IMAP Account, Google Gemini API, Airtable Token, PostgreSQL |

## Import

1. n8n öffnen
2. Oben rechts auf **"+"** → **"Import from file"**
3. Die gewünschte `.json` Datei auswählen
4. Credentials in den Nodes eintragen
5. Workflow aktivieren

## Hinweis

Die Workflows enthalten keine echten API-Keys oder Passwörter — alle Credentials müssen nach dem Import neu eingetragen werden.
