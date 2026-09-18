# 5. Benutzer, Gruppen und Organisationseinheiten

Diese drei Bereiche sind per API nur lesbar. Anlegen und Ändern geht weiterhin nur im Portal oder über die Verzeichnissynchronisierung.

| Zweck | Methode | `-Path` |
| --- | --- | --- |
| [Alle Benutzer](https://developer.apple.com/documentation/applebusinessapi/get-users) | Get | `users` |
| Ein Benutzer | Get | `users/<id>` |
| Alle Benutzergruppen | Get | `userGroups` |
| Eine Benutzergruppe | Get | `userGroups/<id>` |
| Mitglieder einer Gruppe, nur IDs | Get | `userGroups/<id>/relationships/users` |
| Alle Organisationseinheiten (seit API 2.2) | Get | `organizationalUnits` |
| Eine Organisationseinheit | Get | `organizationalUnits/<id>` |
| Benutzer einer Organisationseinheit, nur IDs | Get | `organizationalUnits/<id>/relationships/users` |

Benutzerliste als Tabelle:

```powershell
$userList = Invoke-AppleBusinessApi -Path 'users?limit=1000'
$userList |
    Select-Object -Property id -ExpandProperty attributes |
    Format-Table -Property id, firstName, lastName, managedAppleAccount, status, department
```

Einen Benutzer über seinen verwalteten Apple Account finden:

```powershell
$userList | Where-Object -FilterScript { $_.attributes.managedAppleAccount -like '*nachname*' }
```

Mitglieder einer Gruppe mit Namen statt nur IDs:

```powershell
$groupId = '<Gruppen-ID>'
$memberIdList = (Invoke-AppleBusinessApi -Path "userGroups/$groupId/relationships/users").id
$userList |
    Where-Object -FilterScript { $_.id -in $memberIdList } |
    Select-Object -Property id -ExpandProperty attributes |
    Format-Table -Property id, firstName, lastName, managedAppleAccount
```

- **Benutzer-Attribute:** `firstName`, `middleName`, `lastName`, `managedAppleAccount`, `email`, `status`, `isExternalUser`, `employeeNumber`, `costCenter`, `division`, `department`, `jobTitle`, `phoneNumbers`, `startDateTime`, `createdDateTime`, `updatedDateTime`.
- **Rollen:** `roleOuList` enthält je Eintrag `roleName` und `ouId`. Die `ouId` löst du über `organizationalUnits/<id>` auf.
- **Gruppen und Organisationseinheiten:** Die Attributnamen zeigt dir `Invoke-AppleBusinessApi -Path 'userGroups' | Select-Object -First 1 | ConvertTo-Json -Depth 10`.

## Nächster Schritt

Weiter zu [Apps, Pakete und Konfigurationen](06-apps-pakete-und-konfigurationen.md).
