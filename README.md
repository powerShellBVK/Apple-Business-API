# Apple Business API per PowerShell

Diese Sammlung dokumentiert die Apple Business API mit PowerShell-Beispielen und ist bewusst nach Themenbereichen aufgeteilt, statt alles in einer Datei zu verwalten.

## Inhaltsverzeichnis

- [1. Schnellstart](docs/01-quickstart.md)
- [2. API-Befehle und Grundlagen](docs/02-api-befehle-und-grundlagen.md)
- [3. Geräte und AppleCare](docs/03-geraete-und-applecare.md)
- [4. MDM-Server und Gerätezuweisung](docs/04-mdm-server-und-geraetezuweisung.md)
- [5. Benutzer, Gruppen und Organisationseinheiten](docs/05-benutzer-gruppen-und-organisationseinheiten.md)
- [6. Apps, Pakete und Konfigurationen](docs/06-apps-pakete-und-konfigurationen.md)
- [7. Blueprints](docs/07-blueprints.md)
- [8. Audit-Log](docs/08-audit-log.md)
- [9. Rezepte](docs/09-rezepte.md)

## Überblick

Jede Sitzung beginnt mit `Connect-AppleBusiness`. Danach läuft jede Abfrage über `Invoke-AppleBusinessApi -Path '<Endpunkt>'`.

```powershell
Connect-AppleBusiness -Verbose
Invoke-AppleBusinessApi -Path 'orgDevices'
```

- PowerShell 7 (`pwsh`) direkt starten, nicht aus einem 5.1-Fenster heraus.
- Adminrechte sind nicht nötig.
- Anmeldung erfolgt über Client ID, Key ID und Secret-Name aus `$PROFILE`.
- Token gilt 1 Stunde; das Modul erneuert es 60 Sekunden vor Ablauf und einmalig nach einem 401.
- Basispfad: `https://api-business.apple.com/v1`

## Wichtigste Befehle

```powershell
Connect-AppleBusiness -ClientId <string> -KeyId <string> -SecretName <string> [-Verbose]
Invoke-AppleBusinessApi -Path <string> [-Method <Get|Post|Patch|Delete>] [-Body <object>]
```

## Themenübersicht

- [Schnellstart](docs/01-quickstart.md)
- [API-Befehle und Abfrage-Grundlagen](docs/02-api-befehle-und-grundlagen.md)
- [Geräte und AppleCare](docs/03-geraete-und-applecare.md)
- [MDM-Server](docs/04-mdm-server-und-geraetezuweisung.md)
- [Benutzer und Gruppen](docs/05-benutzer-gruppen-und-organisationseinheiten.md)
- [Apps, Pakete, Konfigurationen](docs/06-apps-pakete-und-konfigurationen.md)
- [Blueprints](docs/07-blueprints.md)
- [Audit-Log](docs/08-audit-log.md)
- [Rezepte](docs/09-rezepte.md)

## Hinweis

Die Inhalte basieren auf der Apple Business API-Befehlsreferenz aus dem Original-MD und wurden nach fachlichen Themen sortiert.
