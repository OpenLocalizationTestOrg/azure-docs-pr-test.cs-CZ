<span data-ttu-id="25357-101">Pokud chcete vytvořit mezipaměť, přihlaste se nejdříve k webu [Azure Portal](https://portal.azure.com), klikněte na **Nový**,  > **Databáze** > **Mezipaměť Redis**.</span><span class="sxs-lookup"><span data-stu-id="25357-101">To create a cache, first sign in to the [Azure portal](https://portal.azure.com), and click **New** > **Databases** > **Redis Cache**.</span></span>

> [!NOTE]
> <span data-ttu-id="25357-102">Pokud účet Azure nemáte, můžete si během několika minut [vytvořit bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="25357-102">If you don't have an Azure account, you can [Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) in just a couple of minutes.</span></span>
> 
> 

![Nová mezipaměť](media/redis-cache-create/redis-cache-new-cache-menu.png)

> [!NOTE]
> <span data-ttu-id="25357-104">Mezipaměti můžete vytvářet na portálu Azure Portal, ale také pomocí šablon Resource Manageru, PowerShellu nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="25357-104">In addition to creating caches in the Azure portal, you can also create them using Resource Manager templates, PowerShell, or Azure CLI.</span></span>
> 
> * <span data-ttu-id="25357-105">Pokud chcete mezipaměť vytvořit pomocí šablony Resource Manageru, přečtěte si článek [Vytvoření mezipaměti Redis pomocí šablony](../articles/redis-cache/cache-redis-cache-arm-provision.md).</span><span class="sxs-lookup"><span data-stu-id="25357-105">To create a cache using Resource Manager templates, see [Create a Redis cache using a template](../articles/redis-cache/cache-redis-cache-arm-provision.md).</span></span>
> * <span data-ttu-id="25357-106">Pokud chcete mezipaměti vytvořit pomocí Azure PowerShellu, přečtěte si článek [Správa mezipaměti Azure Redis Cache pomocí Azure PowerShellu](../articles/redis-cache/cache-howto-manage-redis-cache-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="25357-106">To create a cache using Azure PowerShell, see [Manage Azure Redis Cache with Azure PowerShell](../articles/redis-cache/cache-howto-manage-redis-cache-powershell.md).</span></span>
> * <span data-ttu-id="25357-107">Pokud chcete mezipaměť vytvořit pomocí příkazového řádku Azure (CLI), přečtěte si článek [Postup vytvoření a správy mezipaměti Azure Redis Cache pomocí rozhraní příkazového řádku Azure (CLI)](../articles/redis-cache/cache-manage-cli.md).</span><span class="sxs-lookup"><span data-stu-id="25357-107">To create a cache using Azure CLI, see [How to create and manage Azure Redis Cache using the Azure Command-Line Interface (Azure CLI)](../articles/redis-cache/cache-manage-cli.md).</span></span>
> 
> 

<span data-ttu-id="25357-108">V okně **Nová mezipaměť Redis** zadejte požadovanou konfiguraci mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="25357-108">In the **New Redis Cache** blade, specify the desired configuration for the cache.</span></span>

![Vytvoření mezipaměti](media/redis-cache-create/redis-cache-cache-create.png) 

* <span data-ttu-id="25357-110">Do pole **Název DNS** zadejte jedinečný název mezipaměti, kterou chcete použít pro koncový bod mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="25357-110">In **Dns name**, enter a unique cache name to use for the cache endpoint.</span></span> <span data-ttu-id="25357-111">Název mezipaměti musí být řetězec o délce 1 až 63 znaků a smí obsahovat jenom čísla, písmena a znak `-`.</span><span class="sxs-lookup"><span data-stu-id="25357-111">The cache name must be a string between 1 and 63 characters and contain only numbers, letters, and the `-` character.</span></span> <span data-ttu-id="25357-112">Název mezipaměti nesmí začínat ani končit znakem `-` a po sobě jdoucí znaky `-` nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="25357-112">The cache name cannot start or end with the `-` character, and consecutive `-` characters are not valid.</span></span>
* <span data-ttu-id="25357-113">V rozevíracím seznamu **Předplatné** vyberte požadované předplatné, se kterým chcete mezipaměť používat.</span><span class="sxs-lookup"><span data-stu-id="25357-113">For **Subscription**, select the Azure subscription that you want to use for the cache.</span></span> <span data-ttu-id="25357-114">Pokud má váš účet jenom jedno předplatné, vybere se automaticky a rozevírací seznam **Předplatné** se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="25357-114">If your account has only one subscription, it will be automatically selected and the **Subscription** drop-down will not be displayed.</span></span>
* <span data-ttu-id="25357-115">V části **Skupina prostředků** vyberte nebo vytvořte skupinu prostředků pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="25357-115">In **Resource group**, select or create a resource group for your cache.</span></span> <span data-ttu-id="25357-116">Další informace najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../articles/azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="25357-116">For more information, see [Using Resource groups to manage your Azure resources](../articles/azure-resource-manager/resource-group-overview.md).</span></span> 
* <span data-ttu-id="25357-117">K určení zeměpisného umístění, ve kterém se mezipaměť hostuje, použijte **Umístění**.</span><span class="sxs-lookup"><span data-stu-id="25357-117">Use **Location** to specify the geographic location in which your cache is hosted.</span></span> <span data-ttu-id="25357-118">K zajištění nejlepšího výkonu Microsoft důrazně doporučuje vytvoření mezipaměti ve stejné oblasti, kde se už nachází klientská aplikace mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="25357-118">For the best performance, Microsoft strongly recommends that you create the cache in the same region as the cache client application.</span></span>
* <span data-ttu-id="25357-119">K výběru požadované velikosti a funkcí mezipaměti použijte možnost **Cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="25357-119">Use **Pricing tier** to select the desired cache size and features.</span></span>
* <span data-ttu-id="25357-120">**Cluster Redis** vám umožní vytvoření mezipamětí, které jsou větší než 53 GB, a sdílení dat mezi různými uzly Redis.</span><span class="sxs-lookup"><span data-stu-id="25357-120">**Redis cluster** allows you to create caches larger than 53 GB and to shard data across multiple Redis nodes.</span></span> <span data-ttu-id="25357-121">Další informace najdete v článku [Postup konfigurace clusterů pro mezipaměť Azure Redis Cache Premium](../articles/redis-cache/cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="25357-121">For more information, see [How to configure clustering for a Premium Azure Redis Cache](../articles/redis-cache/cache-how-to-premium-clustering.md).</span></span>
* <span data-ttu-id="25357-122">**Trvalost dat Redis** vám umožňuje zachovat mezipaměť pro účet služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="25357-122">**Redis persistence** offers the ability to persist your cache to an Azure Storage account.</span></span> <span data-ttu-id="25357-123">Pokyny týkající se konfigurace trvalosti najdete v článku [Postup konfigurace trvalosti pro mezipaměť Azure Redis Cache Premium](../articles/redis-cache/cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="25357-123">For instructions on configuring persistence, see [How to configure persistence for a Premium Azure Redis Cache](../articles/redis-cache/cache-how-to-premium-persistence.md).</span></span>
* <span data-ttu-id="25357-124">Služba **Virtual Network** nabízí lepší zabezpečení a izolaci omezením přístupu k vaší mezipaměti jenom pro ty klienty, kteří se nacházejí v určené službě Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="25357-124">**Virtual Network** provides enhanced security and isolation by restricting access to your cache to only those clients within the specified Azure Virtual Network.</span></span> <span data-ttu-id="25357-125">K dalšímu omezení přístupu k Redisu můžete použít všechny funkce sítě VNet, například podsítě, zásady řízení přístupu a další funkce.</span><span class="sxs-lookup"><span data-stu-id="25357-125">You can use all the features of VNet such as subnets, access control policies, and other features to further restrict access to Redis.</span></span> <span data-ttu-id="25357-126">Další informace najdete v článku [Postup konfigurace podpory služby Virtual Network pro mezipaměť Azure Redis Cache Premium](../articles/redis-cache/cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="25357-126">For more information, see [How to configure Virtual Network support for a Premium Azure Redis Cache](../articles/redis-cache/cache-how-to-premium-vnet.md).</span></span>
* <span data-ttu-id="25357-127">Přístup bez SSL je ve výchozím nastavení pro nové mezipaměti zakázaný.</span><span class="sxs-lookup"><span data-stu-id="25357-127">By default, non-SSL access is disabled for new caches.</span></span> <span data-ttu-id="25357-128">Pokud chcete povolit port bez SSL, zaškrtněte **Odblokovat port 6379 (není šifrováno pomocí SSL)**.</span><span class="sxs-lookup"><span data-stu-id="25357-128">To enable the non-SSL port, check **Unblock port 6379 (not SSL encrypted)**.</span></span>

<span data-ttu-id="25357-129">Po nakonfigurování možností nové mezipaměti klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="25357-129">Once the new cache options are configured, click **Create**.</span></span> <span data-ttu-id="25357-130">Vytvoření mezipaměti může několik minut trvat.</span><span class="sxs-lookup"><span data-stu-id="25357-130">It can take a few minutes for the cache to be created.</span></span> <span data-ttu-id="25357-131">Pokud chcete zkontrolovat stav, můžete průběh sledovat na úvodním panelu.</span><span class="sxs-lookup"><span data-stu-id="25357-131">To check the status, you can monitor the progress on the startboard.</span></span> <span data-ttu-id="25357-132">Po vytvoření má nová mezipaměť stav **Spuštěno** a je připravená k použití s [výchozím nastavením](../articles/redis-cache/cache-configure.md#default-redis-server-configuration).</span><span class="sxs-lookup"><span data-stu-id="25357-132">After the cache has been created, your new cache has a **Running** status and is ready for use with [default settings](../articles/redis-cache/cache-configure.md#default-redis-server-configuration).</span></span>

![Mezipaměť vytvořena](media/redis-cache-create/redis-cache-cache-created.png)
