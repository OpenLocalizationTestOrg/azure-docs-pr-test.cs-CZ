---
title: "Vytvoření dostupnosti virtuálních počítačů v Azure | Microsoft Docs"
description: "Naučte se vytvořit skupinu dostupnosti spravované nebo nespravované sadu dostupnosti pro virtuální počítače pomocí prostředí Azure PowerShell nebo portálu v modelu nasazení Resource Manager."
keywords: Sady dostupnosti.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a3db8659-ace8-4e78-8b8c-1e75c04c042c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e813ade8e105169f21138ed8a8eafda1ff39f42e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a><span data-ttu-id="b35d9-104">Zvýšení dostupnosti virtuálních počítačů tak, že vytvoříte skupinu dostupnosti Azure</span><span class="sxs-lookup"><span data-stu-id="b35d9-104">Increase VM availability by creating an Azure availability set</span></span> 
<span data-ttu-id="b35d9-105">Skupiny dostupnosti poskytnout redundanci do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b35d9-105">Availability sets provide redundancy to your application.</span></span> <span data-ttu-id="b35d9-106">Doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="b35d9-106">We recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="b35d9-107">Tato konfigurace zajistí, že během buď plánované i neplánované údržby alespoň jeden virtuální počítač bude k dispozici a splňují podmínku 99.95 % smlouva Azure SLA.</span><span class="sxs-lookup"><span data-stu-id="b35d9-107">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine will be available and meet the 99.95% Azure SLA.</span></span> <span data-ttu-id="b35d9-108">Další informace najdete v tématu [SLA pro virtuální počítače](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="b35d9-108">For more information, see the [SLA for Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b35d9-109">Virtuální počítače musí být vytvořeny ve stejné skupině prostředků jako skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="b35d9-109">VMs must be created in the same resource group as the availability set.</span></span>
> 

<span data-ttu-id="b35d9-110">Pokud chcete virtuální počítač jako součást skupiny dostupnosti, je nutné vytvořit skupinu dostupnosti nejprve nebo při vytváření vašeho prvního virtuálního počítače v sadě.</span><span class="sxs-lookup"><span data-stu-id="b35d9-110">If you want your VM to be part of an availability set, you need to create the availability set first or while you are creating your first VM in the set.</span></span> <span data-ttu-id="b35d9-111">Pokud virtuální počítač bude používat spravované disky, je nutné vytvořit skupinu dostupnosti jako sadu spravovaných dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="b35d9-111">If your VM will be using Managed Disks, the availability set must be created as a managed availability set.</span></span>

