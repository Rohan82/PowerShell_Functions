#Region Function: Update-ProcessIdentity
<#
.SYNOPSIS
	Updates the account a specified service runs under the specified managed account
.DESCRIPTION
	Updates the account a specified service runs under the specified managed account
.EXAMPLE
	Update-ProcessIdentity -ServiceToUpdate $claimsService -ServiceAccount $SPAppPoolAccount
#>
Function global:Update-ProcessIdentity
{
    param
	(
		$serviceToUpdate,
		$ServiceAccount
	)
	
    # Managed Account
    $managedAccount = Get-SPManagedAccount | Where-Object {$_.UserName -eq $($ServiceAccount)}
    if ($managedAccount -eq $null) { Throw " - Managed Account $ServiceAccount not found" }
    if ($serviceToUpdate.Service) {$serviceToUpdate = $serviceToUpdate.Service}
    if ($serviceToUpdate.ProcessIdentity.Username -ne $managedAccount.UserName)
    {
        Write-Host -ForegroundColor White " - Updating $($serviceToUpdate.TypeName) to run as $($managedAccount.UserName)..." -NoNewline
        # Set the Process Identity to our general App Pool Account; otherwise it's set by default to the Farm Account and gives warnings in the Health Analyzer
        $serviceToUpdate.ProcessIdentity.CurrentIdentityType = "SpecificUser"
        $serviceToUpdate.ProcessIdentity.ManagedAccount = $managedAccount
        $serviceToUpdate.ProcessIdentity.Update()
        $serviceToUpdate.ProcessIdentity.Deploy()
        Write-Host -ForegroundColor Green "Done."
    }
    else {Write-Host -ForegroundColor White " - $($serviceToUpdate.TypeName) is already configured to run as $($managedAccount.UserName)."}
}
#EndRegion Function: Update-ProcessIdentity
