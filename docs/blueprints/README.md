# Blueprints

## Überblick

Blueprints verbinden Mitglieder mit Inhalten. Sie steuern, welche Geräte, Benutzer und Konfigurationen zusammengehören.

## Wichtige Endpunkte

```powershell
Invoke-AppleBusinessApi -Path 'blueprints'
Invoke-AppleBusinessApi -Path 'blueprints/<id>'
Invoke-AppleBusinessApi -Path 'blueprints/<id>/relationships/users'
Invoke-AppleBusinessApi -Path 'blueprints/<id>/relationships/configurations'
```

## Beispiel: Blueprint anlegen

```powershell
$testUserId = '<Benutzer-ID>'
$testConfigurationId = '<Konfigurations-ID>'

$blueprintBody = @{
    data = @{
        type = 'blueprints'
        attributes = @{
            name = 'Test-Blueprint API'
            description = 'Per API angelegt'
        }
        relationships = @{
            users = @{ data = @(@{ type = 'users'; id = $testUserId }) }
            configurations = @{ data = @(@{ type = 'configurations'; id = $testConfigurationId }) }
        }
    }
}

$newBlueprint = Invoke-AppleBusinessApi -Path 'blueprints' -Method Post -Body $blueprintBody
$newBlueprint.id
```

## Wichtige Hinweise

- Ein Blueprint braucht mindestens ein Mitglied und einen Inhalt.
- Änderungen greifen sofort auf betroffene Geräte.
- Test- und Pilot-Umgebungen verwenden.
