# 4. MDM-Server und Gerätezuweisung

Geräte hängst du nicht am Gerät selbst um, sondern über eine Aktivität (`orgDeviceActivities`). Apple führt sie asynchron aus, das Ergebnis fragst du danach über die Aktivitäts-ID ab.

| Zweck | Methode | `-Path` |
| --- | --- | --- |
| Alle MDM-Server | Get | `mdmServers` |
| Ein MDM-Server (seit API 2.1) | Get | `mdmServers/<id>` |
| Seriennummern der Geräte eines Servers | Get | `mdmServers/<id>/relationships/devices` |
| [MDM-Server anlegen](https://developer.apple.com/documentation/applebusinessapi/create-an-mdmserver) | Post | `mdmServers` |
| MDM-Server ändern | Patch | `mdmServers/<id>` |
| MDM-Server löschen | Delete | `mdmServers/<id>` |
| [Geräte zuweisen, entfernen, freigeben](https://developer.apple.com/documentation/applebusinessapi/create-an-orgdeviceactivity) | Post | `orgDeviceActivities` |
| Status einer Aktivität | Get | `orgDeviceActivities/<id>` |

MDM-Server auflisten:

```powershell
Invoke-AppleBusinessApi -Path 'mdmServers' |
    Select-Object -Property id -ExpandProperty attributes |
    Format-Table -Property id, serverName, serverType, status, deviceCount
```

Geräte einem MDM-Server zuweisen und den Status prüfen:

```powershell
$mdmServerId = '<MDM-Server-ID>'
$serialNumberList = @('<Seriennummer 1>', '<Seriennummer 2>')

$activityBody = @{
    data = @{
        type          = 'orgDeviceActivities'
        attributes    = @{
            #[!] Aktivitätstyp — siehe Tabelle unten
            activityType = 'ASSIGN_DEVICES'
        }
        relationships = @{
            mdmServer = @{ data = @{ type = 'mdmServers'; id = $mdmServerId } }
            devices   = @{ data = @($serialNumberList | ForEach-Object -Process { @{ type = 'orgDevices'; id = $_ } }) }
        }
    }
}
$deviceActivity = Invoke-AppleBusinessApi -Path 'orgDeviceActivities' -Method Post -Body $activityBody
Invoke-AppleBusinessApi -Path "orgDeviceActivities/$($deviceActivity.id)" | Select-Object -ExpandProperty attributes
```

Die Antwort startet mit `status = IN_PROGRESS` und `subStatus = SUBMITTED`. Frage die Aktivität erneut ab, bis der Status wechselt.

| `activityType` | Wirkung | Nötige Angaben |
| --- | --- | --- |
| `ASSIGN_DEVICES` | Geräte einem MDM-Server zuweisen | `mdmServer`, `devices` |
| `UNASSIGN_DEVICES` | Zuweisung aufheben | `mdmServer`, `devices` |
| `ASSIGN_DEVICES_WITH_MDM_MIGRATION_DEADLINE` | Zuweisen und MDM-Migration mit Frist planen, höchstens 90 Tage voraus | `mdmServer`, `devices`, `activityTypeMetadata.mdmMigrationDeadlineDateTime` |
| `UPDATE_MDM_MIGRATION_DEADLINE` | Frist einer laufenden Migration ändern | `devices`, `activityTypeMetadata.mdmMigrationDeadlineDateTime` |
| `CANCEL_MDM_MIGRATION` | Laufende Migration abbrechen | `devices` |
| `RELEASE_DEVICES` (seit API 2.4) | Geräte aus der Organisation freigeben | `devices` |

**Achtung bei `RELEASE_DEVICES`:** Freigegebene Geräte gehören nicht mehr zur Organisation. Sie verlieren ihre Registrierungszuweisung, fliegen aus der integrierten Verwaltung und aus allen Blueprints. Das lässt sich per API nicht rückgängig machen.

MDM-Server anlegen. Pflicht sind `serverName` und `serverCertificate`:

```powershell
$serverBody = @{
    data = @{
        type       = 'mdmServers'
        attributes = @{
            serverName          = 'Test-MDM'
            serverCertificate   = @{
                name = 'test-mdm.cer'
                data = '<Zertifikat als Base64>'
            }
            enableMdmDisownFlag = $true
        }
    }
}
Invoke-AppleBusinessApi -Path 'mdmServers' -Method Post -Body $serverBody
```

- **Ändern:** `Patch` auf `mdmServers/<id>` mit derselben Hülle plus `id`. Die erlaubten Felder vorher in der Apple-Doku prüfen, sie sind hier nicht verifiziert.
- **Achtung beim Löschen:** `Delete` auf `mdmServers/<id>` löscht den Server. Vorher die Geräte umhängen. Was Apple mit noch zugewiesenen Geräten macht, ist hier nicht verifiziert.

## Nächster Schritt

Weiter zu [Benutzer, Gruppen und Organisationseinheiten](05-benutzer-gruppen-und-organisationseinheiten.md).
