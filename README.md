# Backend-Deployment: Von localhost in die Produktion

## Teil 1: Deployment-Konzepte und Grundlagen

### 1. Was ist Deployment?

Backend-Deployment bedeutet, dass eine Backend-Anwendung von der lokalen
Entwicklungsumgebung auf einen Server übertragen wird. Dadurch ist die
Anwendung über das Internet erreichbar und kann von echten Nutzerinnen
und Nutzern verwendet werden.

### 2. Der Deployment-Prozess

Ein typischer Deployment-Prozess sieht so aus:

1. Der Code wird zu GitHub gepusht.
2. Die Hosting-Plattform lädt den aktuellen Code aus dem Repository.
3. Die benötigten Dependencies werden installiert.
4. Umgebungsvariablen werden auf der Plattform bereitgestellt.
5. Die Anwendung wird gebaut und gestartet.
6. Die Plattform stellt eine öffentliche URL bereit.
7. Die API kann Anfragen aus dem Internet empfangen und beantworten.

### 3. Die Einschränkungen von localhost

`localhost` ist nur auf dem eigenen Computer erreichbar. Andere
Nutzerinnen und Nutzer können nicht einfach über das Internet darauf
zugreifen. Außerdem ist ein persönlicher Computer nicht dafür gedacht,
eine Anwendung rund um die Uhr zuverlässig bereitzustellen.

Für eine Produktionsanwendung ist deshalb ein öffentlich erreichbarer
und dauerhaft verfügbarer Server notwendig.

### 4. Trennung von Zuständigkeiten

In der Produktion werden Backend und Datenbank häufig auf getrennten
Diensten betrieben.

Zum Beispiel:

- Express läuft auf einem Anwendungshost.
- PostgreSQL oder MongoDB läuft auf einem Datenbankdienst.

Dadurch können Backend und Datenbank unabhängig verwaltet, aktualisiert,
gesichert und skaliert werden. Außerdem kann ein spezialisierter
Datenbankanbieter Aufgaben wie Backups und Datenbankverwaltung übernehmen.

---

## Teil 2: Plattformüberblick und Optionen für Lernende

### 1. Plattformrecherche

Für das Hosting einer Node.js-Backend-Anwendung habe ich Render und
Railway verglichen.

#### Render

Render bietet einen kostenlosen Tarif für Web Services und eignet sich
zum Beispiel für Node.js- und Express-Anwendungen.

Der kostenlose Web Service bietet 512 MB RAM. Pro Workspace stehen
monatlich 750 kostenlose Instanzstunden zur Verfügung.

Für größere oder produktive Anwendungen gibt es kostenpflichtige
Compute-Pläne.

#### Railway

Railway kann ebenfalls Node.js-Anwendungen und Datenbanken bereitstellen.

Neue Nutzer erhalten zunächst einen 30-Tage-Testzeitraum mit 5 US-Dollar
Guthaben. Danach gibt es einen kostenlosen Tarif mit 1 US-Dollar
Guthaben pro Monat.

Der Free-Tarif benötigt keine Kreditkarte und eignet sich deshalb gut
zum Experimentieren und für kleine Lernprojekte.

### Database-as-a-Service

Für Datenbanken habe ich MongoDB Atlas und Neon verglichen.

#### MongoDB Atlas

MongoDB Atlas bietet einen kostenlosen M0-Cluster.

Der kostenlose Cluster enthält:

- 512 MB Speicher
- gemeinsam genutzte CPU- und RAM-Ressourcen
- keine zeitliche Begrenzung des Free-Clusters

Dieser Tarif eignet sich besonders zum Lernen und für kleine
MongoDB-Projekte mit Mongoose.

#### Neon

Neon bietet verwaltete PostgreSQL-Datenbanken an und eignet sich deshalb
gut für Projekte mit PostgreSQL und Prisma.

Der Free-Tarif bietet 0,5 GB Speicher pro Projekt. Dadurch kann Neon für
kleine Lern- und Entwicklungsprojekte kostenlos verwendet werden.

