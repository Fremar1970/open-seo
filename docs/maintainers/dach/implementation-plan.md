# DACH-SEO-Bericht: Umsetzungsplan

Stand: 9. Oktober 2026. Grundlage: lokal geprüfter OpenSEO-Commit `89e5a00717cdf049b7c2b4e29646edb3a7d4995b`.

## Produkt und Umfang

Einmaliger SEO-Bericht ohne Kundenkonto. Website eingeben, Zielmarkt Deutschland, Österreich oder Schweiz auswählen, begrenzte Vorschau erhalten, bezahlen und vollständigen Bericht herunterladen. Optional Branche, Ort und bekannte Wettbewerber angeben. Eine E-Mail-Adresse für Beleg und Wiederherstellung des Zugangs ist kein Kundenkonto.

Vorgeschlagene Grenzen der ersten Version: bis zu 100 Seiten der eingegebenen Website, bis zu 30 Keywords und bis zu drei verifizierte Wettbewerber. Die Grenzen sind Obergrenzen, keine Garantie vorhandener Daten. Länder separat auswerten; nicht DACH-Suchvolumen zusammenrechnen. Sprache zunächst Deutsch.

## Geprüfte Grundlage im Repository

- `src/server/workflows/SiteAuditWorkflow.ts`: vorhandener Audit-Workflow.
- `src/serverFunctions/audit.ts`: Audit-Aufrufe benötigen Projektkontext; Start verwendet Benutzer- und Abrechnungskontext.
- `src/shared/audit-issues.ts`: vorhandener Fehlerkatalog mit Schweregrad, Erklärung und Korrekturanleitung.
- `src/server/features/keywords/services/KeywordResearchService.ts`: Keyword-Recherche.
- `src/server/features/domain/services/DomainService.ts`: Domainanalyse als Integrationskandidat.
- `src/server/features/projects/services/websiteResearch.ts`: Wettbewerber-Recherche mit Prüfung der tatsächlichen Angebote und Quellen.
- `src/serverFunctions/reports.ts` und `src/server/features/reports/services/ReportService.ts`: bestehende HTML-Berichte mit Projektzuordnung. Gastzahlung und PDF-Export werden dadurch nicht automatisch abgedeckt.

Vorhandene Services wiederverwenden, sobald ihre Berechtigungs- und Abrechnungsbedingungen für Gastaufträge explizit geklärt sind. Nicht lediglich Auth-Middleware entfernen oder den lokalen No-Auth-Modus öffentlich betreiben.

## Kundenablauf

1. Domain normalisieren und Zielmarkt auswählen.
2. Kostenlos nur wenige Seiten prüfen; Vorschau und tatsächliche Erreichbarkeit anzeigen. Begrenzung pro Sitzung/IP und Domain verhindert unbeschränkte kostenlose Analysen.
3. Leistung, Preis, Grenzen und Datenverfügbarkeit vor der Zahlung anzeigen. Preis erst nach Kostenmessung festlegen.
4. Gast-Checkout anlegen; Stripe Checkout ist ein vorgeschlagener Zahlungsanbieter, noch nicht eingerichtet.
5. Signierten Zahlungs-Webhook prüfen und Bestellung serverseitig als bezahlt markieren. Eine Rückkehr zur Erfolgsseite ist kein Zahlungsnachweis. Verzögerte Zahlungen erst bei bestätigtem Zahlungseingang berücksichtigen.
6. Bezahlten Analyseauftrag asynchron starten. Fortschritt über einen geheimen Bestellzugang anzeigen.
7. PDF und CSV erstellen und nach Zahlung über zeitlich begrenzten, geschützten Download bereitstellen. Optional E-Mail-Link senden.

## Bericht

1. Kurzüberblick: Domain, Zielmarkt, Erfassungszeitpunkt, geprüfte Seiten, Einschränkungen.
2. Sofortmaßnahmen-Plan für heute: bis zu zehn konkrete, belegbare Aufgaben.
3. Technischer Audit: Befunde mit betroffenen URLs und Evidenz.
4. Keywords: Suchvolumen, verfügbare Rankingdaten, Datenquelle und Zeitpunkt.
5. Wettbewerber: Auswahlbegründung, Überschneidungen und konkrete Inhaltschancen.
6. Weiterführender Maßnahmenplan: komplexere Arbeiten für spätere Umsetzung.
7. Methodik und Grenzen: geschätzte Kennzahlen, unzugängliche Seiten und fehlende Daten.

Keine Indexierungsbehauptungen allein aus einem Crawl ableiten. Ohne Zugriff auf die Search Console können beispielsweise Indexierbarkeitssignale beurteilt, aber nicht alle tatsächlichen Google-Indexierungszustände bestätigt werden.

## Sofortmaßnahmen-Plan

Jede Aufgabe enthält eine ID, den zugrunde liegenden Befund, betroffene URL(s), Evidenz, erwarteten Nutzen, geschätzten Aufwand, erforderlichen Zugriff, konkrete Schritte und eine Erfolgskontrolle. Vorher/Nachher-Vorschläge nur liefern, wenn Seiteninhalt und Befund dies tragen. Vorschläge als Vorschläge kennzeichnen.

Priorisierung in zwei getrennten Bereichen:

- Dringende Probleme zuerst sichtbar machen, auch wenn sie Expertenhilfe brauchen; etwa belegbare Serverfehler. Diese dürfen nicht von einfachen kosmetischen Aufgaben verdrängt werden.
- Anschließend die Sofortmaßnahmen nach erwartetem Nutzen, einfachem Aufwand und Zuverlässigkeit des Befunds ordnen. Zeitangaben sind Schätzungen, abhängig vom CMS und den Zugriffsrechten.

