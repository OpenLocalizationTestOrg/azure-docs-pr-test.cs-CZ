

![Virtuální počítače v samostatné cloudové služby](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

<span data-ttu-id="ebc7c-102">Pokud vaše virtuální počítače ve virtuální síti, můžete se rozhodnout, kolik cloudové služby, kterou chcete použít pro načtení sady vyrovnávání a dostupnost.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-102">If you place your virtual machines in a virtual network, you can decide how many cloud services you want to use for load balancing and availability sets.</span></span> <span data-ttu-id="ebc7c-103">Kromě toho můžete uspořádat virtuální počítače v podsítích stejným způsobem jako místní sítě a připojení virtuální sítě k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-103">Additionally, you can organize the virtual machines on subnets in the same way as your on-premises network and connect the virtual network to your on-premises network.</span></span> <span data-ttu-id="ebc7c-104">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="ebc7c-104">Here's an example:</span></span>

![Virtuální počítače ve virtuální síti](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

<span data-ttu-id="ebc7c-106">Virtuální sítě jsou doporučené způsob, jak připojit virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-106">Virtual networks are the recommended way to connect virtual machines in Azure.</span></span> <span data-ttu-id="ebc7c-107">Osvědčeným postupem je ke konfiguraci jednotlivých úrovní vaší aplikace v samostatných cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-107">The best practice is to configure each tier of your application in a separate cloud service.</span></span> <span data-ttu-id="ebc7c-108">Potřebujete však kombinovat některé virtuální počítače z vrstvy jinou aplikaci do stejné cloudové služby za účelem zůstat v rámci maximálně 200 cloudové služby na jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-108">However, you may need to combine some virtual machines from different application tiers into the same cloud service to remain within the maximum of 200 cloud services per subscription.</span></span> <span data-ttu-id="ebc7c-109">Tato a další omezení najdete v tématu [předplatné Azure a omezení služby, kvóty a omezení](../articles/azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="ebc7c-109">To review this and other limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../articles/azure-subscription-service-limits.md).</span></span>

## <a name="connect-vms-in-a-virtual-network"></a><span data-ttu-id="ebc7c-110">Připojit virtuální počítače ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="ebc7c-110">Connect VMs in a virtual network</span></span>
<span data-ttu-id="ebc7c-111">Pro připojení virtuálních počítačů ve virtuální síti:</span><span class="sxs-lookup"><span data-stu-id="ebc7c-111">To connect virtual machines in a virtual network:</span></span>

1. <span data-ttu-id="ebc7c-112">Vytvoření virtuální sítě v [portál Azure](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) a zadejte 'nasazení classic'.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-112">Create the virtual network in the [Azure portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) and specify 'classic deployment'.</span></span>
2. <span data-ttu-id="ebc7c-113">Vytvořte sadu cloudových služeb pro vaše nasazení, aby odrážel návrhu pro skupiny dostupnosti a vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-113">Create the set of cloud services for your deployment to reflect your design for availability sets and load balancing.</span></span> <span data-ttu-id="ebc7c-114">Na portálu Azure klikněte na tlačítko **nový > výpočetní > Cloudová služba** pro jednotlivých cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-114">In the Azure portal, click **New > Compute > Cloud service** for each cloud service.</span></span>

  <span data-ttu-id="ebc7c-115">Při vyplňování podrobnosti cloudové služby, vyberte stejnou _skupiny prostředků_ používat s virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-115">As you fill out the cloud service details, choose the same _resource group_ used with the virtual network.</span></span>

3. <span data-ttu-id="ebc7c-116">Chcete-li vytvořit každý nový virtuální počítač, klikněte na tlačítko **nový > výpočetní**, pak vyberte bitovou kopii odpovídající virtuální počítač z **vybrané aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-116">To create each new virtual machine, click **New > Compute**, then select the appropriate VM image from the **Featured apps**.</span></span>

  <span data-ttu-id="ebc7c-117">Ve virtuálním počítači **Základy** okně vyberte stejnou _skupiny prostředků_ používat s virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-117">In the VM **Basics** blade, choose the same _resource group_ used with the virtual network.</span></span>

  ![Okno základy virtuálních počítačů při použití virtuální sítě](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. <span data-ttu-id="ebc7c-119">Při vyplňování virtuální počítač **nastavení**, vyberte správné _Cloudová služba_ nebo _virtuální sítě_ pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-119">As you fill out the VM **Settings**, choose the correct _Cloud service_ or _virtual network_ for the VM.</span></span>

  <span data-ttu-id="ebc7c-120">Azure vybere jiné položky na základě vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-120">Azure will select the other item based on your selection.</span></span>

  ![Okno nastavení virtuálního počítače při použití virtuální sítě](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a><span data-ttu-id="ebc7c-122">Připojit virtuální počítače v samostatné cloudové služby</span><span class="sxs-lookup"><span data-stu-id="ebc7c-122">Connect VMs in a standalone cloud service</span></span>
<span data-ttu-id="ebc7c-123">Pro připojení virtuálních počítačů v samostatné cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="ebc7c-123">To connect virtual machines in a standalone cloud service:</span></span>

1. <span data-ttu-id="ebc7c-124">Vytvoření cloudové služby v rámci [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ebc7c-124">Create the cloud service in the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="ebc7c-125">Klikněte na tlačítko **nový > výpočetní > Cloudová služba**.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-125">Click **New > Compute > Cloud service**.</span></span> <span data-ttu-id="ebc7c-126">Nebo můžete vytvořit cloudovou službu pro nasazení, když vytvoříte první virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-126">Or, you can create the cloud service for your deployment when you create your first virtual machine.</span></span>
2. <span data-ttu-id="ebc7c-127">Když vytvoříte virtuální počítače, zvolte stejnou skupinu prostředků použít s cloudovou službou.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-127">When you create the virtual machines, choose the same resource group used with the cloud service.</span></span>

  ![Přidat virtuální počítač do existující cloudové služby](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  <span data-ttu-id="ebc7c-129">Při vyplňování podrobnosti virtuálního počítače, vyberte název cloudové služby vytvořili v prvním kroku.</span><span class="sxs-lookup"><span data-stu-id="ebc7c-129">As you fill out the VM details, choose the name of cloud service created in the first step.</span></span>

  ![Výběr cloudové služby pro virtuální počítač](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
