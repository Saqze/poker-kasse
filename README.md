# Poker Kasse – Sicherheitsupdate v3

**Wichtig:** Der Einladungslink ist weiterhin der Zugangsschlüssel. Es gibt **noch keinen sicheren PIN-Schutz** und keine überprüfbare Gruppenmitgliedschaft. Jeder mit der vollständigen Runden-ID kann nach anonymer Anmeldung Daten lesen und ändern. Die neuen Regeln prüfen Struktur, Anzahl und Werte, verhindern aber keine absichtlichen Änderungen durch eingeladene oder unbefugte Link-Besitzer. Keine sensiblen Informationen speichern.

## Nächste Schritte
1. In Firebase → Firestore → Regeln den **gesamten** Inhalt der Datei `firestore.rules` einfügen und **Veröffentlichen** wählen.
2. In Firebase → Projekteinstellungen → Deine Apps eine Web-App registrieren und die Angaben in `firebase-config.js` ersetzen.
3. Ordner `poker-app` auf GitHub Pages oder Netlify veröffentlichen.
4. Eine neue Runde erstellen und den Link mit einem zweiten Handy testen.

**Verbesserungen v3:** striktere Feldvalidierung; maximal zehn Spieler, maximal 100 Buy-ins pro Spieler, maximal 20.000 € Chipwert; neue Räume werden nur ausdrücklich durch „Neue Runde“ erstellt; ungültige oder nicht existierende Einladungslinks erzeugen keine neuen Räume. Bei „Neue Runde mit Spielern“ werden die Namen übernommen.

**Hinweis:** Die Sicherheitsregeln wurden nicht gegen einen laufenden Firebase-Emulator getestet. Vor dem Teilen mit Freunden einen Test mit zwei Geräten durchführen. Die Regelvalidierung ist nicht gleichbedeutend mit vollständigem Zugriffsschutz.

---

# Poker Kasse ♠ – Handy-Web-App

Die App ist fertig programmiert. Sie funktioniert auf iPhone, Android und PC. **Gemeinsame Echtzeitnutzung benötigt Firebase und eine öffentlich erreichbare HTTPS-Adresse.** Ohne Konfiguration gibt es einen lokalen Demo-Modus; der synchronisiert sich nicht mit anderen Geräten.

## Einmalig einrichten (ca. 15–30 Minuten)

1. Auf https://console.firebase.google.com ein Projekt anlegen.
2. Unter **Build → Authentication → Sign-in method** den Anbieter **Anonymous** aktivieren.
3. Unter **Build → Firestore Database** eine Datenbank erstellen.
4. In **Firestore Database → Rules** die Regeln aus `firestore.rules` einfügen und veröffentlichen.
5. Unter **Projekteinstellungen → Deine Apps → Web-App** eine Web-App registrieren. Die dort gezeigten Werte `apiKey`, `authDomain`, `projectId`, `appId` in `firebase-config.js` eintragen.
6. Den gesamten Ordner auf einer HTTPS-Webhosting-Plattform veröffentlichen, z. B. Firebase Hosting, Netlify oder GitHub Pages. **Wichtig:** `index.html` nicht einfach per `file://` öffnen, da JavaScript-Module so oft blockiert werden.
7. Die öffentliche URL auf dem Handy öffnen, „Neue Runde“ wählen und den Einladungslink mit der Gruppe teilen. Über „Zum Home-Bildschirm“ lässt sich die Web-App wie eine App starten.

## Bedienung

- Jeder neue Spieler startet mit 1 Buy-in zu 20 €. Über + und − können Rebuys erfasst werden.
- Zum Schluss bei jedem Spieler den **gesamten Chipwert in Euro** eintragen.
- Erst wenn die Summe aller Chipwerte der Summe aller Buy-ins entspricht, zeigt die App Überweisungen an.
- Die Abrechnung verwendet Cent-Beträge und verrechnet Zahler mit Empfängern, sodass nicht jeder jedem überweisen muss.
- Über „Neue Runde mit Spielern“ werden die Namen übernommen, aber die Einsätze zurückgesetzt.
- Export als CSV ist möglich.

## Wichtige Hinweise

- **Zugang:** Jeder, der den langen zufälligen Einladungslink kennt, kann die Runde ansehen und verändern. Nicht öffentlich teilen. Es gibt derzeit keine Moderatorenrollen, Benutzerkonten oder Zugriffsentzug.
- **Gleichzeitige Änderungen:** Die Live-Synchronisation funktioniert; Änderungen werden als Firestore-Transaktionen auf den jeweils neuesten Stand angewendet. Dadurch überschreiben gleichzeitige Änderungen an unterschiedlichen Feldern einander nicht. Bei Änderungen am selben Feld gilt die zuletzt erfolgreich abgeschlossene Aktion.
- **Keine Überweisungen werden tatsächlich ausgelöst.** Die App zeigt nur an, wer an wen zahlen sollte.
- **Kosten:** Firebase und Hosting bieten häufig kostenlose Einstiegs-Kontingente; prüfe die aktuellen Konditionen selbst.
- **Sicherheit:** Die Firestore-Regeln sind für eine private Freundesgruppe mit geheimen UUID-Links gedacht, nicht für eine öffentliche App. Für eine öffentliche Veröffentlichung wären sichere Mitgliedschaften, strengere Feldvalidierung und ggf. Missbrauchsschutz erforderlich.

## Version 2: Gemeinsames Bearbeiten

- Bis zu 10 Spieler. Alle mit Link können Buy-ins und Chipwerte ändern.
- Änderungen erfolgen über Firestore-Transaktionen, inklusive Änderungsverlauf der letzten 20 Aktionen und gemeinsamer Rückgängig-Funktion.
- **PIN noch nicht implementiert**: Ein nur clientseitig geprüfter PIN wäre kein sicherer Zugriffsschutz. Die App benötigt für echten PIN-Schutz eine serverseitige Berechtigungsprüfung; aktuell ist der geheime Einladungslink der einzige Zugangsschutz.
- Bitte die aktualisierte `firestore.rules` in Firebase veröffentlichen, sonst schlägt das Speichern fehl.
- Neue Runden starten mit einer neuen ID. Vergangene Runden lassen sich über ihre gespeicherten Links wieder öffnen.
