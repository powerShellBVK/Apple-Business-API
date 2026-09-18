# 3. Geräte und AppleCare

`orgDevices` sind alle Geräte der Organisation aus der automatischen Geräteregistrierung. Die ID eines Geräts ist seine Seriennummer. `mdmDevices` sind nur die Geräte in der integrierten Geräteverwaltung von Apple Business.

| Zweck | Methode | `-Path` |
| --- | --- | --- |
| [Alle Geräte](https://developer.apple.com/documentation/applebusinessapi/get-org-devices) | Get | `orgDevices` |
| [Ein Gerät](https://developer.apple.com/documentation/applebusinessapi/get-orgdevice-information) | Get | `orgDevices/<Seriennummer>` |
| AppleCare-Abdeckung | Get | `orgDevices/<Seriennummer>/appleCareCoverage` |
| Status der Aktivierungssperre (seit API 2.5) | Get | `orgDevices/<Seriennummer>/activationLockStatus` |
| Zugewiesener MDM-Server, nur ID | Get | `orgDevices/<Seriennummer>/relationships/assignedServer` |
| Zugewiesener MDM-Server, mit Details | Get | `orgDevices/<Seriennummer>/assignedServer` |
| [Geräte in der integrierten Verwaltung](https://developer.apple.com/documentation/applebusinessapi/get-apple-mdm-enrolled-devices) | Get | `mdmDevices` |
| [Details eines verwalteten Geräts](https://developer.apple.com/documentation/applebusinessapi/get-the-details-for-apple-mdm-enrolled-device) | Get | `mdmDevices/<id>/details` |

Alle Geräte mit den wichtigsten Spalten:

```powershell
$deviceList = Invoke-AppleBusinessApi -Path 'orgDevices?limit=1000'
$deviceList |
    Select-Object -ExpandProperty attributes |
    Select-Object -Property serialNumber, deviceModel, productFamily, status, addedToOrgDateTime |
    Format-Table
```

Ein einzelnes Gerät mit Garantie und Aktivierungssperre:

```powershell
$serialNumber = '<Seriennummer>'
Invoke-AppleBusinessApi -Path "orgDevices/$serialNumber" | Select-Object -ExpandProperty attributes
Invoke-AppleBusinessApi -Path "orgDevices/$serialNumber/appleCareCoverage" | Select-Object -ExpandProperty attributes
Invoke-AppleBusinessApi -Path "orgDevices/$serialNumber/activationLockStatus"
```

Geräte ohne MDM-Zuweisung:

```powershell
# Filter — läuft lokal, die API bietet für Geräte keinen Statusfilter
$deviceList | Where-Object -FilterScript { $_.attributes.status -eq 'UNASSIGNED' }
```

- **Wichtige Attribute:** `serialNumber`, `deviceModel`, `productFamily`, `productType`, `deviceCapacity`, `color`, `status`, `orderNumber`, `orderDateTime`, `addedToOrgDateTime`, `releasedFromOrgDateTime`, `purchaseSourceType`.
- **Netz und Mobilfunk:** `wifiMacAddress`, `bluetoothMacAddress`, `ethernetMacAddress`, `imei`, `meid`, `eid`.
- **MDM-Migration (seit API 2.3):** `isMdmMigrationCapable`, `mdmMigrationStatus`, `mdmMigrationDeadlineDateTime`.
- **Status:** `ASSIGNED` oder `UNASSIGNED`, bezogen auf die Zuweisung zu einem MDM-Server.

## Nächster Schritt

Weiter zu [MDM-Server und Gerätezuweisung](04-mdm-server-und-geraetezuweisung.md).
