# Recipes

## Überblick

Die Recipes-Sektion enthält wiederverwendbare PowerShell-Workflows für Export, Sicherung und schnelle Ad-hoc-Abfragen.

## Beispiel: Geräte als CSV exportieren

```powershell
Invoke-AppleBusinessApi -Path 'orgDevices?limit=1000' |
    Select-Object -ExpandProperty attributes |
    Select-Object -Property serialNumber, deviceModel, productFamily, status, orderNumber, addedToOrgDateTime,
        @{ Name = 'imei'; Expression = { $_.imei -join ', ' } } |
    Export-Csv -Path "$HOME\Desktop\apple-geraete.csv" -NoTypeInformation -Encoding utf8BOM -Delimiter ';'
```

## Beispiel: Alles als JSON sichern

```powershell
$exportFolder = Join-Path -Path $HOME -ChildPath 'Desktop\apple-export'
New-Item -Path $exportFolder -ItemType Directory -Force | Out-Null

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
