# Users

## Überblick

Benutzer, Gruppen und Organisationseinheiten sind überwiegend lesbar. Die API liefert sie über `users`, `userGroups` und `organizationalUnits`.

## Wichtige Endpunkte

```powershell
Invoke-AppleBusinessApi -Path 'users'
Invoke-AppleBusinessApi -Path 'users/<id>'
Invoke-AppleBusinessApi -Path 'userGroups'
Invoke-AppleBusinessApi -Path 'userGroups/<id>'
Invoke-AppleBusinessApi -Path 'organizationalUnits'
Invoke-AppleBusinessApi -Path 'organizationalUnits/<id>'
```

## Beispiel

```powershell
$userList = Invoke-AppleBusinessApi -Path 'users?limit=1000'
$userList |
    Select-Object -Property id -ExpandProperty attributes |
    Format-Table -Property id, firstName, lastName, managedAppleAccount, status, department
```

## Wichtige Felder

- `firstName`
- `lastName`
- `managedAppleAccount`
- `email`
- `status`
- `department`
- `costCenter`
- `jobTitle`
