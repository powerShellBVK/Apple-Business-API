# Audit

## Überblick

Das Audit-Log liefert Ereignisse über Zeiträume hinweg und ist besonders wichtig für Nachweise, Änderungen und Sicherheitsüberwachung.

## Wichtige Endpunkte

```powershell
Invoke-AppleBusinessApi -Path "auditEvents?filter%5BstartTimestamp%5D=$startTime&filter%5BendTimestamp%5D=$endTime&limit=1000"
```

## Beispiel

```powershell
$dayCount = 7
$timestampFormat = "yyyy-MM-dd'T'HH:mm:ss'Z'"
$startTime = [uri]::EscapeDataString([datetime]::UtcNow.AddDays(-$dayCount).ToString($timestampFormat))
$endTime = [uri]::EscapeDataString([datetime]::UtcNow.ToString($timestampFormat))

$auditEventList = Invoke-AppleBusinessApi -Path "auditEvents?filter%5BstartTimestamp%5D=$startTime&filter%5BendTimestamp%5D=$endTime&limit=1000"
$auditEventList |
    Select-Object -ExpandProperty attributes |
    Sort-Object -Property eventDateTime -Descending |
    Format-Table -Property eventDateTime, type, actorName, subjectName, outcome
```

## Wichtige Felder

- `eventDateTime`
- `type`
- `category`
- `actorName`
- `subjectName`
- `outcome`
- `eventDataPropertyKey`
