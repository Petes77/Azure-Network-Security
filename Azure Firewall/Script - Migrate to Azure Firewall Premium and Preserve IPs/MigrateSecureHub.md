## Migrate to Azure Firewall Premium in Secured vWAN hub with preserved Public IP addresses  

For detailed step by step, visit the TechCommunity blogpost published [here](https://techcommunity.microsoft.com/t5/azure-network-security/migrate-to-azure-firewall-premium-in-secured-vwan-hub-with/ba-p/2922140)  

<p align="center">
 <img src="https://github.com/Azure/Azure-Network-Security/blob/master/Cross%20Product/MediaFiles/Cross-Product/firewall-secure-hub.png"> 
</p>  


Follow these steps to successfully migrate Azure Firewall in your secure virtual hub while preserving the Public IP addresses already assigned to the Azure Firewall during migration. A schedule down-time should be planned for this migration.  


Note: Minimum PowerShell Version Supported: [PowerShell Gallery | Az 6.5.0](https://www.powershellgallery.com/packages/Az/6.5.0)  


```
##Get Firewall resource object
$azfw = Get-AzFirewall -Name "XXXXFW_Hub” -ResourceGroupName "XXXXRG"

## Store Virtual Hub object in a variable
$hub = Get-AzVirtualHub -ResourceGroupName "XXXXRG" -Name "XXXHub”

## De-allocate the Firewall.
$azfw.Deallocate()

## Set the Firewall. This may take a few minutes 
Set-AzFirewall -AzureFirewall $azfw

##Get the Firewall resource object
$azfw = Get-AzFirewall -Name "XXXXFW_Hub” -ResourceGroupName "XXXXRG"

##select new Firewall SKU
$azfw.Sku.Tier="Premium"

##pass the hub information
$azfw.Allocate($hub.id)

##Re-allocate the Firewall
Set-AzFirewall -AzureFirewall $azfw
```  

When the deployment completes, confirm you now have Premium Firewall SKU and the Public IP addresses are available. You can now configure all the additional Azure Firewall Premium features.