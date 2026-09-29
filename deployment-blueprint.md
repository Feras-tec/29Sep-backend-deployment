# Deployment-Blueprint

## Gewählter Stack

Für mein Deployment verwende ich:

- **Render** für das Node.js-/Express-Backend
- **Neon** für die PostgreSQL-Datenbank
- **Prisma** als ORM
- **GitHub** für die Versionsverwaltung und das automatische Deployment

Der Aufbau sieht so aus:

```text
GitHub → Render → Node.js/Express → Prisma → Neon PostgreSQL
```

## 1. Remote PostgreSQL-Datenbank mit Neon

Zuerst erstelle ich bei Neon ein neues PostgreSQL-Projekt.

Nach dem Erstellen der Datenbank stellt Neon einen sicheren
Connection String bereit. Dieser enthält die Informationen, die meine
Anwendung benötigt, um sich mit der PostgreSQL-Datenbank zu verbinden.

Ein Connection String sieht zum Beispiel so aus:

```text
postgresql://USER:PASSWORD@HOST/DATABASE?sslmode=require
```

Diesen Connection String speichere ich nicht direkt im Quellcode und
auch nicht im GitHub-Repository.

Lokal verwende ich dafür eine `.env`-Datei:

```env
DATABASE_URL="postgresql://..."
```

Die `.env`-Datei muss in `.gitignore` eingetragen sein, damit geheime
Zugangsdaten nicht zu GitHub hochgeladen werden.

## 2. Umgebungsvariablen auf Render

Geheime Daten werden in der Produktion nicht in einer lokalen
`.env`-Datei gespeichert. Stattdessen werden sie als Environment
Variables auf der Hosting-Plattform eingerichtet.

Auf Render füge ich zum Beispiel folgende Variable hinzu:

```text
DATABASE_URL = <Connection-String-von-Neon>
```

Meine Anwendung kann anschließend über `process.env.DATABASE_URL`
auf diese Variable zugreifen.

Dadurch bleibt der Connection String außerhalb des Quellcodes und wird
nicht versehentlich in das GitHub-Repository hochgeladen.

## 3. Build- und Startbefehle

Damit Render die Anwendung bereitstellen kann, müssen zuerst die
Dependencies installiert und der Prisma Client generiert werden.

Als Build-Befehl verwende ich:

```bash
npm install && npx prisma generate
```

Danach wird die Anwendung mit folgendem Start-Befehl gestartet:

```bash
npm start
```

In der `package.json` muss dafür ein passendes Start-Script vorhanden
sein, zum Beispiel:

```json
{
  "scripts": {
    "start": "node server.js"
  }
}
```

Für Änderungen am Datenbankschema können vorhandene Prisma-Migrationen
in der Produktionsumgebung mit folgendem Befehl ausgeführt werden:

```bash
npx prisma migrate deploy
```

## 4. GitHub mit Render verbinden

Nachdem mein Projekt auf GitHub hochgeladen wurde, verbinde ich das
GitHub-Repository mit Render.

Auf Render erstelle ich einen neuen Web Service und wähle mein
GitHub-Repository aus.

Danach konfiguriere ich:

- das richtige Repository und den Branch, zum Beispiel `main`
- den Build-Befehl
- den Start-Befehl
- die benötigten Environment Variables

Wenn automatisches Deployment aktiviert ist, erkennt Render neue
Änderungen im verbundenen Repository.

Der Ablauf sieht dann so aus:

```text
Code ändern
    ↓
git add / git commit
    ↓
git push
    ↓
GitHub
    ↓
Render startet automatisch ein neues Deployment
```

Dadurch muss die Anwendung nach jeder Änderung nicht manuell neu
bereitgestellt werden.

## 5. Deployment testen

Nach einem erfolgreichen Deployment stellt Render eine öffentliche URL
für meine Anwendung bereit.

Zum Beispiel:

```text
https://mein-backend.onrender.com
```

Zuerst teste ich einen einfachen GET-Endpunkt, zum Beispiel:

```text
GET https://mein-backend.onrender.com/health
```

Danach teste ich einen Endpunkt, der Daten aus der PostgreSQL-Datenbank
liest:

```text
GET https://mein-backend.onrender.com/api/patients
```

Anschließend teste ich mit einer POST-Anfrage, ob neue Daten in der
entfernten Datenbank gespeichert werden können:

```text
POST https://mein-backend.onrender.com/api/patients
```

Wenn die GET-Anfrage Daten aus Neon zurückgibt und die POST-Anfrage neue
Daten erfolgreich speichert, funktioniert die Verbindung zwischen
Render, Prisma und der entfernten PostgreSQL-Datenbank.

Zusätzlich überprüfe ich bei Problemen die Deployment- und Server-Logs
auf Render.
