# 8. Audit-Log

Das Audit-Log hat genau einen Endpunkt, `auditEvents`. Er verlangt immer einen Zeitraum über `filter[startTimestamp]` und `filter[endTimestamp]`.

| Zweck | Methode | `-Path` |
| --- | --- | --- |
| [Ereignisse eines Zeitraums](https://developer.apple.com/documentation/applebusinessapi/get-audit-events) | Get | `auditEvents?filter%5BstartTimestamp%5D=<Start>&filter%5BendTimestamp%5D=<Ende>` |

Ereignisse der letzten 7 Tage:

```powershell
#[!] Zeitraum — Tage rückwärts ab jetzt
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

Welche Ereignistypen im Zeitraum vorkommen:

```powershell
$auditEventList |
    Group-Object -Property { $_.attributes.type } |
    Sort-Object -Property Count -Descending |
    Format-Table -Property Count, Name
```

Details zu einem Ereignis. Der Name des Detailfelds steht in `eventDataPropertyKey`:

```powershell
$auditEvent = $auditEventList[0].attributes
$auditEvent.($auditEvent.eventDataPropertyKey)
```

- **Zeitformat:** UTC als `2026-09-18T00:00:00Z`. Der Doppelpunkt muss kodiert sein, weil `-Path` keinen zulässt.
- **Attribute:** `eventDateTime`, `type`, `category`, `actorType`, `actorId`, `actorName`, `subjectType`, `subjectId`, `subjectName`, `outcome`, `groupId`, `eventDataPropertyKey`.
- **Erfasste Bereiche:** Benutzer- und API-Accounts, Rollen, Geräte (hinzugefügt, zugewiesen, gelöscht, freigegeben), Domains, Konfigurationen, Abos sowie AppleCare- und iCloud-Käufe.
- **Weitere Filter:** Apple nennt den Endpunkt filterbar. Die übrigen Filternamen stehen nur in der JavaScript-Ansicht der Apple-Seite und sind hier nicht verifiziert.

## Nächster Schritt

Weiter zu [Rezepte](09-rezepte.md).
