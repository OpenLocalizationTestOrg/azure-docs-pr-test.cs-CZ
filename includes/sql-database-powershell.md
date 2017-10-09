
## <a name="start-your-powershell-session"></a>Spuštění relace prostředí PowerShell
By měl mít nejprve hello nejnovější Azure PowerShell nainstalovaná a spuštěná. Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Databáze SQL, mnoha nových funkcí podporují jenom při použití hello [modelu nasazení Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md), takže příklady použití hello [rutiny prostředí PowerShell databáze SQL Azure](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) pro Resource Manager . model nasazení správy (klasické) služby Hello [rutiny pro správu služby databáze SQL Azure](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) jsou podporovány pro zpětnou kompatibilitu, ale doporučujeme používat rutiny Resource Manager hello.
> 
> 

Spustit hello [ **Add-AzureRmAccount** ](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) rutinu a zobrazí se přihlašovací obrazovka tooenter přihlašovacích údajů. Použití hello stejné přihlašovací údaje použijete toosign v toohello portálu Azure.

```PowerShell
Add-AzureRmAccount
```

Pokud máte více předplatných, použijte hello [ **Set-AzureRmContext** ](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) tooselect rutiny, které předplatné má relace prostředí PowerShell použít. Spustit toosee hello aktuální prostředí PowerShell relace používá, jaké předplatné [ **Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx). toosee Všechna předplatná, spusťte [ **Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx).

```PowerShell
Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```
