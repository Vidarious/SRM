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
function Add-SharePoint
{
	Write-Host "`nChecking for SharePoint Snapin ..." -ForegroundColor Cyan -NoNewline;
	
	#Assign module name.
	$snapinName = "Microsoft.SharePoint.PowerShell";
	
	#Check if Snapin is loaded.
	if (Test-ScriptSnapin $snapinName)
	{
		Write-Host " Already Loaded!" -ForegroundColor Green;
	}
	else
	{
		Write-Host " Not Loaded!" -ForegroundColor Red;
		
		Write-Host "`nAttempting to load Snapin ..." -ForegroundColor Cyan -NoNewline;
		
		#Try and load the Snapin.
		if (Add-ScriptSnapin $snapinName)
		{
			Write-Host " Successfully Loaded!" -ForegroundColor Green;
		}
		else
		{
			Write-Host " Failed to Load!" -ForegroundColor Red;
			
			Write-Host "`nEnsure the following conditions have been met ..." -ForegroundColor Red;
			Write-Host "1. SharePoint is installed on this machine." -ForegroundColor Red;
			Write-Host "2. PowerShell is being ran as an Administrator." -ForegroundColor Red;
			Write-Host "3. PowerShell is being ran as the 64x bit edition." -ForegroundColor Red;
			
			Write-Host "`nThis application will now exit ...";
			
			Close-Application;
		}
	}
}
```
