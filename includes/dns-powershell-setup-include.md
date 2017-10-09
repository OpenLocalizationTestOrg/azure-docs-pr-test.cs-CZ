## <a name="set-up-azure-powershell-for-azure-dns"></a>Nastavení prostředí Azure PowerShell pro Azure DNS

### <a name="before-you-begin"></a>Než začnete

Ověřte, zda máte následující položky před zahájením konfigurace hello.

* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
* Je nutné tooinstall hello nejnovější verzi hello rutiny Powershellu pro Azure Resource Manager. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="sign-in-tooyour-azure-account"></a>Přihlaste se tooyour účet Azure

Otevřete konzolu prostředí PowerShell a připojte tooyour účtu. Další informace najdete v tématu [pomocí prostředí PowerShell s Resource Managerem](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-hello-subscription"></a>Vyberte předplatné hello
 
Zkontrolujte předplatná hello pro účet hello.

```powershell
Get-AzureRmSubscription
```

Zvolte, které vaše toouse předplatných Azure.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto umístění se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Ale vzhledem k tomu, že všechny prostředky DNS jsou globální, a ne místní, hello Volba umístění skupiny prostředků nemá žádný vliv na Azure DNS.

Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Registrace poskytovatele prostředků

Hello služba Azure DNS spravuje poskytovatel prostředků Microsoft.Network hello. Vaše předplatné Azure musí být registrovaný toouse tohoto poskytovatele prostředků než budete moci použít Azure DNS. Jedná se o jednorázovou operaci u každého odběru.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```