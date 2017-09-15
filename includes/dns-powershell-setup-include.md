## <a name="set-up-azure-powershell-for-azure-dns"></a><span data-ttu-id="5995f-101">Nastavení prostředí Azure PowerShell pro Azure DNS</span><span class="sxs-lookup"><span data-stu-id="5995f-101">Set up Azure PowerShell for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="5995f-102">Než začnete</span><span class="sxs-lookup"><span data-stu-id="5995f-102">Before you begin</span></span>

<span data-ttu-id="5995f-103">Před zahájením konfigurace ověřte, zda máte následující.</span><span class="sxs-lookup"><span data-stu-id="5995f-103">Verify that you have the following items before beginning your configuration.</span></span>

* <span data-ttu-id="5995f-104">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5995f-104">An Azure subscription.</span></span> <span data-ttu-id="5995f-105">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5995f-105">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="5995f-106">Musíte nainstalovat nejnovější verzi rutin Powershellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5995f-106">You need to install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="5995f-107">Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="5995f-107">For more information, see [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>

### <a name="sign-in-to-your-azure-account"></a><span data-ttu-id="5995f-108">Přihlášení k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="5995f-108">Sign in to your Azure account</span></span>

<span data-ttu-id="5995f-109">Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="5995f-109">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="5995f-110">Další informace najdete v tématu [pomocí prostředí PowerShell s Resource Managerem](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="5995f-110">For more information, see [Using PowerShell with Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="select-the-subscription"></a><span data-ttu-id="5995f-111">Výběr předplatného</span><span class="sxs-lookup"><span data-stu-id="5995f-111">Select the subscription</span></span>
 
<span data-ttu-id="5995f-112">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="5995f-112">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="5995f-113">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="5995f-113">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="5995f-114">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5995f-114">Create a resource group</span></span>

<span data-ttu-id="5995f-115">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="5995f-115">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="5995f-116">Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="5995f-116">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="5995f-117">Všechny prostředky DNS jsou ale globální, a ne místní, takže volba umístění skupiny prostředků nemá na Azure DNS žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="5995f-117">However, because all DNS resources are global, not regional, the choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="5995f-118">Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="5995f-118">You can skip this step if you are using an existing resource group.</span></span>

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a><span data-ttu-id="5995f-119">Registrace poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="5995f-119">Register resource provider</span></span>

<span data-ttu-id="5995f-120">Službu Azure DNS spravuje poskytovatel prostředků Microsoft.Network.</span><span class="sxs-lookup"><span data-stu-id="5995f-120">The Azure DNS service is managed by the Microsoft.Network resource provider.</span></span> <span data-ttu-id="5995f-121">Abyste mohli používat Azure DNS, je nutné zaregistrovat předplatné Azure k používání tohoto poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="5995f-121">Your Azure subscription must be registered to use this resource provider before you can use Azure DNS.</span></span> <span data-ttu-id="5995f-122">Jedná se o jednorázovou operaci u každého odběru.</span><span class="sxs-lookup"><span data-stu-id="5995f-122">This is a one-time operation for each subscription.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```