# Poker-Kasse v11 – Einladung nur per Link

Neue Runde erstellen, auf **Einladen** tippen und den vollständigen Link teilen. Empfänger öffnen den Link und gelangen automatisch in die Runde. Keine manuelle Runden-ID-Eingabe mehr. Die zufällige ID bleibt technisch im Link, weil sie die Runde identifiziert.

Die globale All-Time-Statistik und Löschfunktionen aus v10 bleiben erhalten. Die Firestore-Regeln sind unverändert; für dieses reine Oberflächen-Update müssen sie nicht neu veröffentlicht werden.

**Installation:** Die Dateien aus `poker-app/` in das GitHub-Repository hochladen und gleichnamige Dateien ersetzen. `firebase-config.js` enthält die vorhandene Konfiguration: vor dem Hochladen mit der funktionierenden GitHub-Datei abgleichen. Im privaten Browserfenster testen: Runde erstellen → Einladen → Link auf zweitem Gerät öffnen.

**Sicherheit:** Jeder mit dem vollständigen Link kann die Runde bearbeiten.

# Poker-Kasse v7 – Globale All-Time-Statistik

Alle abgeschlossenen Abende aus **neuen Einladungslinks** werden in Firestore unter `results/{roomId}` gespeichert und gemeinsam ausgewertet. Die Abrechnung bleibt in `rooms/{roomId}`. Die globale Speicherung und der Rundenabschluss erfolgen in einer Transaktion. Ein Link kann nur einmal zur globalen Statistik beitragen. Für jeden neuen Abend einen neuen Link erstellen.

## Installation
1. **Vorher Sicherung**: bisherige App und CSV-Exporte aufbewahren.
2. In Firebase Firestore **Regeln** den Inhalt von `firestore.rules` veröffentlichen.
3. Die Dateien aus `poker-app/` in GitHub Pages aktualisieren. Die `firebase-config.js` muss die bestehende gültige Konfiguration enthalten.
4. Im privaten Browserfenster testen: neue Runde, Spieler, ausgeglichene Chipwerte, abschließen, zweite neue Runde, wieder abschließen. Rangliste sollte beide anzeigen.

## Spieleridentität
Die Namen werden ohne Beachtung der Groß-/Kleinschreibung zusammengefasst. **Gleiche Namen werden als eine Person gezählt**; unterschiedliche Schreibweisen als verschiedene. Verwendet deshalb für jede Person einen festen, eindeutigen Spitznamen, z. B. `Max K` statt `Max`. Es gibt keine verifizierten Benutzerkonten.

## Grenzen und Sicherheit
Die globale Statistik ist **für alle anonym angemeldeten Nutzer der App lesbar**, unabhängig vom Einladungslink. Die Datenbank ist öffentlich über die App nutzbar; die globale Liste ist **nicht** auf eine private Freundesgruppe beschränkt. Jeder mit einem gültigen Rundenlink kann die zugehörige Runde bearbeiten und deren Ergebnis beeinflussen. Ein späterer Korrekturmechanismus für abgeschlossene Abende ist nicht enthalten. Bestehende historische Einträge aus v6 werden **nicht automatisch** in die globale Sammlung übertragen. Im Offline-Demomodus ist die Statistik nur lokal. Firebase Spark hat Nutzungsgrenzen; bei vielen Runden wird die komplette Ergebnisliste geladen.

Die Firestore-Regeln validieren die Datenstruktur, ersetzen aber kein sicheres Benutzer-/Gruppenberechtigungssystem. Bei sensiblen Namen oder privaten Gruppen sollte eine separate Gruppenidentität/Authentifizierung eingeführt werden.


## Version 8: Test-Ergebnisse löschen

- Unter „Vergangene Pokerabende“ lässt sich jeder globale Ergebnis-Eintrag nach Bestätigung löschen.
- Die globale Rangliste wird live neu berechnet; der ursprüngliche Rundenbeleg bleibt erhalten.
- Der Button „Offline testen“ wurde entfernt.
- **Wichtig:** `firestore.rules` muss ebenfalls veröffentlicht werden, sonst schlägt das Löschen fehl.
- **Sicherheitswarnung:** In dieser Testversion kann jeder anonym angemeldete App-Nutzer globale Ergebnisse löschen. Die globale Statistik ist damit nicht gegen unberechtigtes Löschen geschützt. Vor dem produktiven Dauerbetrieb ist ein Admin-Konzept nötig.


## Version 9 – Globale Statistik auf der Startseite
Die globale Statistik ist jederzeit sichtbar, auch ohne geöffnete Pokerrunde.
In der Rangliste kann ein Spieler nach Bestätigung aus allen gespeicherten Ergebnissen entfernt werden.
In der Abendliste kann ein kompletter Abend gelöscht werden.
Beide Vorgänge ändern nur die globalen `results`-Dokumente, nicht die ursprünglichen `rooms`-Daten.
Nach dem Löschen eines Ergebnisses kann derselbe Link nicht erneut archiviert werden, weil die Runde bereits abgeschlossen ist; in der aktuellen Implementierung prüft die App nur das Vorhandensein des Ergebnisses, daher ist eine erneute Archivierung theoretisch möglich, falls die Runde nochmals ausgeglichen wird.
Ohne Admin-Login kann jeder anonym angemeldete App-Nutzer die globale Statistik verändern.
Bei sehr vielen betroffenen Abenden werden Änderungen in Blöcken vorgenommen und können bei Verbindungsabbruch teilweise abgeschlossen sein.

## App-Icon auf iPhone und Android
Die Version v12 enthält PNG-Icons für Android (192/512 px), Apple-Touch-Icon (180 px) und Favicon (32 px). Nach dem Hochladen auf GitHub Pages ggf. bereits installierte Startbildschirm-Verknüpfung entfernen und neu hinzufügen. iPhone: Safari > Teilen > Zum Home-Bildschirm. Android: Chrome > Menü > Zum Startbildschirm hinzufügen / App installieren. Die bestehende firebase-config.js beibehalten, falls bereits korrekt eingerichtet. Keine Firestore-Regeländerung nötig.


## Version 17
Die All-Time-Statistik ist ausschließlich auf der Startseite sichtbar. Die Excel-Einmalimport-Oberfläche wurde entfernt. Bereits importierte Ergebnisse bleiben erhalten. Die Firebase-Regeln aus Version 16 können unverändert bleiben.