<span data-ttu-id="b35d9-112">Další informace o vytváření a použití skupiny dostupnosti najdete v tématu [Správa dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b35d9-112">For more information about creating and using availability sets, see [Manage the availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a><span data-ttu-id="b35d9-113">Vytvořit sadu před vytvořením virtuálních počítačů dostupnosti pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="b35d9-113">Use the portal to create an availability set before creating your VMs</span></span>
1. <span data-ttu-id="b35d9-114">V nabídce centra klikněte na **Procházet** a vyberte **sady dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="b35d9-114">In the hub menu, click **Browse** and select **Availability sets**.</span></span>
2. <span data-ttu-id="b35d9-115">Na **sady dostupnosti okno**, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b35d9-115">On the **Availability sets blade**, click **Add**.</span></span>
   
    ![Snímek obrazovky, který ukazuje na tlačítko Přidat pro vytvoření nové dostupnosti nastavit.](./media/create-availability-set/add-availability-set.png)
3. <span data-ttu-id="b35d9-117">Na **vytvořit skupinu dostupnosti** okno, zadejte informace o vaší sady.</span><span class="sxs-lookup"><span data-stu-id="b35d9-117">On the **Create availability set** blade, complete the information for your set.</span></span>
   
    ![Snímek obrazovky s informacemi, budete muset zadat pro vytvoření sady dostupnosti.](./media/create-availability-set/create-availability-set.png)
   
   * <span data-ttu-id="b35d9-119">**Název** -název musí být 1-80 znaků skládá z čísla, písmena, tečky, podtržítka a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="b35d9-119">**Name** - the name should be 1-80 characters made up of numbers, letters, periods, underscores and dashes.</span></span> <span data-ttu-id="b35d9-120">První znak musí být písmeno nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="b35d9-120">The first character must be a letter or number.</span></span> <span data-ttu-id="b35d9-121">Poslední znak musí být písmeno, číslo nebo podtržítko.</span><span class="sxs-lookup"><span data-stu-id="b35d9-121">The last character must be a letter, number or underscore.</span></span>
   * <span data-ttu-id="b35d9-122">**Poruch domén** -domén selhání definování skupiny virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power.</span><span class="sxs-lookup"><span data-stu-id="b35d9-122">**Fault domains** - fault domains define the group of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="b35d9-123">Ve výchozím nastavení virtuální počítače jsou oddělené v rámci až tři domén selhání a můžete změnit tak, aby mezi 1 a 3.</span><span class="sxs-lookup"><span data-stu-id="b35d9-123">By default, the VMs  are separated across up to three fault domains and can be changed to between 1 and 3.</span></span>
   * <span data-ttu-id="b35d9-124">**Aktualizovat domén** – ve výchozím nastavení jsou přiřazeny pět domén aktualizace, a to může být nastaven na 1 až 20.</span><span class="sxs-lookup"><span data-stu-id="b35d9-124">**Update domains** -  five update domains are assigned by default and this can be set to between 1 and 20.</span></span> <span data-ttu-id="b35d9-125">Aktualizace domén označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="b35d9-125">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="b35d9-126">Například pokud bychom zadat, že pět aktualizovat domén, když více než pět virtuální počítače jsou nastaveny v rámci jedné skupiny dostupnosti, šesté virtuální počítač se umístí do jedné aktualizační doméně jako první virtuální počítač, sedmý ve stejné UD jako druhý virtuální počítač a tak dále.</span><span class="sxs-lookup"><span data-stu-id="b35d9-126">For example, if we specify five update domains, when more than five virtual machines are configured within a single Availability Set, the sixth virtual machine will be placed into the same update domain as the first virtual machine, the seventh in the same UD as the second virtual machine, and so on.</span></span> <span data-ttu-id="b35d9-127">Pořadí restartování počítače nemusí být po sobě jdoucích, ale bude restartován pouze jednu aktualizační doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="b35d9-127">The order of the reboots may not be sequential, but only one update domain will be rebooted at a time.</span></span>
   * <span data-ttu-id="b35d9-128">**Předplatné** -vyberte odběr, který má použít, pokud máte více než jeden.</span><span class="sxs-lookup"><span data-stu-id="b35d9-128">**Subscription** - select the subscription to use if you have more than one.</span></span>
   * <span data-ttu-id="b35d9-129">**Skupina prostředků** – klikněte na šipku a výběrem skupiny prostředků v rozevíracím seznamu vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b35d9-129">**Resource group** - select an existing resource group by clicking the arrow and selecting a resource group from the drop down.</span></span> <span data-ttu-id="b35d9-130">Můžete také vytvořit novou skupinu prostředků tak, že zadáte v názvu.</span><span class="sxs-lookup"><span data-stu-id="b35d9-130">You can also create a new resource group by typing in a name.</span></span> <span data-ttu-id="b35d9-131">Název může obsahovat následující znaky: písmena, číslice, tečky, pomlčky, podtržítka a otevírání nebo kulaté závorky.</span><span class="sxs-lookup"><span data-stu-id="b35d9-131">The name can contain any of the following characters: letters, numbers, periods, dashes, underscores and opening or closing parenthesis.</span></span> <span data-ttu-id="b35d9-132">Název nemůže končit tečkou.</span><span class="sxs-lookup"><span data-stu-id="b35d9-132">The name cannot end in a period.</span></span> <span data-ttu-id="b35d9-133">Všechny virtuální počítače ve skupině dostupnosti potřeba vytvořit ve stejné skupině prostředků jako skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="b35d9-133">All of the VMs in the availability group need to be created in the same resource group as the availability set.</span></span>
   * <span data-ttu-id="b35d9-134">**Umístění** – z rozevíracího seznamu vyberte umístění.</span><span class="sxs-lookup"><span data-stu-id="b35d9-134">**Location** - select a location from the drop-down.</span></span>
   * <span data-ttu-id="b35d9-135">**Spravované** – vyberte *Ano* k vytvoření spravovaného dostupnosti nastavení aplikaci virtuálních počítačů, které používají spravovaný disků pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="b35d9-135">**Managed** - select *Yes* to create a managed availability set to use with VMs that use Managed Disks for storage.</span></span> <span data-ttu-id="b35d9-136">Vyberte **ne** Pokud virtuální počítače, které budou v sadě používat nespravované disky v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b35d9-136">Select **No** if the VMs that will be in the set use unmanaged disks in a storage account.</span></span>
   
4. <span data-ttu-id="b35d9-137">Po dokončení zadáváte informace, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b35d9-137">When you are done entering the information, click **Create**.</span></span> 

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a><span data-ttu-id="b35d9-138">Použití portálu k vytvoření virtuálního počítače a současně sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="b35d9-138">Use the portal to create a virtual machine and an availability set at the same time</span></span>
<span data-ttu-id="b35d9-139">Při vytváření nového virtuálního počítače pomocí portálu, můžete také vytvořit novou sadu dostupnosti pro virtuální počítač při vytváření první virtuální počítač v sadě.</span><span class="sxs-lookup"><span data-stu-id="b35d9-139">If you are creating a new VM using the portal, you can also create a new availability set for the VM while you create the first VM in the set.</span></span> <span data-ttu-id="b35d9-140">Pokud se rozhodnete používat spravované disky pro virtuální počítač, bude vytvořen sadu spravovaných dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="b35d9-140">If you choose to use Managed Disks for your VM, a managed availability set will be created.</span></span>

![Snímek obrazovky, který ukazuje postup pro vytvoření nové dostupnosti nastavit při vytváření virtuálního počítače.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-to-an-existing-availability-set-in-the-portal"></a><span data-ttu-id="b35d9-142">Přidat nový virtuální počítač do existující dostupnosti nastaveným na portálu</span><span class="sxs-lookup"><span data-stu-id="b35d9-142">Add a new VM to an existing availability set in the portal</span></span>
<span data-ttu-id="b35d9-143">Každý další virtuální počítač, který vytvoříte, by měly patřit v sadě, je nutné vytvořit ve stejné **skupiny prostředků** a potom vyberte existující sadu v kroku 3 dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="b35d9-143">For each additional VM that you create that should belong in the set, make sure that you create it in the same **resource group** and then select the existing availability set in Step 3.</span></span> 

![Snímek obrazovky ukazující, jak vybrat existující sadu používat pro virtuální počítač dostupnosti.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-to-create-an-availability-set"></a><span data-ttu-id="b35d9-145">Použití prostředí PowerShell vytvořit skupinu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="b35d9-145">Use PowerShell to create an availability set</span></span>
<span data-ttu-id="b35d9-146">Tento příklad vytvoří sadu s názvem dostupnosti **myAvailabilitySet** v **myResourceGroup** skupinu prostředků **západní USA** umístění.</span><span class="sxs-lookup"><span data-stu-id="b35d9-146">This example creates an availability set named **myAvailabilitySet** in the **myResourceGroup** resource group in the **West US** location.</span></span> <span data-ttu-id="b35d9-147">To je potřeba provést před vytvořením první virtuální počítač, který bude v sadě.</span><span class="sxs-lookup"><span data-stu-id="b35d9-147">This needs to be done before you create the first VM that will be in the set.</span></span>

<span data-ttu-id="b35d9-148">Než začnete, ujistěte se, že máte nejnovější verzi modulu AzureRM.Compute prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b35d9-148">Before you begin, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="b35d9-149">Spusťte následující příkaz k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="b35d9-149">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="b35d9-150">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b35d9-150">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


<span data-ttu-id="b35d9-151">Pokud používáte spravované disky pro virtuální počítače, zadejte:</span><span class="sxs-lookup"><span data-stu-id="b35d9-151">If you are using managed disks for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" -managed
```

<span data-ttu-id="b35d9-152">Pokud používáte vlastní účty úložiště pro virtuální počítače, zadejte:</span><span class="sxs-lookup"><span data-stu-id="b35d9-152">If you are using your own storage accounts for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" 
```

<span data-ttu-id="b35d9-153">Další informace najdete v tématu [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="b35d9-153">For more information, see [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b35d9-154">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b35d9-154">Troubleshooting</span></span>
* <span data-ttu-id="b35d9-155">Při vytváření virtuálního počítače, není-li skupinu dostupnosti, které chcete v rozevíracím seznamu v portálu pravděpodobně jste vytvořili ho v jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="b35d9-155">When you create a VM, if the availability set you want isn't in the drop-down list in the portal you may have created it in a different resource group.</span></span> <span data-ttu-id="b35d9-156">Pokud si nejste jisti, skupinu prostředků pro vaše dostupnosti nastavit, přejděte do nabídky rozbočovače a klikněte na tlačítko Procházet > sady dostupnosti zobrazte seznam skupiny dostupnosti a který patří do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b35d9-156">If you don't know the resource group for your availability set, go to the hub menu and click Browse > Availability sets to see a list of your availability sets and which resource groups they belong to.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b35d9-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b35d9-157">Next steps</span></span>
<span data-ttu-id="b35d9-158">Přidejte další úložiště k virtuálnímu počítači tak, že přidáte další [datový disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b35d9-158">Add additional storage to your VM by adding an additional [data disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

