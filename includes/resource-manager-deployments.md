## <a name="incremental-and-complete-deployments"></a><span data-ttu-id="7a4df-101">Přírůstkové a úplné nasazení</span><span class="sxs-lookup"><span data-stu-id="7a4df-101">Incremental and complete deployments</span></span>
<span data-ttu-id="7a4df-102">Při nasazení vašich prostředků, zadejte, že hello nasazení je k přírůstkové aktualizaci nebo kompletní aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7a4df-102">When deploying your resources, you specify that hello deployment is either an incremental update or a complete update.</span></span> <span data-ttu-id="7a4df-103">Hello hlavní rozdíl mezi tyto dva režimy je, jak Resource Manager zpracovává existující prostředky ve skupině prostředků hello, které nejsou v šabloně hello:</span><span class="sxs-lookup"><span data-stu-id="7a4df-103">hello primary difference between these two modes is how Resource Manager handles existing resources in hello resource group that are not in hello template:</span></span>

* <span data-ttu-id="7a4df-104">V dokončení režimu Resource Manager **odstraní** prostředky, které existují ve skupině prostředků hello, ale nejsou zadané v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="7a4df-104">In complete mode, Resource Manager **deletes** resources that exist in hello resource group but are not specified in hello template.</span></span> 
* <span data-ttu-id="7a4df-105">V přírůstkové režimu Resource Manager **zůstane beze změny** prostředky, které existují ve skupině prostředků hello, ale nejsou zadané v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="7a4df-105">In incremental mode, Resource Manager **leaves unchanged** resources that exist in hello resource group but are not specified in hello template.</span></span>

<span data-ttu-id="7a4df-106">Pro oba režimy Resource Manager pokusí tooprovision všechny prostředky zadané v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="7a4df-106">For both modes, Resource Manager attempts tooprovision all resources specified in hello template.</span></span> <span data-ttu-id="7a4df-107">Pokud už existuje prostředek hello ve skupině prostředků hello a jsou stejné jako jeho nastavení, výsledkem operace hello žádná změna.</span><span class="sxs-lookup"><span data-stu-id="7a4df-107">If hello resource already exists in hello resource group and its settings are unchanged, hello operation results in no change.</span></span> <span data-ttu-id="7a4df-108">Pokud změníte nastavení hello prostředků, prostředků hello je opatřen tyto nové nastavení.</span><span class="sxs-lookup"><span data-stu-id="7a4df-108">If you change hello settings for a resource, hello resource is provisioned with those new settings.</span></span> <span data-ttu-id="7a4df-109">Když zkusíte tooupdate hello umístění nebo typ existující prostředek, hello nasazení se nezdaří s chybou.</span><span class="sxs-lookup"><span data-stu-id="7a4df-109">If you attempt tooupdate hello location or type of an existing resource, hello deployment fails with an error.</span></span> <span data-ttu-id="7a4df-110">Místo toho nasaďte nový prostředek s umístěním hello nebo typu, je nutné.</span><span class="sxs-lookup"><span data-stu-id="7a4df-110">Instead, deploy a new resource with hello location or type that you need.</span></span>

<span data-ttu-id="7a4df-111">Ve výchozím nastavení používá hello přírůstkové režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7a4df-111">By default, Resource Manager uses hello incremental mode.</span></span>

<span data-ttu-id="7a4df-112">tooillustrate hello rozdíl mezi režimy přírůstkové a úplné, zvažte následující scénáře hello.</span><span class="sxs-lookup"><span data-stu-id="7a4df-112">tooillustrate hello difference between incremental and complete modes, consider hello following scenario.</span></span>

<span data-ttu-id="7a4df-113">**Existující skupinu prostředků** obsahuje:</span><span class="sxs-lookup"><span data-stu-id="7a4df-113">**Existing Resource Group** contains:</span></span>

* <span data-ttu-id="7a4df-114">Prostředek A</span><span class="sxs-lookup"><span data-stu-id="7a4df-114">Resource A</span></span>
* <span data-ttu-id="7a4df-115">Prostředek B</span><span class="sxs-lookup"><span data-stu-id="7a4df-115">Resource B</span></span>
* <span data-ttu-id="7a4df-116">Prostředek C</span><span class="sxs-lookup"><span data-stu-id="7a4df-116">Resource C</span></span>

<span data-ttu-id="7a4df-117">**Šablona** definuje:</span><span class="sxs-lookup"><span data-stu-id="7a4df-117">**Template** defines:</span></span>

* <span data-ttu-id="7a4df-118">Prostředek A</span><span class="sxs-lookup"><span data-stu-id="7a4df-118">Resource A</span></span>
* <span data-ttu-id="7a4df-119">Prostředek B</span><span class="sxs-lookup"><span data-stu-id="7a4df-119">Resource B</span></span>
* <span data-ttu-id="7a4df-120">Prostředek D</span><span class="sxs-lookup"><span data-stu-id="7a4df-120">Resource D</span></span>

<span data-ttu-id="7a4df-121">Při nasazení v **přírůstkové** režimu hello skupina prostředků obsahuje:</span><span class="sxs-lookup"><span data-stu-id="7a4df-121">When deployed in **incremental** mode, hello resource group contains:</span></span>

* <span data-ttu-id="7a4df-122">Prostředek A</span><span class="sxs-lookup"><span data-stu-id="7a4df-122">Resource A</span></span>
* <span data-ttu-id="7a4df-123">Prostředek B</span><span class="sxs-lookup"><span data-stu-id="7a4df-123">Resource B</span></span>
* <span data-ttu-id="7a4df-124">Prostředek C</span><span class="sxs-lookup"><span data-stu-id="7a4df-124">Resource C</span></span>
* <span data-ttu-id="7a4df-125">Prostředek D</span><span class="sxs-lookup"><span data-stu-id="7a4df-125">Resource D</span></span>

<span data-ttu-id="7a4df-126">Při nasazení v **dokončení** režimu C prostředků se odstraní.</span><span class="sxs-lookup"><span data-stu-id="7a4df-126">When deployed in **complete** mode, Resource C is deleted.</span></span> <span data-ttu-id="7a4df-127">Skupina prostředků Hello obsahuje:</span><span class="sxs-lookup"><span data-stu-id="7a4df-127">hello resource group contains:</span></span>

* <span data-ttu-id="7a4df-128">Prostředek A</span><span class="sxs-lookup"><span data-stu-id="7a4df-128">Resource A</span></span>
* <span data-ttu-id="7a4df-129">Prostředek B</span><span class="sxs-lookup"><span data-stu-id="7a4df-129">Resource B</span></span>
* <span data-ttu-id="7a4df-130">Prostředek D</span><span class="sxs-lookup"><span data-stu-id="7a4df-130">Resource D</span></span>
