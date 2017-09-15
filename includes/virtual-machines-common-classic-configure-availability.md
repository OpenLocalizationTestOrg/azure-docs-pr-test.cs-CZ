


<span data-ttu-id="aa7f3-101">Sadu dostupnosti pomáhá udržovat vaše virtuální počítače, které jsou k dispozici při výpadku, například při údržbě.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-101">An availability set helps keep your virtual machines available during downtime, such as during maintenance.</span></span> <span data-ttu-id="aa7f3-102">Umístění podobně nakonfigurované dva nebo více virtuálních počítačů v nastavení dostupnosti vytvoří redundance potřebné pro zachování dostupnost aplikace nebo služby, které běží ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-102">Placing two or more similarly configured virtual machines in an availability set creates the redundancy needed to maintain availability of the applications or services that your virtual machine runs.</span></span> <span data-ttu-id="aa7f3-103">Podrobnosti o tomto postupu najdete v tématu [Správa dostupnosti virtuálních počítačů][Manage the availability of virtual machines].</span><span class="sxs-lookup"><span data-stu-id="aa7f3-103">For details about how this works, see [Manage the availability of virtual machines][Manage the availability of virtual machines].</span></span>

<span data-ttu-id="aa7f3-104">Je vhodné použít skupiny dostupnosti a vyrovnávání zatížení koncové body k zajištění, že aplikace je vždy k dispozici a spuštěné efektivně.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-104">It's a best practice to use both availability sets and load-balancing endpoints to help ensure that your application is always available and running efficiently.</span></span> <span data-ttu-id="aa7f3-105">Podrobnosti o koncové body s vyrovnáváním zatížení najdete v tématu [pro infrastrukturu Azure služby Vyrovnávání zatížení][Load balancing for Azure infrastructure services].</span><span class="sxs-lookup"><span data-stu-id="aa7f3-105">For details about load-balanced endpoints, see [Load balancing for Azure infrastructure services][Load balancing for Azure infrastructure services].</span></span>

<span data-ttu-id="aa7f3-106">Klasické virtuální počítače můžete přidat do dostupnosti nastavit pomocí jedné ze dvou možností:</span><span class="sxs-lookup"><span data-stu-id="aa7f3-106">You can add classic virtual machines into an availability set by using one of two options:</span></span>

* <span data-ttu-id="aa7f3-107">[Možnost 1: Vytvoření virtuálního počítače a sadu ve stejnou dobu dostupnosti][Option 1: Create a virtual machine and an availability set at the same time].</span><span class="sxs-lookup"><span data-stu-id="aa7f3-107">[Option 1: Create a virtual machine and an availability set at the same time][Option 1: Create a virtual machine and an availability set at the same time].</span></span> <span data-ttu-id="aa7f3-108">Potom přidáte nové virtuální počítače do sady, při vytváření těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-108">Then, add new virtual machines to the set when you create those virtual machines.</span></span>
* <span data-ttu-id="aa7f3-109">[Možnost 2: Přidání existujícího virtuálního počítače do skupiny dostupnosti][Option 2: Add an existing virtual machine to an availability set].</span><span class="sxs-lookup"><span data-stu-id="aa7f3-109">[Option 2: Add an existing virtual machine to an availability set][Option 2: Add an existing virtual machine to an availability set].</span></span>

> [!NOTE]
> <span data-ttu-id="aa7f3-110">V klasickém modelu virtuální počítače, které chcete umístit do stejné skupiny dostupnosti musí patřit do stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-110">In the classic model, virtual machines that you want to put in the same availability set must belong to the same cloud service.</span></span>
> 
> 

## <span data-ttu-id="aa7f3-111"><a id="createset"></a>Možnost 1: vytvoření virtuálního počítače a současně sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="aa7f3-111"><a id="createset"> </a>Option 1: Create a virtual machine and an availability set at the same time</span></span>
<span data-ttu-id="aa7f3-112">K tomu můžete portálu Azure nebo Azure PowerShell příkazy.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-112">You can use either the Azure portal or Azure PowerShell commands to do this.</span></span>

<span data-ttu-id="aa7f3-113">Použití portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="aa7f3-113">To use the Azure portal:</span></span>

