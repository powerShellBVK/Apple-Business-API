# 2. API-Befehle und Grundlagen

## Grundlagen für jede Abfrage

Jedes Objekt der API hat dieselbe Form: `id`, `type`, `attributes` und meist `relationships`. Die eigentlichen Daten stehen in `attributes`.

```powershell
$deviceList = Invoke-AppleBusinessApi -Path 'orgDevices'
$deviceList[0].id
$deviceList[0].attributes.deviceModel
```

Attribute als flache Tabelle, die ID bleibt als Spalte erhalten:

```powershell
Invoke-AppleBusinessApi -Path 'users' |
    Select-Object -Property id -ExpandProperty attributes |
    Format-Table
```

### Query-Parameter

| Parameter | Wirkung | Beispiel für `-Path` |
| --- | --- | --- |
| `limit` | Objekte pro Seite. Standard ist 100, Maximum 1000. | `orgDevices?limit=1000` |
| `fields[<typ>]` | Liefert nur die genannten Attribute. | `orgDevices?fields%5BorgDevices%5D=serialNumber,status` |
| `filter[...]` | Nur beim Audit-Log, dort Pflicht. | siehe Abschnitt 9 |

- **Paging:** Das Modul holt alle Seiten selbst. Ein hohes `limit` spart nur Aufrufe.
- **URL-Kodierung:** Eckige Klammern als `%5B` und `%5D` schreiben. Werte mit Doppelpunkt über `[uri]::EscapeDataString()` kodieren.
- **Relationship-Pfade:** Alles unter `.../relationships/<typ>` liefert nur `type` und `id`, keine Attribute.
- **Apple-Beispiel mit Paging-Feldern:** [Get Organization Devices](https://developer.apple.com/documentation/applebusinessapi/get-org-devices). Die Query-Parameter stehen nur in der JavaScript-Ansicht der Seite.

### Aufbau eines schreibenden Aufrufs

`Post` und `Patch` erwarten immer dieselbe Hülle. Bei `Patch` gehört zusätzlich die `id` hinein.

```powershell
$requestBody = @{
    data = @{
        type          = '<typ>'
        attributes    = @{ }
        relationships = @{ }
    }
}
Invoke-AppleBusinessApi -Path '<typ>' -Method Post -Body $requestBody
```

## Standardmuster

- `Get` für Lesezugriffe
- `Post` für Erstellung
- `Patch` für Aktualisierung
- `Delete` für Entfernung bzw. Freigabe
- `data`-Objekte mit `type`, `attributes` und optional `relationships`

## Nächster Schritt

Weiter zu [Geräte und AppleCare](03-geraete-und-applecare.md).
