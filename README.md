# MNWeb CRM – eigenständiges Design-Frontend

Dieses Projekt übernimmt das vorhandene React-Frontend und beide CSS-Dateien aus CRM-MN. Es enthält weder Backend noch Datenbank oder echte Kundendaten. Alle vorhandenen Ansichten, Formulare, Navigation, SVG-Icons und responsiven Layouts bleiben erhalten. Die bisherige flache Komponentenstruktur ist bewusst beibehalten; Services, Datentypen und Demo-Daten sind separat organisiert.

## Lokal starten

Voraussetzung: Node.js 22.16+ aus der 22er-Reihe oder Node.js 24+ und npm.

```sh
cd CRM-MN-lovable
npm install
npm run dev
```

Adresse: http://localhost:5174 (bei belegtem Port zeigt Vite eine andere Adresse). Kein alter CRM-Server erforderlich. Keine .env nötig.

```sh
npm run typecheck
npm run build
npm run preview
```

Build-Ausgabe: dist/. Zum Beenden Strg+C im Terminal.

## Demo bedienen

Die Anmeldung ist eine Simulation, keine echte Authentifizierung. Jedes nicht leere Passwort funktioniert; keine Passwörter werden gespeichert. Ausschließlich erfundene Daten eingeben.

| E-Mail | Ansicht |
| --- | --- |
| admin@example.test | Management, alle Funktionen |
| technik@example.test | Technik, alle Funktionen |
| success@example.test | Customer Success, lesender Überblick |
| kunde1@example.test bis kunde6@example.test | Jeweils eigenes Kundendashboard |

Es gibt sechs erfundene Kunden mit unterschiedlichen Phasen, Inaktivität, pausierter Betreuung, abgeschlossenem Prozess und leerer Berichtshistorie. Seite zwei der Übersicht ist ebenfalls erreichbar. Suche ohne Treffer zeigt den Leerzustand; unbekanntes Konto oder ungültiger Wochenbericht zeigen Fehlermeldungen. Eine kurze simulierte Antwortzeit erhält Lade-/Speicherzustände.

Kunden und Gruppen anlegen, Kunden bearbeiten, Aufgaben abhaken, Phasen freischalten, Wochenberichte erstellen/ändern und Vorlagen bearbeiten funktionieren im Arbeitsspeicher. Abmelden erhält Änderungen, Neuladen setzt alles einschließlich Anmeldung auf die Demo-Ausgangsdaten zurück. Neu angelegte Kunden können sich bis zum Neuladen mit ihrer eingegebenen E-Mail und beliebigem Passwort anmelden. Vorlagenänderungen gelten wie bisher nur für neue Kunden. Google Ads ist optional. Die sechs Phasen und 40 Aufgaben entsprechen der fachlichen PDF-Referenz; nur diese statischen Fachtexte wurden übernommen, keine Prisma-Dateien oder Datenbankinhalte.

## Struktur und Service-Grenze

Alle UI-Aufrufe laufen über src/services/api.ts. Der Adapter ruft ausschließlich mock-service.ts auf, niemals fetch oder einen lokalen Server. mock-service.ts simuliert alle von der bestehenden Oberfläche genutzten Endpunkte, Rollen, Fehler und Änderungen. mock-data.ts erzeugt die erfundenen Daten, process-definition.ts enthält die Phasentexte. types/contracts.ts enthält ausschließlich TypeScript-Datentypen.

Für eine spätere echte API wird der Adapter ausgetauscht. Die Demo-Rollen sind keine Sicherheitsgrenze und ersetzen keine serverseitige Autorisierung. Dieses Projekt ist zur Designentwicklung gedacht.

## Für Lovable verwenden

Übernehmen: den gesamten src/-Ordner, public/, index.html, package.json, package-lock.json, tsconfig.json, vite.config.ts, .gitignore und diese README. Den Inhalt dieses Ordners als Projektwurzel verwenden, nicht den übergeordneten CRM-MN-Ordner. node_modules/ und dist/ entstehen nur durch Installation/Build und werden nicht übertragen.

Nicht übernommen: server/, prisma/, Datenbankdateien, Migrationen, Backups, .env, Backend-Tests, Backend-Skripte, lokale Tools und ursprüngliche Build-Artefakte. Nicht mehr benötigt: @prisma/client, prisma, express, cookie-parser, express-rate-limit, helmet, dotenv, zod, concurrently, tsx und Backend-Typenpakete. Kein Prisma-postinstall, kein API-Proxy, keine Datenbankbefehle. Keine externen Bild- oder Fontdateien vorhanden; Icons sind SVG in components.tsx, Fonts verwenden die bestehende System-Fallback-Liste.

## Aktuelle Lovable-Grenzen (geprüft am 16.09.2026)

Die [offizielle GitHub-Dokumentation](https://docs.lovable.dev/integrations/github) beschreibt keinen direkten Import bestehender Repositories. Lovable legt beim Verbinden ein eigenes Repository an. Deshalb ist dieser Ordner eine portable Frontend-Übergabe, kein garantierter Ein-Klick-Import.

Laut [Lovable-FAQ](https://docs.lovable.dev/introduction/faq) verwenden neue Projekte TanStack Start/SSR; ältere React/Vite-Projekte bleiben unterstützt. Bei der Übernahme in ein neues Lovable-Projekt dessen Framework-Konfiguration beibehalten und die vorhandene App als Client-Ansicht einbinden (sie verwendet location, window und DOM-Dialoge). main.tsx/index.html/vite.config.ts dort nicht blind über die Framework-Dateien kopieren. In einer passenden React/Vite-Projektbasis kann dieser Aufbau direkt verwendet werden. Das tatsächliche Lovable-Zielprojekt ist noch nicht verbunden oder getestet.

Empfohlener Arbeitsauftrag dort: „Verwende ausschließlich das übergebene React-Frontend und die bestehenden CSS-Dateien. Erhalte Design und sämtliche Ansichten. Arbeite weiter mit der Mock-Service-Schicht. Keine Datenbank, Supabase, echte Anmeldung oder Backend-Integration hinzufügen.“
