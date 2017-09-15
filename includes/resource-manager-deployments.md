## <a name="incremental-and-complete-deployments"></a><span data-ttu-id="9d6d6-101">Přírůstkové a úplné nasazení</span><span class="sxs-lookup"><span data-stu-id="9d6d6-101">Incremental and complete deployments</span></span>
<span data-ttu-id="9d6d6-102">Při nasazení vašich prostředků, zadejte, že nasazení je k přírůstkové aktualizaci nebo kompletní aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-102">When deploying your resources, you specify that the deployment is either an incremental update or a complete update.</span></span> <span data-ttu-id="9d6d6-103">Hlavní rozdíl mezi tyto dva režimy je, jak Resource Manager zpracovává existující prostředky ve skupině prostředků, které nejsou v šabloně:</span><span class="sxs-lookup"><span data-stu-id="9d6d6-103">The primary difference between these two modes is how Resource Manager handles existing resources in the resource group that are not in the template:</span></span>

* <span data-ttu-id="9d6d6-104">V dokončení režimu Resource Manager **odstraní** prostředky, které existují ve skupině prostředků, ale nejsou zadané v šabloně.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-104">In complete mode, Resource Manager **deletes** resources that exist in the resource group but are not specified in the template.</span></span> 
* <span data-ttu-id="9d6d6-105">V přírůstkové režimu Resource Manager **zůstane beze změny** prostředky, které existují ve skupině prostředků, ale nejsou zadané v šabloně.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-105">In incremental mode, Resource Manager **leaves unchanged** resources that exist in the resource group but are not specified in the template.</span></span>

<span data-ttu-id="9d6d6-106">Pro oba režimy Resource Manager pokusí zřídit všechny prostředky zadané v šabloně.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-106">For both modes, Resource Manager attempts to provision all resources specified in the template.</span></span> <span data-ttu-id="9d6d6-107">Pokud prostředek již existuje ve skupině prostředků a jsou stejné jako jeho nastavení, výsledkem operace žádná změna.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-107">If the resource already exists in the resource group and its settings are unchanged, the operation results in no change.</span></span> <span data-ttu-id="9d6d6-108">Pokud změníte nastavení pro prostředek, prostředek je opatřen tyto nové nastavení.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-108">If you change the settings for a resource, the resource is provisioned with those new settings.</span></span> <span data-ttu-id="9d6d6-109">Pokud budete chtít aktualizovat umístění nebo typ existující prostředek, nasazení se nezdaří s chybou.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-109">If you attempt to update the location or type of an existing resource, the deployment fails with an error.</span></span> <span data-ttu-id="9d6d6-110">Místo toho nasaďte nový prostředek s umístění nebo typu, je nutné.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-110">Instead, deploy a new resource with the location or type that you need.</span></span>

<span data-ttu-id="9d6d6-111">Ve výchozím nastavení používá přírůstkové režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-111">By default, Resource Manager uses the incremental mode.</span></span>

<span data-ttu-id="9d6d6-112">Pro ilustraci rozdíl mezi režimy přírůstkové a úplné, zvažte následující scénář.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-112">To illustrate the difference between incremental and complete modes, consider the following scenario.</span></span>

<span data-ttu-id="9d6d6-113">**Existující skupinu prostředků** obsahuje:</span><span class="sxs-lookup"><span data-stu-id="9d6d6-113">**Existing Resource Group** contains:</span></span>

* <span data-ttu-id="9d6d6-114">Prostředek A</span><span class="sxs-lookup"><span data-stu-id="9d6d6-114">Resource A</span></span>
* <span data-ttu-id="9d6d6-115">Prostředek B</span><span class="sxs-lookup"><span data-stu-id="9d6d6-115">Resource B</span></span>
* <span data-ttu-id="9d6d6-116">Prostředek C</span><span class="sxs-lookup"><span data-stu-id="9d6d6-116">Resource C</span></span>

<span data-ttu-id="9d6d6-117">**Šablona** definuje:</span><span class="sxs-lookup"><span data-stu-id="9d6d6-117">**Template** defines:</span></span>

* <span data-ttu-id="9d6d6-118">Prostředek A</span><span class="sxs-lookup"><span data-stu-id="9d6d6-118">Resource A</span></span>
* <span data-ttu-id="9d6d6-119">Prostředek B</span><span class="sxs-lookup"><span data-stu-id="9d6d6-119">Resource B</span></span>
* <span data-ttu-id="9d6d6-120">Prostředek D</span><span class="sxs-lookup"><span data-stu-id="9d6d6-120">Resource D</span></span>

<span data-ttu-id="9d6d6-121">Při nasazení v **přírůstkové** režimu, skupina prostředků obsahuje:</span><span class="sxs-lookup"><span data-stu-id="9d6d6-121">When deployed in **incremental** mode, the resource group contains:</span></span>

* <span data-ttu-id="9d6d6-122">Prostředek A</span><span class="sxs-lookup"><span data-stu-id="9d6d6-122">Resource A</span></span>
* <span data-ttu-id="9d6d6-123">Prostředek B</span><span class="sxs-lookup"><span data-stu-id="9d6d6-123">Resource B</span></span>
* <span data-ttu-id="9d6d6-124">Prostředek C</span><span class="sxs-lookup"><span data-stu-id="9d6d6-124">Resource C</span></span>
* <span data-ttu-id="9d6d6-125">Prostředek D</span><span class="sxs-lookup"><span data-stu-id="9d6d6-125">Resource D</span></span>

<span data-ttu-id="9d6d6-126">Při nasazení v **dokončení** režimu C prostředků se odstraní.</span><span class="sxs-lookup"><span data-stu-id="9d6d6-126">When deployed in **complete** mode, Resource C is deleted.</span></span> <span data-ttu-id="9d6d6-127">Skupina prostředků obsahuje:</span><span class="sxs-lookup"><span data-stu-id="9d6d6-127">The resource group contains:</span></span>

* <span data-ttu-id="9d6d6-128">Prostředek A</span><span class="sxs-lookup"><span data-stu-id="9d6d6-128">Resource A</span></span>
* <span data-ttu-id="9d6d6-129">Prostředek B</span><span class="sxs-lookup"><span data-stu-id="9d6d6-129">Resource B</span></span>
* <span data-ttu-id="9d6d6-130">Prostředek D</span><span class="sxs-lookup"><span data-stu-id="9d6d6-130">Resource D</span></span>
