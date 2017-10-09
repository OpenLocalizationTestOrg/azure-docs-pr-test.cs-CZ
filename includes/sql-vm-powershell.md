
## <a name="start-your-powershell-session"></a>Spuštění relace prostředí PowerShell
Je třeba nejprve toohave hello nejnovější [prostředí Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) nainstalovaná a spuštěná. Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Příklady v tomto tématu Hello [modelu nasazení Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md), takže příklady použití hello [rutiny Azure Resource Manager](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Spustit hello [ **Add-AzureRmAccount** ](http://msdn.microsoft.com/library/mt619267.aspx) rutiny a zobrazí se přihlašovací obrazovka tooenter přihlašovacích údajů. Použití hello stejné přihlašovací údaje použijete toosign v toohello portálu Azure.

    Add-AzureRmAccount

Pokud máte více předplatných použijte hello [ **Set-AzureRmContext** ](http://msdn.microsoft.com/library/mt619263.aspx) tooselect rutiny, které předplatné má relace prostředí PowerShell použít. Spustit toosee hello aktuální prostředí PowerShell relace používá, jaké předplatné [ **Get-AzureRmContext**](http://msdn.microsoft.com/library/mt619265.aspx). toosee Všechna předplatná, spusťte [ **Get-AzureRmSubscription**](http://msdn.microsoft.com/library/mt619284.aspx).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

