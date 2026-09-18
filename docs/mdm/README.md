# MDM

## Überblick

MDM-Server werden über `mdmServers` verwaltet. Geräte werden über `orgDeviceActivities` zugewiesen oder freigegeben.

## Wichtige Endpunkte

```powershell
Invoke-AppleBusinessApi -Path 'mdmServers'
Invoke-AppleBusinessApi -Path 'mdmServers/<id>'
Invoke-AppleBusinessApi -Path 'orgDeviceActivities'
Invoke-AppleBusinessApi -Path 'orgDeviceActivities/<id>'
```

## Beispiel: MDM-Server auflisten

```powershell
Invoke-AppleBusinessApi -Path 'mdmServers' |
    Select-Object -Property id -ExpandProperty attributes |
    Format-Table -Property id, serverName, serverType, status, deviceCount
```

## Beispiel: Geräte zuweisen

```powershell
$mdmServerId = '<MDM-Server-ID>'
$serialNumberList = @('<Seriennummer 1>', '<Seriennummer 2>')

$activityBody = @{
    data = @{
        type = 'orgDeviceActivities'
        attributes = @{ activityType = 'ASSIGN_DEVICES' }
        relationships = @{
            mdmServer = @{ data = @{ type = 'mdmServers'; id = $mdmServerId } }
            devices = @{ data = @($serialNumberList | ForEach-Object { @{ type = 'orgDevices'; id = $_ } }) }
        }
    }
}

$deviceActivity = Invoke-AppleBusinessApi -Path 'orgDeviceActivities' -Method Post -Body $activityBody
Invoke-AppleBusinessApi -Path "orgDeviceActivities/$($deviceActivity.id)"
```
