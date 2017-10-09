## <a name="set-up-azure-cli-for-azure-dns"></a><span data-ttu-id="359a8-101">Nastavení rozhraní příkazového řádku Azure pro Azure DNS</span><span class="sxs-lookup"><span data-stu-id="359a8-101">Set up Azure CLI for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="359a8-102">Než začnete</span><span class="sxs-lookup"><span data-stu-id="359a8-102">Before you begin</span></span>

<span data-ttu-id="359a8-103">Ověřte, zda máte následující položky před zahájením konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="359a8-103">Verify that you have hello following items before beginning your configuration.</span></span>

* <span data-ttu-id="359a8-104">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="359a8-104">An Azure subscription.</span></span> <span data-ttu-id="359a8-105">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="359a8-105">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="359a8-106">Nainstalujte nejnovější verzi hello hello Azure CLI, k dispozici pro Windows, Linux nebo MAC.</span><span class="sxs-lookup"><span data-stu-id="359a8-106">Install hello latest version of hello Azure CLI, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="359a8-107">Další informace najdete v [hello instalace rozhraní příkazového řádku Azure](../articles/cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="359a8-107">More information is available at [Install hello Azure CLI](../articles/cli-install-nodejs.md).</span></span>

### <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="359a8-108">Přihlaste se tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="359a8-108">Sign in tooyour Azure account</span></span>

<span data-ttu-id="359a8-109">Otevřete okno konzoly a proveďte ověření pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="359a8-109">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="359a8-110">Další informace najdete v tématu [přihlásit tooAzure z hello rozhraní příkazového řádku Azure](../articles/xplat-cli-connect.md)</span><span class="sxs-lookup"><span data-stu-id="359a8-110">For more information, see [Log in tooAzure from hello Azure CLI](../articles/xplat-cli-connect.md)</span></span>

```azurecli
azure login
```

### <a name="switch-cli-mode"></a><span data-ttu-id="359a8-111">Přepnutí režimu rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="359a8-111">Switch CLI mode</span></span>

<span data-ttu-id="359a8-112">Azure DNS používá Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="359a8-112">Azure DNS uses Azure Resource Manager.</span></span> <span data-ttu-id="359a8-113">Ujistěte se, že jste přešli příkazy Azure Resource Manager toouse režimu rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="359a8-113">Make sure you switch CLI mode toouse Azure Resource Manager commands.</span></span>

```azurecli
azure config mode arm
```

### <a name="select-hello-subscription"></a><span data-ttu-id="359a8-114">Vyberte předplatné hello</span><span class="sxs-lookup"><span data-stu-id="359a8-114">Select hello subscription</span></span>

<span data-ttu-id="359a8-115">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="359a8-115">Check hello subscriptions for hello account.</span></span>

```azurecli
azure account list
```

<span data-ttu-id="359a8-116">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="359a8-116">Choose which of your Azure subscriptions toouse.</span></span>

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="359a8-117">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="359a8-117">Create a resource group</span></span>

<span data-ttu-id="359a8-118">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="359a8-118">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="359a8-119">To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="359a8-119">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="359a8-120">Ale vzhledem k tomu, že všechny prostředky DNS jsou globální, a ne místní, hello Volba umístění skupiny prostředků nemá žádný vliv na Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="359a8-120">However, because all DNS resources are global, not regional, hello choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="359a8-121">Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="359a8-121">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a><span data-ttu-id="359a8-122">Registrace poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="359a8-122">Register resource provider</span></span>

<span data-ttu-id="359a8-123">Hello služba Azure DNS spravuje poskytovatel prostředků Microsoft.Network hello.</span><span class="sxs-lookup"><span data-stu-id="359a8-123">hello Azure DNS service is managed by hello Microsoft.Network resource provider.</span></span> <span data-ttu-id="359a8-124">Vaše předplatné Azure musí být registrovaný toouse tohoto poskytovatele prostředků než budete moci použít Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="359a8-124">Your Azure subscription must be registered toouse this resource provider before you can use Azure DNS.</span></span> <span data-ttu-id="359a8-125">Jedná se o jednorázovou operaci u každého odběru.</span><span class="sxs-lookup"><span data-stu-id="359a8-125">This is a one-time operation for each subscription.</span></span>

```azurecli
azure provider register --namespace Microsoft.Network
```

