# PowerShell Notes

## Get a sorted list of groups for a specific AD user

    ForEach ($group in Get-ADPrincipalGroupMembership (Get-ADUser picardjp).DistinguishedName | Sort-Object -Property Name) { $group.name }

## Get a sorted list of groups for a specific AD computer

    ForEach ($group in Get-ADPrincipalGroupMembership (Get-ADComputer desktop-ncc1701d).DistinguishedName | Sort-Object -Property Name) { $group.name }
