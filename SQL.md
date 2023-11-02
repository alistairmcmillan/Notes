# SQL Notes

## Rename table

`EXEC sp_rename 'Status', 'SCStatus';`

## Get today's date and time

`SELECT GETDATE()`

## Get today's date

`SELECT DATEADD(dd, DATEDIFF(dd,0,GETDATE()), 0)`