Geeignete Kandidaten: fehlende oder doppelte Seitentitel, fehlende Beschreibungen, defekte interne Links und belegbar unklare Hauptüberschriften. Änderungen an Canonicals, noindex, robots.txt, hreflang und umfassenden Weiterleitungen benötigen Kontextprüfung; sie nicht pauschal als schnelle Klickaufgabe empfehlen. Eine Meta-Beschreibung als Verbesserung der Suchdarstellung erklären, nicht als garantierten Rankinggewinn.

Beispielvorlage, kein tatsächlicher Websitebefund:

> **Fehlenden Seitentitel ergänzen**
> URL: [tatsächlich geprüfte URL]
> Befund: [gemessener fehlender Titel mit Erfassungszeitpunkt]
> Aufwand: ungefähr 5–10 Minuten; Zugriff auf die Seiteneinstellungen erforderlich.
> Schritte: Seite im CMS öffnen, SEO-/Titel-Einstellung finden, passenden Titel eintragen, veröffentlichen.
> Vorschlag: [aus dem tatsächlichen Inhalt abgeleiteter Titel]
> Kontrolle: Veröffentlichte Seite erneut prüfen; der Titel muss im HTML vorhanden sein. Änderungen in Google können später erscheinen.

KI darf bestätigte Daten erklären und Textvorschläge formulieren. Auswahl und Nachweis der technischen Befunde erfolgen anhand strukturierter Analyseergebnisse. Websiteinhalte als untrusted Daten behandeln und keine darin enthaltenen Anweisungen ausführen.

## Auftragsschicht und Daten

Neue Gastbestellungen getrennt von bestehenden Benutzerprojekten modellieren; Integration gemäß Repository-Muster Serverfunktion → Service → Repository. SQLite und Postgres unterstützen.

- Bestellung: Domain, Zielmarkt, Paket, Preis in kleinster Währungseinheit, Währung, Zahlungsstatus, Provider-Referenz und Zeitstempel.
- Zugang: kryptografisch zufälliges Geheimnis; nur dessen Hash speichern. Bestell-ID allein gewährt keinen Zugriff.
- Zahlungsevent: eindeutige Provider-Event-ID zur Deduplizierung; Preis, Währung und Bestellzuordnung serverseitig abgleichen.
- Analyseauftrag: Bestellzuordnung, Status, Versuchszahl, Teilanalysen, Fehler und Datenzeitpunkte.
- Befunde und Maßnahmen: explizite Beziehungen zur Bestellung und zu geprüften URLs.
- Export: Dateireferenz, Format, Erzeugungszeitpunkt und Ablaufdatum; private Speicherung.

Zahlungsstatus und Analysestatus getrennt führen. Wiederholte Webhooks dürfen weder zweite Analysen noch doppelte Kosten verursachen. Queue-/Workflow-Wiederholungen müssen bereits abgeschlossene Teilanalysen erkennen. Rückerstattung und abgelaufene Zahlungen erhalten explizite Zustände.

Crawler und PDF-Renderer müssen interne/private Netzwerkziele auch nach DNS-Auflösung und Weiterleitungen sperren. Bestehende SSRF-Helfer prüfen und wiederverwenden. API-Schlüssel bleiben serverseitig. HTML-Berichte escapen bzw. kontrolliert rendern; PDF-Rendering darf keine beliebigen internen URLs laden.

## Kosten und Fehlerbehandlung

Je Bestellung Budget für Crawl, Daten-API, optionale KI-Erklärung, PDF, Speicherung und Zahlungskosten messen. Keine unbeschränkten Wettbewerberabfragen oder Keyword-Erweiterungen. Kleine Stichprobe über verschiedene reale Websitegrößen durchführen, bevor ein Verkaufspreis festgelegt wird.

Teilweise fehlende Keyword-/Wettbewerberdaten offen ausweisen; fehlende Daten niemals als null Suchvolumen oder null Fehler darstellen. Bei blockiertem Crawl keine normalen Vollständigkeitsangaben verwenden. Wiederholungsversuch, manueller Support und Rückerstattung bei nicht lieferbarem Bericht müssen vor Verkaufsstart konkret definiert werden.

## Umsetzung und Abnahme

1. Gastbestellungen, isolierter Zugang und Zahlungsintegration implementieren.
2. Audit-, Keyword- und Wettbewerberservices kontrolliert an bezahlte Aufträge anbinden; tatsächliche Länderparameter und Kosten prüfen.
3. Regelbasierte Maßnahmenauswahl und deutsche Berichtstexte erstellen.
4. Private PDF-/CSV-Exporte und Wiederherstellung des Downloads ergänzen.
5. Zahlungs-Sandbox und echte Analysefälle prüfen; dann Verkaufsbetrieb vorbereiten.

Wesentliche Abnahmekriterien: kein Download vor bestätigter Zahlung; kein Zugriff auf fremde Bestellungen; doppelte Webhooks verursachen genau einen Auftrag; DE/AT/CH werden korrekt an Datenanbieter übergeben; jede Sofortmaßnahme hat einen echten Befund; kritische Probleme bleiben sichtbar; unvollständige Daten werden gekennzeichnet; PDF ist lesbar und enthält keine abgeschnittenen Tabellen.

## Aktueller Stand

Repository lokal geklont und zentrale Integrationsstellen geprüft. Dieses Dokument ist die konkrete Umsetzungsspezifikation. Noch keine produktiven Zahlungen, keine API-Zugangsdaten, keine neue Anwendung implementiert oder veröffentlicht. Name, finaler Preis, Hosting und Zahlungsanbieter sind noch nicht verbindlich festgelegt.
