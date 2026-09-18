# Devices

## Geräteübersicht

`orgDevices` enthält alle Geräte der Organisation aus der automatischen Geräteregistrierung. `mdmDevices` enthält nur Geräte aus der integrierten Geräteverwaltung.

## Wichtige Endpunkte

```powershell
Invoke-AppleBusinessApi -Path 'orgDevices'
Invoke-AppleBusinessApi -Path 'orgDevices/<Seriennummer>'
Invoke-AppleBusinessApi -Path 'orgDevices/<Seriennummer>/appleCareCoverage'
Invoke-AppleBusinessApi -Path 'orgDevices/<Seriennummer>/activationLockStatus'
Invoke-AppleBusinessApi -Path 'mdmDevices'
Invoke-AppleBusinessApi -Path 'mdmDevices/<id>/details'
```

## Beispiel

```powershell
$deviceList = Invoke-AppleBusinessApi -Path 'orgDevices?limit=1000'
$deviceList |
    Select-Object -ExpandProperty attributes |
    Select-Object -Property serialNumber, deviceModel, productFamily, status, addedToOrgDateTime |
    Format-Table
```

## Bewertete Felder

- `serialNumber`
- `deviceModel`
- `productFamily`
- `status`
- `imei`
- `meid`
- `wifiMacAddress`
- `bluetoothMacAddress`
- `ethernetMacAddress`
