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

## Get a list of AD Domain Controllers hostnames

    ForEach ($dc in Get-ADDomainController -Filter {Name -like "*"}) { $dc.HostName }

## Retrieve web content and save to a file

    Invoke-WebRequest https://api.github.com/users/alistairmcmillan -OutputFile response.json

## Search a folder for names containing a particular string, within a date range

    $startDate = Get-Date -Date "2023-02-27 00:00:00Z"
    $endDate = Get-Date -Date "2023-02-28 00:00:00Z"
    $searchTerm = "gram"

    Get-ChildItem . -Recurse | Where-Object {
        $_.LastWriteTime -gt $startDate -and 
        $_.LastWriteTime -lt $endDate -and
        $_.name -like "*$searchTerm*" 
    }
