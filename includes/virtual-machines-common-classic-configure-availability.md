


<span data-ttu-id="e0307-101">Sadu dostupnosti pomáhá udržovat vaše virtuální počítače, které jsou k dispozici při výpadku, například při údržbě.</span><span class="sxs-lookup"><span data-stu-id="e0307-101">An availability set helps keep your virtual machines available during downtime, such as during maintenance.</span></span> <span data-ttu-id="e0307-102">Umístění podobně nakonfigurované dva nebo více virtuálních počítačů v nastavení dostupnosti vytvoří hello redundance potřeby toomaintain dostupnost hello aplikacím nebo službám, které běží ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e0307-102">Placing two or more similarly configured virtual machines in an availability set creates hello redundancy needed toomaintain availability of hello applications or services that your virtual machine runs.</span></span> <span data-ttu-id="e0307-103">Podrobnosti o tomto postupu najdete v tématu [spravovat hello dostupnosti virtuálních počítačů][Manage hello availability of virtual machines].</span><span class="sxs-lookup"><span data-stu-id="e0307-103">For details about how this works, see [Manage hello availability of virtual machines][Manage hello availability of virtual machines].</span></span>

<span data-ttu-id="e0307-104">Je nejlepší postup toouse skupiny dostupnosti a vyrovnávání zatížení toohelp koncových bodů zajistěte, aby byl aplikaci vždy k dispozici a spuštěná efektivně.</span><span class="sxs-lookup"><span data-stu-id="e0307-104">It's a best practice toouse both availability sets and load-balancing endpoints toohelp ensure that your application is always available and running efficiently.</span></span> <span data-ttu-id="e0307-105">Podrobnosti o koncové body s vyrovnáváním zatížení najdete v tématu [pro infrastrukturu Azure služby Vyrovnávání zatížení][Load balancing for Azure infrastructure services].</span><span class="sxs-lookup"><span data-stu-id="e0307-105">For details about load-balanced endpoints, see [Load balancing for Azure infrastructure services][Load balancing for Azure infrastructure services].</span></span>

<span data-ttu-id="e0307-106">Klasické virtuální počítače můžete přidat do dostupnosti nastavit pomocí jedné ze dvou možností:</span><span class="sxs-lookup"><span data-stu-id="e0307-106">You can add classic virtual machines into an availability set by using one of two options:</span></span>

* <span data-ttu-id="e0307-107">[Možnost 1: Vytvoření virtuálního počítače a sadu dostupnosti v hello stejný čas][Option 1: Create a virtual machine and an availability set at hello same time].</span><span class="sxs-lookup"><span data-stu-id="e0307-107">[Option 1: Create a virtual machine and an availability set at hello same time][Option 1: Create a virtual machine and an availability set at hello same time].</span></span> <span data-ttu-id="e0307-108">Potom můžete přidáte nové virtuální počítače toohello nastavit při vytváření těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e0307-108">Then, add new virtual machines toohello set when you create those virtual machines.</span></span>
* <span data-ttu-id="e0307-109">[Možnost 2: Přidat stávající sadu dostupnosti virtuálního počítače tooan][Option 2: Add an existing virtual machine tooan availability set].</span><span class="sxs-lookup"><span data-stu-id="e0307-109">[Option 2: Add an existing virtual machine tooan availability set][Option 2: Add an existing virtual machine tooan availability set].</span></span>

> [!NOTE]
> <span data-ttu-id="e0307-110">V hello klasického modelu, hello virtuálních počítačů, které chcete mít tooput ve stejné skupině dostupnosti musí patřit toohello stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e0307-110">In hello classic model, virtual machines that you want tooput in hello same availability set must belong toohello same cloud service.</span></span>
> 
> 

## <span data-ttu-id="e0307-111"><a id="createset"></a>Možnost 1: vytvoření virtuálního počítače a sadu dostupnosti v hello stejný čas</span><span class="sxs-lookup"><span data-stu-id="e0307-111"><a id="createset"> </a>Option 1: Create a virtual machine and an availability set at hello same time</span></span>
<span data-ttu-id="e0307-112">Můžete použít buď hello portál Azure nebo Azure PowerShell příkazy toodo to.</span><span class="sxs-lookup"><span data-stu-id="e0307-112">You can use either hello Azure portal or Azure PowerShell commands toodo this.</span></span>

<span data-ttu-id="e0307-113">toouse hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e0307-113">toouse hello Azure portal:</span></span>

