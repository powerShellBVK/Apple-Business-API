# Apple Business API per PowerShell

Sammlung von PowerShell-Referenzen, Beispielen und Best Practices für die Apple Business API. Das Repository ist bewusst nach fachlichen Themen getrennt, damit Blueprints, Geräte, Apps, MDM-Server und Audit-Log sauber voneinander gepflegt werden können.

## Warum dieses Repository?

Dieses Projekt dient als strukturierte Referenz für Administratoren, IT-Architekten und PowerShell-Autoren, die mit der Apple Business API arbeiten. Es verbindet:

- die wichtigsten API-Endpunkte
- PowerShell-Beispiele für `Connect-AppleBusiness` und `Invoke-AppleBusinessApi`
- praktische Workflows für Geräte, MDM, Benutzer, Blueprints und Konfigurationen
- separate Dokumente je Themenbereich statt einer großen, unübersichtlichen Datei

## Struktur

```text
Apple-Business-API/
├── README.md
├── docs/
│   ├── 00-overview.md
│   ├── 01-quickstart.md
│   ├── 02-api-befehle-und-grundlagen.md
│   ├── 03-geraete-und-applecare.md
│   ├── 04-mdm-server-und-geraetezuweisung.md
│   ├── 05-benutzer-gruppen-und-organisationseinheiten.md
│   ├── 06-apps-pakete-und-konfigurationen.md
│   ├── 07-blueprints.md
│   ├── 08-audit-log.md
│   └── 09-rezepte.md
└── .gitignore
```

## Inhaltsverzeichnis

- [Repository-Übersicht](docs/00-overview.md)
- [1. Schnellstart](docs/01-quickstart.md)
- [2. API-Befehle und Grundlagen](docs/02-api-befehle-und-grundlagen.md)
- [3. Geräte und AppleCare](docs/03-geraete-und-applecare.md)
- [4. MDM-Server und Gerätezuweisung](docs/04-mdm-server-und-geraetezuweisung.md)
- [5. Benutzer, Gruppen und Organisationseinheiten](docs/05-benutzer-gruppen-und-organisationseinheiten.md)
- [6. Apps, Pakete und Konfigurationen](docs/06-apps-pakete-und-konfigurationen.md)
- [7. Blueprints](docs/07-blueprints.md)
- [8. Audit-Log](docs/08-audit-log.md)
- [9. Rezepte](docs/09-rezepte.md)

## Schnellstart

```powershell
Connect-AppleBusiness -Verbose
Invoke-AppleBusinessApi -Path 'orgDevices'
```

Wichtige Punkte:

- PowerShell 7 (`pwsh`) verwenden
- keine Adminrechte nötig
- Token läuft nach 1 Stunde
- Basis-URL: `https://api-business.apple.com/v1`

## Zentrale Befehle

```powershell
Connect-AppleBusiness -ClientId <string> -KeyId <string> -SecretName <string> [-Verbose]
Invoke-AppleBusinessApi -Path <string> [-Method <Get|Post|Patch|Delete>] [-Body <object>]
```

## Themenbereiche

- Geräte und AppleCare
- MDM-Server und Zuweisungen
- Benutzer, Gruppen und Organisationseinheiten
- Apps, Pakete und Konfigurationen
- Blueprints
- Audit-Log
- Export-/Sicherungsrezepte

## Hinweis

Die Inhalte basieren auf der Apple Business API-Befehlsreferenz aus dem Original-MD und wurden nach fachlichen Themen sortiert, damit das Repository leichter wartbar, verständlich und erweiterbar bleibt.