### 2. Kostenanalyse

Die kostenlosen Tarife reichen für kleine Lernprojekte meistens aus,
haben aber technische Limits.

Bei Render kann ein kostenpflichtiger Web Service zum Beispiel ab
7 US-Dollar pro Monat verwendet werden.

Railway verwendet ein nutzungsabhängiges Preismodell. Nach dem
kostenlosen Tarif gibt es unter anderem den Hobby-Tarif mit einem
Mindestpreis von 5 US-Dollar pro Monat.

MongoDB Atlas bietet neben dem kostenlosen M0-Cluster kostenpflichtige
Flex- und Dedicated-Cluster an.

Neon bietet neben dem Free-Tarif kostenpflichtige, nutzungsabhängige
Tarife an.

### Welche Optionen sind für Lernende sicher?

Für Lernende sind kostenlose Tarife besonders geeignet, wenn keine
Kreditkarte erforderlich ist und klare Nutzungslimits vorhanden sind.

Für ein kleines Lernprojekt würde ich zum Beispiel folgende Kombination
verwenden:

- Render für das Node.js-/Express-Backend
- Neon für PostgreSQL mit Prisma

Wichtig ist, die Nutzungslimits regelmäßig zu kontrollieren und nicht
unbeabsichtigt auf einen kostenpflichtigen Tarif zu wechseln.

---

## Teil 3: Limits kostenloser Tarife einfach erklärt

### 1. RAM- und Speicherlimits

RAM bestimmt, wie viel Arbeitsspeicher eine Anwendung während der
Ausführung verwenden kann.

Wenn eine Anwendung mehr RAM benötigt als der Tarif erlaubt, kann sie
langsamer werden, abstürzen oder von der Hosting-Plattform beendet
werden.

### 2. Cold Starts und Ruhephasen

Bei einigen kostenlosen Hosting-Tarifen wird eine Anwendung nach einer
bestimmten Zeit ohne Nutzung in einen Ruhezustand versetzt.

Kommt danach eine neue Anfrage, muss die Anwendung zuerst wieder
gestartet werden. Dies nennt man Cold Start.

Dadurch kann die erste Anfrage deutlich länger dauern als normale
Anfragen.

### 3. Rechenstunden und CPU-Kontingente

Ein kostenloser Tarif kann die Anzahl der Stunden begrenzen, in denen
eine Anwendung pro Monat ausgeführt werden darf.

Wenn das kostenlose Kontingent erreicht ist, kann die Anwendung
angehalten werden oder ein kostenpflichtiger Tarif notwendig werden.

CPU-Limits bestimmen außerdem, wie viel Rechenleistung die Anwendung
verwenden darf.

### 4. Datenbankspeicher und aktive Verbindungen

Datenbankanbieter begrenzen bei kostenlosen Tarifen häufig den
verfügbaren Speicherplatz und die Anzahl gleichzeitig aktiver
Datenbankverbindungen.

ORMs wie Prisma oder ODMs wie Mongoose stellen Verbindungen zwischen
der Anwendung und der Datenbank her. Wenn sehr viele Instanzen oder
Verbindungen gleichzeitig geöffnet werden, kann das Verbindungslimit
der Datenbank erreicht werden.

Deshalb sollte das Connection Management korrekt konfiguriert werden.

### 5. Ausgehender Datentransfer und Bandbreite

Ausgehender Datentransfer beschreibt die Datenmenge, die ein Server an
Nutzer oder andere Dienste sendet.

Wenn ein kostenloser Tarif zum Beispiel nur eine bestimmte Menge
Datentransfer pro Monat erlaubt, kann die Anwendung nach Erreichen
dieses Limits eingeschränkt werden oder zusätzliche Kosten verursachen.

Für Lernprojekte sind diese Limits normalerweise ausreichend. Trotzdem
sollte man die Nutzung im Dashboard der Hosting-Plattform regelmäßig
kontrollieren.
