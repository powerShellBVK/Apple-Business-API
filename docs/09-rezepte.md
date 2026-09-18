# 9. Rezepte

Diese Bausteine setzen eine Anmeldung mit `Connect-AppleBusiness` voraus.

### Geräte als CSV für ein deutsches Excel

Listenfelder wie `imei` müssen vor dem Export zu Text werden, sonst steht `System.Object[]` in der Zelle.

```powershell
Invoke-AppleBusinessApi -Path 'orgDevices?limit=1000' |
    Select-Object -ExpandProperty attributes |
    Select-Object -Property serialNumber, deviceModel, productFamily, status, orderNumber, addedToOrgDateTime,
        @{ Name = 'imei'; Expression = { $_.imei -join ', ' } } |
    Export-Csv -Path "$HOME\Desktop\apple-geraete.csv" -NoTypeInformation -Encoding utf8BOM -Delimiter ';'
```

### Alles als JSON sichern

```powershell
$exportFolder = Join-Path -Path $HOME -ChildPath 'Desktop\apple-export'
New-Item -Path $exportFolder -ItemType Directory -Force | Out-Null

#[!] Endpunktliste — Bereiche ohne Berechtigung werden übersprungen
$endpointList = @('orgDevices', 'mdmDevices', 'mdmServers', 'users', 'userGroups', 'organizationalUnits', 'apps', 'packages', 'configurations', 'blueprints')
foreach ($endpoint in $endpointList) {
    try {
        $resultList = @(Invoke-AppleBusinessApi -Path $endpoint)
        ConvertTo-Json -InputObject $resultList -Depth 10 |
            Set-Content -Path (Join-Path -Path $exportFolder -ChildPath "$endpoint.json") -Encoding utf8
        Write-Host "${endpoint}: $($resultList.Count) Objekte"
    }
    catch {
        Write-Warning "$endpoint übersprungen: $($_.Exception.Message)"
    }
}
```

## Abschluss

Die Themen sind bewusst nach fachlichen Bereichen aufgeteilt und nicht in einer einzigen Datei zusammengeführt. So lassen sich Blueprints, Apps, Geräte, MDM-Server und Audit-Log unabhängig voneinander pflegen und erweitern.
