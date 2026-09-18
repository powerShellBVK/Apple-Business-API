# 1. Schnellstart

Jede Sitzung beginnt mit `Connect-AppleBusiness`. Danach läuft jede Abfrage über `Invoke-AppleBusinessApi -Path '<Endpunkt>'`.

```powershell
Connect-AppleBusiness -Verbose
Invoke-AppleBusinessApi -Path 'orgDevices'
```

- **Fenster:** PowerShell 7 (`pwsh`) direkt starten, nicht aus einem 5.1-Fenster heraus. Adminrechte sind nicht nötig.
- **Anmeldung:** Client ID, Key ID und Secret-Name kommen aus `$PROFILE`. Ohne Profilwerte fragt der Befehl sie ab.
- **Token:** Es gilt 1 Stunde. Das Modul erneuert es 60 s vor Ablauf und einmalig nach einem 401.
- **SecretStore:** Er sperrt sich nach 15 Minuten. Bei der nächsten Token-Erneuerung fragt er das Store-Passwort ab.
- **Basis:** Alle Pfade sind relativ zu `https://api-business.apple.com/v1`.

## Die zwei Befehle

Das Modul `AppleBusiness` hat genau zwei Befehle: einen für die Anmeldung und einen für alle API-Aufrufe.

### Connect-AppleBusiness

```powershell
Connect-AppleBusiness -ClientId <string> -KeyId <string> -SecretName <string> [-Verbose]
```

| Parameter | Bedeutung |
| --- | --- |
| `-ClientId` | Client ID des API-Accounts. Sie muss mit `BUSINESSAPI.` beginnen. |
| `-KeyId` | Key ID des API-Accounts. |
| `-SecretName` | Name des Secrets im SecretStore, das den Inhalt der `.pem`-Datei enthält. |
| `-Verbose` | Zeigt nach der Anmeldung, bis wann das Token gilt. |

Der Befehl gibt nichts zurück. Schlägt der Token-Abruf fehl, wirft er einen Fehler und die Sitzung bleibt unverbunden.

### Invoke-AppleBusinessApi

```powershell
Invoke-AppleBusinessApi -Path <string> [-Method <Get|Post|Patch|Delete>] [-Body <object>]
```

| Parameter | Bedeutung |
| --- | --- |
| `-Path` | Endpunkt relativ zu `/v1`, mit oder ohne Query-String. Ein Doppelpunkt ist nicht erlaubt, Zeitstempel daher URL-kodieren. |
| `-Method` | HTTP-Methode. Standard ist `Get`. |
| `-Body` | Hashtable oder Objekt für `Post` und `Patch`. Das Modul sendet es als JSON mit Tiefe 10. |

- **Rückgabe:** Enthält die Antwort ein Feld `data`, gibt der Befehl nur dessen Inhalt aus. Sonst gibt er die ganze Antwort aus.
- **Paging:** Der Befehl folgt `links.next` selbst und liefert alle Seiten als einen Strom von Objekten.
- **Einschränkung:** `meta`, `links` und `included` der Antwort gehen dabei verloren.
- **Delete:** Eine erfolgreiche Löschung liefert keine Ausgabe.

## Nächster Schritt

Weiter zu [API-Befehle und Grundlagen](02-api-befehle-und-grundlagen.md).
