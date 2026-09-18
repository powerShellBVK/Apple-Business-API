# Apps & Configurations

## Überblick

Apple Business API unterteilt Apps, Pakete und Konfigurationen in getrennte Bereiche. Konfigurationen können als `CUSTOM_SETTING` mit mobilen Profilen gepflegt werden.

## Wichtige Endpunkte

```powershell
Invoke-AppleBusinessApi -Path 'apps'
Invoke-AppleBusinessApi -Path 'packages'
Invoke-AppleBusinessApi -Path 'configurations'
Invoke-AppleBusinessApi -Path 'configurations/<id>'
```

## Konfiguration anlegen

```powershell
$profilePath = 'C:\Pfad\WLAN.mobileconfig'

$configurationBody = @{
    data = @{
        type = 'configurations'
        attributes = @{
            type = 'CUSTOM_SETTING'
            name = 'WLAN Standort A'
            customSettingsValues = @{
                configurationProfile = Get-Content -Path $profilePath -Raw
                filename = Split-Path -Path $profilePath -Leaf
            }
        }
    }
}

$newConfiguration = Invoke-AppleBusinessApi -Path 'configurations' -Method Post -Body $configurationBody
$newConfiguration.id
```

## Wichtig

- `configurationProfile` ist Pflicht
- `filename` ist optional, aber hilfreich
- Ein `Delete` auf `configurations/<id>` wirkt direkt
- Bevor eine Konfiguration gelöscht wird, prüfen, wo sie in Blueprints verwendet wird
