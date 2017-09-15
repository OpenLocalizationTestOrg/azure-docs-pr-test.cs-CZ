
## <a name="start-your-powershell-session"></a><span data-ttu-id="75110-101">Spuštění relace prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="75110-101">Start your PowerShell session</span></span>
<span data-ttu-id="75110-102">Nejdříve byste měli nainstalovat a zprovoznit nejnovější Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75110-102">First, you should have the latest Azure PowerShell installed and running.</span></span> <span data-ttu-id="75110-103">Podrobné informace najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="75110-103">For detailed information, see [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>

> [!NOTE]
> <span data-ttu-id="75110-104">Podpora řady nových funkcí služby SQL Database je dostupná jen v případě, že používáte [model nasazení Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md), takže se v příkladech používají [rutiny prostředí PowerShell Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) pro Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="75110-104">Many new features of SQL Database are only supported when you are using the [Azure Resource Manager deployment model](../articles/azure-resource-manager/resource-group-overview.md), so examples use the [Azure SQL Database PowerShell cmdlets](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) for Resource Manager.</span></span> <span data-ttu-id="75110-105">[Rutiny služby Azure SQL Database](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) v modelu nasazení správy služby (klasické) podporují zpětnou kompatibilitu, ale přesto doporučujeme používat rutiny Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="75110-105">The service management (classic) deployment model [Azure SQL Database Service Management cmdlets](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) are supported for backward compatibility, but we recommend you use the Resource Manager cmdlets.</span></span>
> 
> 

<span data-ttu-id="75110-106">Po spuštění rutiny [**Add-AzureRmAccount**](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) se zobrazí přihlašovací obrazovka, kam zadáte svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="75110-106">Run the [**Add-AzureRmAccount**](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) cmdlet, and you will be presented with a sign-in screen to enter your credentials.</span></span> <span data-ttu-id="75110-107">Použijte stejné přihlašovací údaje, pomocí kterých se přihlašujete na portál Azure.</span><span class="sxs-lookup"><span data-stu-id="75110-107">Use the same credentials that you use to sign in to the Azure portal.</span></span>

```PowerShell
Add-AzureRmAccount
```

<span data-ttu-id="75110-108">Pokud máte více předplatných, pomocí rutiny [**Set-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) vyberte, které předplatné má relace prostředí PowerShell použít.</span><span class="sxs-lookup"><span data-stu-id="75110-108">If you have multiple subscriptions, use the [**Set-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) cmdlet to select which subscription your PowerShell session should use.</span></span> <span data-ttu-id="75110-109">Pokud chcete zjistit, jaké předplatné používá aktuální relace prostředí PowerShell, spusťte rutinu [**Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx).</span><span class="sxs-lookup"><span data-stu-id="75110-109">To see what subscription the current PowerShell session is using, run [**Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx).</span></span> <span data-ttu-id="75110-110">Pokud chcete zobrazit všechna předplatná, spusťte rutinu [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx).</span><span class="sxs-lookup"><span data-stu-id="75110-110">To see all your subscriptions, run [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx).</span></span>

```PowerShell
Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```