1. <span data-ttu-id="e0307-114">Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e0307-114">If you haven't already done so, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e0307-115">V nabídce centra hello, klikněte na tlačítko **+ nový**a potom klikněte na **virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="e0307-115">On hello hub menu, click **+ New**, and then click **Virtual Machine**.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. <span data-ttu-id="e0307-117">Vyberte Marketplace bitovou kopii virtuálního počítače hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="e0307-117">Select hello Marketplace virtual machine image you wish toouse.</span></span> <span data-ttu-id="e0307-118">Můžete toocreate virtuálního počítače se systémem Linux nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="e0307-118">You can choose toocreate a Linux or Windows virtual machine.</span></span>
4. <span data-ttu-id="e0307-119">Pro hello vybrané virtuální počítače, ověřte, že model nasazení hello je nastaven příliš**Classic** a pak klikněte na tlačítko **vytvořit**</span><span class="sxs-lookup"><span data-stu-id="e0307-119">For hello selected virtual machine, verify that hello deployment model is set too**Classic** and then click **Create**</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. <span data-ttu-id="e0307-121">Zadejte název virtuálního počítače, uživatelské jméno a heslo (pro počítače s Windows) nebo veřejný klíč SSH (pro počítače se systémem Linux).</span><span class="sxs-lookup"><span data-stu-id="e0307-121">Enter a virtual machine name, user name and password (for Windows machines) or SSH public key (for Linux machines).</span></span> 
6. <span data-ttu-id="e0307-122">Zvolte velikost virtuálního počítače hello a pak klikněte na tlačítko **vyberte** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="e0307-122">Choose hello VM size and then click **Select** toocontinue.</span></span>
7. <span data-ttu-id="e0307-123">Zvolte **volitelné konfigurace > sadu dostupnosti**a vyberte skupinu dostupnosti hello chcete tooadd hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e0307-123">Choose **Optional Configuration > Availability set**, and select hello availability set you wish tooadd hello virtual machine to.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. <span data-ttu-id="e0307-125">Zkontrolujte nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e0307-125">Review your configuration settings.</span></span> <span data-ttu-id="e0307-126">Když jste hotovi, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e0307-126">When you're done, click **Create**.</span></span>
9. <span data-ttu-id="e0307-127">Zatímco Azure vytváří virtuální počítač, můžete sledovat průběh hello pod **virtuální počítače** v nabídce centra hello.</span><span class="sxs-lookup"><span data-stu-id="e0307-127">While Azure creates your virtual machine, you can track hello progress under **Virtual Machines** in hello hub menu.</span></span>

<span data-ttu-id="e0307-128">toouse prostředí Azure PowerShell příkazy toocreate virtuální počítač Azure a přidejte ho sady dostupnosti. nový nebo existující tooa, přečtěte si [toocreate použití Azure PowerShell a předem nakonfigurujte virtuálních počítačích se systémem Windows](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e0307-128">toouse Azure PowerShell commands toocreate an Azure virtual machine and add it tooa new or existing availability set, see [Use Azure PowerShell toocreate and preconfigure Windows-based virtual machines](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="e0307-129"><a id="addmachine"></a>Možnost 2: Přidat stávající sadu dostupnosti tooan virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e0307-129"><a id="addmachine"> </a>Option 2: Add an existing virtual machine tooan availability set</span></span>
<span data-ttu-id="e0307-130">V hello portálu Azure můžete přidat existující klasické virtuální počítače tooan stávající sadu dostupnosti nebo vytvořte novou pro ně.</span><span class="sxs-lookup"><span data-stu-id="e0307-130">In hello Azure portal, you can add existing classic virtual machines tooan existing availability set or create a new one for them.</span></span> <span data-ttu-id="e0307-131">(Nezapomeňte, že hello hello virtuální počítače ve stejné skupině dostupnosti musí patřit toohello stejné cloudové služby.) Hello kroky jsou hello téměř stejný.</span><span class="sxs-lookup"><span data-stu-id="e0307-131">(Keep in mind that hello virtual machines in hello same availability set must belong toohello same cloud service.) hello steps are almost hello same.</span></span> <span data-ttu-id="e0307-132">V prostředí Azure PowerShell můžete přidat hello virtuálního počítače tooan stávající sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="e0307-132">With Azure PowerShell, you can add hello virtual machine tooan existing availability set.</span></span>

1. <span data-ttu-id="e0307-133">Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e0307-133">If you have not already done so, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e0307-134">V nabídce centra hello, klikněte na tlačítko **virtuálních počítačů (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="e0307-134">On hello Hub menu, click **Virtual Machines (classic)**.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. <span data-ttu-id="e0307-136">Hello seznam virtuálních počítačů vyberte název hello hello virtuálního počítače, které mají tooadd toohello sady.</span><span class="sxs-lookup"><span data-stu-id="e0307-136">From hello list of virtual machines, select hello name of hello virtual machine that you want tooadd toohello set.</span></span>
4. <span data-ttu-id="e0307-137">Zvolte **sadu dostupnosti** z virtuálního počítače hello **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="e0307-137">Choose **Availability set** from hello virtual machine **Settings**.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. <span data-ttu-id="e0307-139">Vyberte hello sady dostupnosti. Chcete tooadd hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e0307-139">Select hello availability set you wish tooadd hello virtual machine to.</span></span> <span data-ttu-id="e0307-140">virtuální počítač Hello musí patřit toohello stejná jako skupina dostupnosti hello Cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="e0307-140">hello virtual machine must belong toohello same cloud service as hello availability set.</span></span>
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. <span data-ttu-id="e0307-142">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e0307-142">Click **Save**.</span></span>

<span data-ttu-id="e0307-143">Příkazy prostředí Azure PowerShell toouse, otevřete relaci prostředí Azure PowerShell úrovni správce a spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="e0307-143">toouse Azure PowerShell commands, open an administrator-level Azure PowerShell session and run hello following command.</span></span> <span data-ttu-id="e0307-144">Pro hello zástupné symboly (například &lt;VmCloudServiceName&gt;), nahraďte vše v rámci hello uvozovky, včetně hello < a > znaků, se hello správnými názvy.</span><span class="sxs-lookup"><span data-stu-id="e0307-144">For hello placeholders (such as &lt;VmCloudServiceName&gt;), replace everything within hello quotes, including hello < and > characters, with hello correct names.</span></span>

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> <span data-ttu-id="e0307-145">Hello virtuální počítač může mít toobe restartovat toofinish přidáním toohello sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="e0307-145">hello virtual machine might have toobe restarted toofinish adding it toohello availability set.</span></span>
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at hello same time]: #createset
[Option 2: Add an existing virtual machine tooan availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage hello availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

