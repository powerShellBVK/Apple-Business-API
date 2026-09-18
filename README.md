# Apple Business API per PowerShell

Strukturierte PowerShell-Referenz und Beispielsammlung für die Apple Business API. Das Repository ist bewusst nach funktionalen Bereichen aufgeteilt, damit Geräte, MDM, Benutzer, Apps, Blueprints und Audit-Log sauber getrennt und leicht wartbar bleiben.

## Warum diese Struktur?

Die Apple Business API ist groß und fachlich stark gegliedert. Deshalb liegt der Fokus dieses Repositories auf sauber getrennten Themenbereichen statt einer einzigen langen Datei:

- Getting Started
- Devices
- MDM
- Users
- Apps & Configurations
- Blueprints
- Audit
- Recipes

## Quick Links

- [Repository Overview](docs/00-overview.md)
- [Documentation Index](docs/README.md)
- [Quickstart](docs/01-quickstart.md)
- [Devices](docs/devices/README.md)
- [MDM](docs/mdm/README.md)
- [Blueprints](docs/blueprints/README.md)
- [Audit Log](docs/audit/README.md)
- [Recipes](docs/recipes/README.md)

## Repository-Struktur

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
│   ├── 09-rezepte.md
│   ├── getting-started/
│   │   └── README.md
│   ├── devices/
│   │   └── README.md
│   ├── mdm/
│   │   └── README.md
│   ├── users/
│   │   └── README.md
│   ├── apps/
│   │   └── README.md
│   ├── blueprints/
│   │   └── README.md
│   ├── audit/
│   │   └── README.md
│   └── recipes/
│       └── README.md
└── .gitignore
```

## Inhaltsverzeichnis

### Start
- [Repository-Übersicht](docs/00-overview.md)
- [Schnellstart](docs/01-quickstart.md)
- [API-Befehle und Grundlagen](docs/02-api-befehle-und-grundlagen.md)

### Funktionale Bereiche
- [Getting Started](docs/getting-started/README.md)
- [Devices](docs/devices/README.md)
- [MDM](docs/mdm/README.md)
- [Users](docs/users/README.md)
- [Apps & Configurations](docs/apps/README.md)
- [Blueprints](docs/blueprints/README.md)
- [Audit](docs/audit/README.md)
- [Recipes](docs/recipes/README.md)

### Original-Referenzen
- [Geräte und AppleCare](docs/03-geraete-und-applecare.md)
- [MDM-Server und Gerätezuweisung](docs/04-mdm-server-und-geraetezuweisung.md)
- [Benutzer, Gruppen und Organisationseinheiten](docs/05-benutzer-gruppen-und-organisationseinheiten.md)
- [Apps, Pakete und Konfigurationen](docs/06-apps-pakete-und-konfigurationen.md)
- [Blueprints](docs/07-blueprints.md)
- [Audit-Log](docs/08-audit-log.md)
- [Rezepte](docs/09-rezepte.md)

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

## Bereichsübersicht

- **Getting Started**: Anmeldung, erste Abfragen, Grundprinzipien
- **Devices**: Gerätebestand, Seriennummern, AppleCare, Aktivierungssperre
- **MDM**: Server, Gerätezuweisung, Aktivitäts-Status
- **Users**: Benutzer, Gruppen, Organisationseinheiten
- **Apps & Configurations**: App-Lizenzen, Pakete, mobileconfig-Profile
- **Blueprints**: Zuweisungen von Geräten, Benutzern und Inhalten
- **Audit**: Ereignisprotokolle und Filterung
- **Recipes**: CSV- und JSON-Export, repeatable workflows

## Beitrag und Pflege

Beiträge, Korrekturen und neue Beispiele sind willkommen. Bitte halte dich an die vorhandene Gliederung und dokumentiere neue Inhalte in dem passenden Bereich.

## Lizenz

Dieses Projekt ist unter der MIT-Lizenz veröffentlicht. Siehe [LICENSE](LICENSE).

## Hinweis

Die Inhalte basieren auf der Apple Business API-Befehlsreferenz und wurden bewusst in funktionale Gruppen aufgeteilt, damit das Repository sauber, nachvollziehbar und für eine spätere Erweiterung vorbereitet ist.
