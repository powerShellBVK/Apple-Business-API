# Repository-Übersicht

Dieses Repository bündelt die wichtigsten PowerShell-Beispiele und Anleitungen für die Apple Business API in einer übersichtlichen, modularen Struktur.

## Themenbereiche

- **Schnellstart**: Login, Token und erste Abfragen
- **API-Befehle und Grundlagen**: Struktur von Antworten, Query-Parameter und allgemeine Regeln
- **Geräte und AppleCare**: `orgDevices`, `mdmDevices`, AppleCare und Aktivierungssperre
- **MDM-Server und Gerätezuweisung**: Serverpflege, Zuordnungen und Aktivitäts-Workflows
- **Benutzer, Gruppen und Organisationseinheiten**: Lesende Auswertung von Benutzern und Strukturen
- **Apps, Pakete und Konfigurationen**: lizenzierten Inhalte und `CUSTOM_SETTING`-Profile
- **Blueprints**: Verknüpfung von Mitgliedern mit Inhalten
- **Audit-Log**: Ereignisabfragen über Zeitfenster
- **Rezepte**: CSV- und JSON-Export, praktische Automatisierungsbeispiele

## Typische Nutzung

```powershell
Connect-AppleBusiness -Verbose

# Geräte abrufen
Invoke-AppleBusinessApi -Path 'orgDevices?limit=1000'

# Benutzer abrufen
Invoke-AppleBusinessApi -Path 'users?limit=1000'

# Blueprint prüfen
Invoke-AppleBusinessApi -Path 'blueprints'
```

## Empfehlung

Die Inhalte sind bewusst nicht in einer einzigen Datei zusammengefasst, sondern in separate Dokumente nach Fachbereich gruppiert. Das erleichtert Pflege, Suche und spätere Erweiterungen.
