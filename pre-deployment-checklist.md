# Pre-Deployment-Checkliste

## 1. Sicherheit

Bevor ich meine Anwendung deploye, überprüfe ich folgende Punkte:

- [ ] API-Schlüssel, Passwörter und `DATABASE_URL` stehen nicht im Quellcode.
- [ ] Geheime Daten werden als Environment Variables auf der Hosting-Plattform gespeichert.
- [ ] Die `.env`-Datei ist in `.gitignore` eingetragen.
- [ ] CORS erlaubt nur die benötigten und bekannten Origins.
- [ ] Sicherheits-Header werden zum Beispiel mit `helmet` eingerichtet.
- [ ] Rate Limiting schützt die API vor zu vielen Anfragen.
- [ ] Eingehende Daten werden validiert.

## 2. Datenbankverwaltung

Vor dem Deployment überprüfe ich die Datenbankkonfiguration:

- [ ] Die `DATABASE_URL` zeigt auf die richtige Produktionsdatenbank.
- [ ] Datenbankänderungen werden über kontrollierte Migrationen durchgeführt.
- [ ] In der Produktion verwende ich für vorhandene Prisma-Migrationen:

```bash
npx prisma migrate deploy
```

- [ ] Ich verwende in der Produktion nicht einfach `prisma db push` für Schemaänderungen.
- [ ] Wichtige Datenbankfelder werden auf sinnvolle Indizes überprüft.
- [ ] Vor kritischen Änderungen sollte ein Backup der Datenbank vorhanden sein.

## 3. Fehlerbehandlung und Logs

Vor dem Deployment überprüfe ich die Fehlerbehandlung:

- [ ] Interne Fehlermeldungen und Stacktraces werden nicht an öffentliche Nutzer gesendet.
- [ ] Nutzer erhalten stattdessen eine allgemeine Fehlermeldung, zum Beispiel:

```json
{
  "error": "Internal Server Error"
}
```

- [ ] Technische Fehler werden serverseitig protokolliert.
- [ ] Die Logs enthalten keine Passwörter, API-Schlüssel oder andere Geheimnisse.
- [ ] Nach dem Deployment überprüfe ich die Live-Logs im Dashboard der Hosting-Plattform.
- [ ] Bei Problemen nutze ich die Logs, um Fehler beim Start oder bei API-Anfragen zu finden.

## 4. Umgebung einrichten

Vor dem Deployment überprüfe ich die Produktionsumgebung:

- [ ] Nur notwendige Production-Dependencies werden für die laufende Anwendung verwendet.
- [ ] Entwicklungswerkzeuge und Testpakete stehen in `devDependencies`.
- [ ] Lokale Testkonfigurationen werden nicht für die Produktionsumgebung verwendet.
- [ ] Environment Variables für Production sind korrekt eingerichtet.
- [ ] Die Anwendung verwendet den von der Hosting-Plattform bereitgestellten Port über `process.env.PORT`.
- [ ] Das `start`-Script in der `package.json` startet die Anwendung korrekt.
- [ ] Nicht benötigte lokale Dateien werden nicht in das Deployment aufgenommen.

## Abschlusskontrolle

Vor dem Deployment prüfe ich noch einmal:

- [ ] Keine Geheimnisse befinden sich im GitHub-Repository.
- [ ] Die Datenbankverbindung funktioniert.
- [ ] Prisma-Migrationen sind vorbereitet.
- [ ] CORS, Helmet und Rate Limiting sind konfiguriert.
- [ ] Fehlerbehandlung und Logs funktionieren.
- [ ] Build- und Startbefehle sind korrekt.
- [ ] Die Anwendung kann in der Produktionsumgebung gestartet werden.