1. <span data-ttu-id="aa7f3-114">Pokud jste to ještě neudělali, přihlaste se k [Portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="aa7f3-114">If you haven't already done so, sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="aa7f3-115">V nabídce centra klikněte na tlačítko **+ nový**a potom klikněte na **virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-115">On the hub menu, click **+ New**, and then click **Virtual Machine**.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. <span data-ttu-id="aa7f3-117">Vyberte Marketplace image virtuálního počítače, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-117">Select the Marketplace virtual machine image you wish to use.</span></span> <span data-ttu-id="aa7f3-118">Můžete vytvořit virtuální počítač se systémem Linux nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-118">You can choose to create a Linux or Windows virtual machine.</span></span>
4. <span data-ttu-id="aa7f3-119">Pro vybraný virtuální počítač, ověřte, že model nasazení je nastavena na **Classic** a pak klikněte na tlačítko **vytvořit**</span><span class="sxs-lookup"><span data-stu-id="aa7f3-119">For the selected virtual machine, verify that the deployment model is set to **Classic** and then click **Create**</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. <span data-ttu-id="aa7f3-121">Zadejte název virtuálního počítače, uživatelské jméno a heslo (pro počítače s Windows) nebo veřejný klíč SSH (pro počítače se systémem Linux).</span><span class="sxs-lookup"><span data-stu-id="aa7f3-121">Enter a virtual machine name, user name and password (for Windows machines) or SSH public key (for Linux machines).</span></span> 
6. <span data-ttu-id="aa7f3-122">Zvolte velikost virtuálního počítače a pak klikněte na tlačítko **vyberte** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-122">Choose the VM size and then click **Select** to continue.</span></span>
7. <span data-ttu-id="aa7f3-123">Zvolte **volitelné konfigurace > sadu dostupnosti**, a vyberte sadu dostupnosti, které chcete přidat virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-123">Choose **Optional Configuration > Availability set**, and select the availability set you wish to add the virtual machine to.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. <span data-ttu-id="aa7f3-125">Zkontrolujte nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-125">Review your configuration settings.</span></span> <span data-ttu-id="aa7f3-126">Když jste hotovi, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-126">When you're done, click **Create**.</span></span>
9. <span data-ttu-id="aa7f3-127">Zatímco Azure vytváří virtuální počítač, můžete sledovat průběh v části **virtuální počítače** v nabídce centra.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-127">While Azure creates your virtual machine, you can track the progress under **Virtual Machines** in the hub menu.</span></span>

<span data-ttu-id="aa7f3-128">Pomocí příkazů prostředí Azure PowerShell k vytvoření virtuálního počítače Azure a přidejte ji do nové nebo existující dostupnost sady naleznete v části [pomocí prostředí Azure PowerShell k vytvoření a nastavení virtuálních počítačích se systémem Windows](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="aa7f3-128">To use Azure PowerShell commands to create an Azure virtual machine and add it to a new or existing availability set, see [Use Azure PowerShell to create and preconfigure Windows-based virtual machines](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="aa7f3-129"><a id="addmachine"></a>Možnost 2: Přidání existujícího virtuálního počítače do skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="aa7f3-129"><a id="addmachine"> </a>Option 2: Add an existing virtual machine to an availability set</span></span>
<span data-ttu-id="aa7f3-130">Na portálu Azure můžete přidat existující klasické virtuální počítače na existující dostupnost nastavit nebo vytvořte novou pro ně.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-130">In the Azure portal, you can add existing classic virtual machines to an existing availability set or create a new one for them.</span></span> <span data-ttu-id="aa7f3-131">(Mějte na paměti, že virtuální počítače ve stejné skupině dostupnosti musí patřit do stejné cloudové služby.) Kroky jsou téměř stejný.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-131">(Keep in mind that the virtual machines in the same availability set must belong to the same cloud service.) The steps are almost the same.</span></span> <span data-ttu-id="aa7f3-132">V prostředí Azure PowerShell můžete přidat virtuální počítač do stávající sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-132">With Azure PowerShell, you can add the virtual machine to an existing availability set.</span></span>

1. <span data-ttu-id="aa7f3-133">Pokud jste tak již neučinili, přihlaste se k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="aa7f3-133">If you have not already done so, sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="aa7f3-134">V nabídce centra klikněte na tlačítko **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-134">On the Hub menu, click **Virtual Machines (classic)**.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. <span data-ttu-id="aa7f3-136">Seznam virtuálních počítačů vyberte název virtuálního počítače, který chcete přidat do sady.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-136">From the list of virtual machines, select the name of the virtual machine that you want to add to the set.</span></span>
4. <span data-ttu-id="aa7f3-137">Zvolte **sadu dostupnosti** z virtuálního počítače **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-137">Choose **Availability set** from the virtual machine **Settings**.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. <span data-ttu-id="aa7f3-139">Vyberte skupinu dostupnosti, které chcete přidat virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-139">Select the availability set you wish to add the virtual machine to.</span></span> <span data-ttu-id="aa7f3-140">Virtuální počítač musí patřit do stejné cloudové služby jako skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-140">The virtual machine must belong to the same cloud service as the availability set.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. <span data-ttu-id="aa7f3-142">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-142">Click **Save**.</span></span>

<span data-ttu-id="aa7f3-143">Chcete-li použít příkazy prostředí Azure PowerShell, otevřete relaci prostředí Azure PowerShell úrovni správce a spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-143">To use Azure PowerShell commands, open an administrator-level Azure PowerShell session and run the following command.</span></span> <span data-ttu-id="aa7f3-144">Zástupné symboly (například &lt;VmCloudServiceName&gt;), nahraďte všechna data v uvozovkách, včetně < a > znaky s správné názvy.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-144">For the placeholders (such as &lt;VmCloudServiceName&gt;), replace everything within the quotes, including the < and > characters, with the correct names.</span></span>

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> <span data-ttu-id="aa7f3-145">Může mít virtuální počítač restartovat, aby dokončit ho přidáte do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="aa7f3-145">The virtual machine might have to be restarted to finish adding it to the availability set.</span></span>
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at the same time]: #createset
[Option 2: Add an existing virtual machine to an availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage the availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

