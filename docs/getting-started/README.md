# Getting Started

## Überblick

Die Apple Business API wird per PowerShell über zwei zentrale Befehle genutzt:

```powershell
Connect-AppleBusiness -Verbose
Invoke-AppleBusinessApi -Path 'orgDevices'
```

## Grundprinzipien

- `Connect-AppleBusiness` authentifiziert die Sitzung.
- `Invoke-AppleBusinessApi` führt alle API-Aufrufe aus.
- Alle Endpunkte sind relativ zu `https://api-business.apple.com/v1`.
- Das Token läuft nach 1 Stunde und wird automatisch erneuert.

## Parameter

```powershell
Connect-AppleBusiness -ClientId <string> -KeyId <string> -SecretName <string> [-Verbose]
Invoke-AppleBusinessApi -Path <string> [-Method <Get|Post|Patch|Delete>] [-Body <object>]
```

## Wichtige Regeln

- Query-Parameter mit URL-Kodierung versehen
- Zeitstempel als ISO-8601 UTC senden
- `data`-Antworten direkt auslesen
- Paging über `links.next` automatisch verarbeiten

## Beispiel

```powershell
Connect-AppleBusiness
$devices = Invoke-AppleBusinessApi -Path 'orgDevices?limit=1000'
$devices | Select-Object -ExpandProperty attributes | Format-Table
```
