# PowerShell Notes

## Get distinguished name for a specific AD computer

    (Get-ADComputer desktop-ncc1701d).DistinguishedName

## Get all properties for a specific AD user

    Get-ADUser picardjp -Properties *

## Get all properties for a specific AD computer

    Get-ADComputer desktop-ncc1701d -Properties *

## Get a sorted list of groups for a specific AD user

    ForEach ($group in Get-ADPrincipalGroupMembership (Get-ADUser picardjp).DistinguishedName | Sort-Object -Property Name) { $group.name }

## Get a sorted list of groups for a specific AD computer

    ForEach ($group in Get-ADPrincipalGroupMembership (Get-ADComputer desktop-ncc1701d).DistinguishedName | Sort-Object -Property Name) { $group.name }
