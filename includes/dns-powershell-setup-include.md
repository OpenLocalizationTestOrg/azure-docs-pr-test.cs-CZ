## <a name="set-up-azure-powershell-for-azure-dns"></a><span data-ttu-id="be865-101">Nastavení prostředí Azure PowerShell pro Azure DNS</span><span class="sxs-lookup"><span data-stu-id="be865-101">Set up Azure PowerShell for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="be865-102">Než začnete</span><span class="sxs-lookup"><span data-stu-id="be865-102">Before you begin</span></span>

<span data-ttu-id="be865-103">Ověřte, zda máte následující položky před zahájením konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="be865-103">Verify that you have hello following items before beginning your configuration.</span></span>

* <span data-ttu-id="be865-104">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="be865-104">An Azure subscription.</span></span> <span data-ttu-id="be865-105">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="be865-105">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="be865-106">Je nutné tooinstall hello nejnovější verzi hello rutiny Powershellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="be865-106">You need tooinstall hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="be865-107">Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="be865-107">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>

### <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="be865-108">Přihlaste se tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="be865-108">Sign in tooyour Azure account</span></span>

<span data-ttu-id="be865-109">Otevřete konzolu prostředí PowerShell a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="be865-109">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="be865-110">Další informace najdete v tématu [pomocí prostředí PowerShell s Resource Managerem](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="be865-110">For more information, see [Using PowerShell with Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="select-hello-subscription"></a><span data-ttu-id="be865-111">Vyberte předplatné hello</span><span class="sxs-lookup"><span data-stu-id="be865-111">Select hello subscription</span></span>
 
<span data-ttu-id="be865-112">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="be865-112">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="be865-113">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="be865-113">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="be865-114">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="be865-114">Create a resource group</span></span>

<span data-ttu-id="be865-115">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="be865-115">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="be865-116">Toto umístění se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="be865-116">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="be865-117">Ale vzhledem k tomu, že všechny prostředky DNS jsou globální, a ne místní, hello Volba umístění skupiny prostředků nemá žádný vliv na Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="be865-117">However, because all DNS resources are global, not regional, hello choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="be865-118">Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="be865-118">You can skip this step if you are using an existing resource group.</span></span>

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a><span data-ttu-id="be865-119">Registrace poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="be865-119">Register resource provider</span></span>

<span data-ttu-id="be865-120">Hello služba Azure DNS spravuje poskytovatel prostředků Microsoft.Network hello.</span><span class="sxs-lookup"><span data-stu-id="be865-120">hello Azure DNS service is managed by hello Microsoft.Network resource provider.</span></span> <span data-ttu-id="be865-121">Vaše předplatné Azure musí být registrovaný toouse tohoto poskytovatele prostředků než budete moci použít Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="be865-121">Your Azure subscription must be registered toouse this resource provider before you can use Azure DNS.</span></span> <span data-ttu-id="be865-122">Jedná se o jednorázovou operaci u každého odběru.</span><span class="sxs-lookup"><span data-stu-id="be865-122">This is a one-time operation for each subscription.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```