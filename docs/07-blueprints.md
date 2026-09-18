# 7. Blueprints

Ein Blueprint verbindet Mitglieder (Geräte, Benutzer, Gruppen) mit Inhalten (Apps, Pakete, Konfigurationen). Änderungen wirken sofort auf die Geräte der Mitglieder, deshalb nur mit Testbenutzern oder Testgeräten üben.

| Zweck | Methode | `-Path` |
| --- | --- | --- |
| Alle Blueprints | Get | `blueprints` |
| Ein Blueprint | Get | `blueprints/<id>` |
| [Blueprint anlegen](https://developer.apple.com/documentation/applebusinessapi/create-a-blueprint) | Post | `blueprints` |
| [Blueprint ändern](https://developer.apple.com/documentation/applebusinessapi/update-a-blueprint) | Patch | `blueprints/<id>` |
| Blueprint löschen | Delete | `blueprints/<id>` |
| IDs einer Zuordnung lesen | Get | `blueprints/<id>/relationships/<typ>` |
| [Einträge hinzufügen](https://developer.apple.com/documentation/applebusinessapi/add-users-to-a-blueprint) | Post | `blueprints/<id>/relationships/<typ>` |
| [Einträge entfernen](https://developer.apple.com/documentation/applebusinessapi/remove-users-from-a-blueprint) | Delete | `blueprints/<id>/relationships/<typ>` |

Für `<typ>` gilt: `apps`, `configurations`, `packages`, `orgDevices`, `users` oder `userGroups`. Der Typ im Body heißt genauso wie der Pfadteil.

### Auflisten und wiederfinden

```powershell
Invoke-AppleBusinessApi -Path 'blueprints' |
    Select-Object -Property id -ExpandProperty attributes |
    Format-Table -Property id, name, status, appLicenseDeficient, updatedDateTime
```

### Anlegen

Ein Blueprint braucht mindestens ein Mitglied und mindestens einen Inhalt. Sonst antwortet Apple mit 409.

```powershell
$testUserId = '<Benutzer-ID>'
$testConfigurationId = '<Konfigurations-ID>'

$blueprintBody = @{
    data = @{
        type          = 'blueprints'
        attributes    = @{
            name        = 'Test-Blueprint API'
            description = 'Per API angelegt'
        }
        relationships = @{
            users          = @{ data = @(@{ type = 'users'; id = $testUserId }) }
            configurations = @{ data = @(@{ type = 'configurations'; id = $testConfigurationId }) }
        }
    }
}
$newBlueprint = Invoke-AppleBusinessApi -Path 'blueprints' -Method Post -Body $blueprintBody
$newBlueprint.id
```

### Mitglieder und Inhalte nachträglich pflegen

Hinzufügen und Entfernen nutzen denselben Pfad und denselben Body. Nur die Methode unterscheidet sich. Beide antworten bei Erfolg ohne Inhalt (204).

```powershell
$blueprintId = '<Blueprint-ID>'
#[!] Zuordnungstyp — apps, configurations, packages, orgDevices, users oder userGroups
$relationshipType = 'users'
$resourceIdList = @('<ID 1>', '<ID 2>')

$linkageBody = @{
    data = @($resourceIdList | ForEach-Object -Process { @{ type = $relationshipType; id = $_ } })
}

# Hinzufügen
Invoke-AppleBusinessApi -Path "blueprints/$blueprintId/relationships/$relationshipType" -Method Post -Body $linkageBody

# Entfernen
Invoke-AppleBusinessApi -Path "blueprints/$blueprintId/relationships/$relationshipType" -Method Delete -Body $linkageBody

# Kontrolle — liefert nur type und id
Invoke-AppleBusinessApi -Path "blueprints/$blueprintId/relationships/$relationshipType"
```

### Name oder Beschreibung ändern

```powershell
$updateBody = @{
    data = @{
        type       = 'blueprints'
        id         = $blueprintId
        attributes = @{ description = 'Neue Beschreibung' }
    }
}
Invoke-AppleBusinessApi -Path "blueprints/$blueprintId" -Method Patch -Body $updateBody
```

### Löschen

```powershell
Invoke-AppleBusinessApi -Path "blueprints/$blueprintId" -Method Delete
```

**Achtung beim Löschen:** Die Zuweisung entfällt sofort. Die Geräte der Mitglieder verlieren die Apps und Konfigurationen aus diesem Blueprint.

- **Ungültige IDs:** Apple verwirft sie beim Anlegen und Ändern ohne Fehlermeldung. Deshalb IDs vorher gegen die abgerufenen Listen prüfen und danach die Zuordnung kontrollieren.
- **Patch mit `relationships`:** Apple erlaubt das. Ob eine mitgeschickte Liste die bestehende ersetzt oder ergänzt, sagt die Doku nicht. Für gezielte Änderungen die Zuordnungspfade oben nehmen.
- **Lizenzen:** `appLicenseDeficient = True` heißt, für enthaltene Apps fehlen Lizenzen.
- **Geräte zuweisen:** Ein Gerät kommt über die Zuordnung `orgDevices` in den Blueprint. Eine eigene Aktivität wie bei MDM-Servern gibt es dafür nicht.

## Nächster Schritt

Weiter zu [Audit-Log](08-audit-log.md).
