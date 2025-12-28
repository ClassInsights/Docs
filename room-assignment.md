Hier finden Sie die verschiedenen Möglichkeiten, um einzelne Computer mit ihren WebUntis Räumen zu verknüpfen.

## Synchronisierung mit den Organisationseinheiten (empfohlen)

?> Wenn Sie ein gepflegtes Active Directory haben, bei denen die einzelnen Computerräume in verschiedenen Organisationseinheiten aufgeteilt wurden, ist dies definitiv die beste Variante.

### Konfiguration eines AD-Benutzers

- Navigieren Sie auf dem lokalem Dashboard zum Reiter **Raumverwaltung** und klicken Sie auf den Button **Active Directory Integration**.
- Nun tragen Sie im Feld FQDN Ihren Domainnamen ein (bspw. `ihreSchule.lokal`)
- In den Feldern Benutzername und Passwort tragen Sie bitte Zugangsdaten eines Domänenbenutzers ein (dieser muss und sollte bestenfalls keine besonderen Berechtigungen haben)

Mit einem Klick auf **Verbindung testen und speichern** sehen Sie direkt, ob alles funktioniert. Nun wählen Sie bestenfalls in der Kategorie **Automatische Raumzuweisung** die Option 2.
Dadurch werden Änderungen die Sie in Ihrer AD-Organisationsstruktur vornehmen synchronisiert.

### Verknüpfung der Organisationseinheiten

- Gehen Sie zurück in die **Raumverwaltung**
- Nun können Sie für jeden WebUntis Raum mit dem Button **Organisationseinheit auswählen** eine OU auswählen.

Fertig! Nun werden alle Computer automatisch in die Räume anhand ihrer Organisationseinheit sortiert.

## Manuelle Raumzuweisung

Sollten Sie kein gepflegtes Active Directory haben, müssen Sie die Raumzuweisung leider per Hand durchführen.
Um dies ebenfalls so einfach wie möglich zu gestalten, haben wir hierfür ein paar Extras in unsere Filter und Suchfunktionen eingebaut.

- Navigieren Sie auf dem lokalem Dashboard zum Reiter **Computerliste**
- Klicken Sie auf den Filter **Name**
- Geben Sie ein Namenspattern ein, dass möglichst viele Computer in einem Raum erfüllen (bspw. `DV1*` um alle Computer zu erhalten die mit `DV1` beginnen)
- Wählen Sie nun alle Computer aus die in einen Raum sollen
- Mit rechter Maustase → **Raum zuweisen** können Sie den WebUntis Raum auswählen zudem die Computer gehören

Nachdem Sie alle Computer zugewiesen haben, sollte ClassInsights wie gewünscht funktionieren!
