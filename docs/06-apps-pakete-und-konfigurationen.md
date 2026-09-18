# 6. Apps, Pakete und Konfigurationen

Apps und Pakete sind nur lesbar. Konfigurationen kannst du per API anlegen und ändern, aber nur vom Typ `CUSTOM_SETTING`, also als eigenes `.mobileconfig`-Profil. Alle drei Bereiche setzen die integrierte Geräteverwaltung von Apple Business voraus.

| Zweck | Methode | `-Path` |
| --- | --- | --- |
| Alle lizenzierten Apps | Get | `apps` |
| Eine App | Get | `apps/<id>` |
| Alle Pakete | Get | `packages` |
| Ein Paket | Get | `packages/<id>` |
| Alle Konfigurationen | Get | `configurations` |
| Eine Konfiguration | Get | `configurations/<id>` |
| [Konfiguration anlegen](https://developer.apple.com/documentation/applebusinessapi/create-a-configuration) | Post | `configurations` |
| [Konfiguration ändern](https://developer.apple.com/documentation/applebusinessapi/update-a-configuration) | Patch | `configurations/<id>` |
| Konfiguration löschen | Delete | `configurations/<id>` |

Apps und Konfigurationen mit ihren IDs auflisten. Die IDs brauchst du für Blueprints:

```powershell
Invoke-AppleBusinessApi -Path 'apps' | Select-Object -Property id -ExpandProperty attributes | Format-Table
Invoke-AppleBusinessApi -Path 'configurations' |
    Select-Object -Property id -ExpandProperty attributes |
    Format-Table -Property id, name, type, configuredForPlatforms
```

Eigene Konfiguration aus einer `.mobileconfig`-Datei anlegen:

```powershell
$profilePath = 'C:\Pfad\WLAN.mobileconfig'
$configurationBody = @{
    data = @{
        type       = 'configurations'
        attributes = @{
            type                 = 'CUSTOM_SETTING'
            name                 = 'WLAN Standort A'
            customSettingsValues = @{
                # Profilinhalt — als XML-Text, nicht Base64
                configurationProfile = Get-Content -Path $profilePath -Raw
                filename             = Split-Path -Path $profilePath -Leaf
            }
        }
    }
}
$newConfiguration = Invoke-AppleBusinessApi -Path 'configurations' -Method Post -Body $configurationBody
$newConfiguration.id
```

Namen einer Konfiguration ändern. Apple ändert nur die mitgeschickten Felder:

```powershell
$configurationId = '<Konfigurations-ID>'
$updateBody = @{
    data = @{
        type       = 'configurations'
        id         = $configurationId
        attributes = @{ name = 'WLAN Standort A (neu)' }
    }
}
Invoke-AppleBusinessApi -Path "configurations/$configurationId" -Method Patch -Body $updateBody
```

- **Pflichtfeld beim Anlegen:** `configurationProfile`. Ohne `filename` vergibt Apple einen Namen aus der ID.
- **Plattformen:** `configuredForPlatforms` ist optional, etwa `PLATFORM_IOS` oder `PLATFORM_MACOS`. Ohne Angabe erkennt Apple die Plattformen aus dem Profilinhalt.
- **Beim Ändern:** Mindestens eines von `name`, `configuredForPlatforms`, `configurationProfile` oder `filename` mitschicken. Ein `filename` muss auf `.mobileconfig` enden.
- **Achtung beim Löschen:** `Delete` auf `configurations/<id>` wirkt sofort. Vorher prüfen, in welchen Blueprints die Konfiguration steckt.

## Nächster Schritt

Weiter zu [Blueprints](07-blueprints.md).
