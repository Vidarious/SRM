# PowerShell / SharePoint Reference Material

A SharePoint [C# / ASP.NET / JavaScript] reference guide for developers.

---

**TABLE OF CONTENTS**
 
* [PowerShell for Anything and Everything](#PowerShellEverything) 
* [Load SharePoint](#LoadSharePoint) 

### <a name="PowerShellEverything"></a>PowerShell for Anything and Everything

Everything and anything for SharePoint can be done via PowerShell. The interface, such as Central Administration simply leverages the same commands that can be used in PowerShell to accomplish the same tasks.

### <a name="LoadSharePoint"></a>Load SharePoint

In order to run SharePoint PowerShell commands you must initialize the SharePoint Snapin. The following script will check to ensure the SharePoint Snapin is not already loaded, then load it and return a acknowledgement.

```cs
$SPAddin = "Microsoft.SharePoint.PowerShell"

If (!(Get-PSSnapin | Where-Object –Property “Name” -eq –Value $SPAddin)) 
{
  Add-PSSnapin $SPAddin
  Write-Host "`nSharePoint Snapin has been initialized .." -ForegroundColor "Green"
}
else
{
  Write-Host "`nSharePoint Snapin has already been initialized .." -ForegroundColor "Green"
}
```
