---
title: "nastavit aaaCreate dostupnosti virtuálních počítačů v Azure | Microsoft Docs"
description: "Zjistěte, jak nastavit toocreate spravované dostupnosti nebo nespravované dostupnosti pro virtuální počítače pomocí prostředí Azure PowerShell nastavit nebo hello portálu v modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: eadcdfcd28bb2fa21a4647f207b390c33e022ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a><span data-ttu-id="8bc87-104">Zvýšení dostupnosti virtuálních počítačů tak, že vytvoříte skupinu dostupnosti Azure</span><span class="sxs-lookup"><span data-stu-id="8bc87-104">Increase VM availability by creating an Azure availability set</span></span> 
<span data-ttu-id="8bc87-105">Skupiny dostupnosti poskytují redundanci tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="8bc87-105">Availability sets provide redundancy tooyour application.</span></span> <span data-ttu-id="8bc87-106">Doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="8bc87-106">We recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="8bc87-107">Tato konfigurace zajistí, že během buď plánované i neplánované údržby alespoň jeden virtuální počítač bude k dispozici a plnění hello 99,95 % smlouva Azure SLA.</span><span class="sxs-lookup"><span data-stu-id="8bc87-107">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine will be available and meet hello 99.95% Azure SLA.</span></span> <span data-ttu-id="8bc87-108">Další informace najdete v tématu hello [SLA pro virtuální počítače](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="8bc87-108">For more information, see hello [SLA for Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8bc87-109">Virtuální počítače musí být vytvořen v hello stejné skupině prostředků jako skupinu dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="8bc87-109">VMs must be created in hello same resource group as hello availability set.</span></span>
> 

<span data-ttu-id="8bc87-110">Pokud chcete, aby váš virtuální počítač toobe součástí skupiny dostupnosti, je třeba toocreate hello dostupnosti nastavit první nebo při vytváření vašeho prvního virtuálního počítače v sadě hello.</span><span class="sxs-lookup"><span data-stu-id="8bc87-110">If you want your VM toobe part of an availability set, you need toocreate hello availability set first or while you are creating your first VM in hello set.</span></span> <span data-ttu-id="8bc87-111">Pokud virtuální počítač bude používat spravované disků, musí být vytvořeny skupinu dostupnosti hello jako sadu spravovaných dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="8bc87-111">If your VM will be using Managed Disks, hello availability set must be created as a managed availability set.</span></span>

<span data-ttu-id="8bc87-112">Další informace o vytváření a použití skupiny dostupnosti najdete v tématu [spravovat hello dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8bc87-112">For more information about creating and using availability sets, see [Manage hello availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="use-hello-portal-toocreate-an-availability-set-before-creating-your-vms"></a><span data-ttu-id="8bc87-113">Použití portálu toocreate hello před vytvořením virtuálních počítačů sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="8bc87-113">Use hello portal toocreate an availability set before creating your VMs</span></span>
1. <span data-ttu-id="8bc87-114">V nabídce centra hello, klikněte na **Procházet** a vyberte **sady dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="8bc87-114">In hello hub menu, click **Browse** and select **Availability sets**.</span></span>
2. <span data-ttu-id="8bc87-115">Na hello **sady dostupnosti okno**, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8bc87-115">On hello **Availability sets blade**, click **Add**.</span></span>
   
    ![Snímek obrazovky zobrazující hello přidat tlačítko pro vytvoření nové sady dostupnosti.](./media/create-availability-set/add-availability-set.png)
3. <span data-ttu-id="8bc87-117">Na hello **vytvořit skupinu dostupnosti** okno, informace o dokončení hello vaší sady.</span><span class="sxs-lookup"><span data-stu-id="8bc87-117">On hello **Create availability set** blade, complete hello information for your set.</span></span>
   
    ![Snímek obrazovky, ukazuje hello informace tooenter toocreate hello dostupnosti je potřeba nastavit.](./media/create-availability-set/create-availability-set.png)
   
   * <span data-ttu-id="8bc87-119">**Název** -hello název by měl být 1-80 znaků skládá z čísla, písmena, tečky, podtržítka a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="8bc87-119">**Name** - hello name should be 1-80 characters made up of numbers, letters, periods, underscores and dashes.</span></span> <span data-ttu-id="8bc87-120">Hello první znak musí být písmeno nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="8bc87-120">hello first character must be a letter or number.</span></span> <span data-ttu-id="8bc87-121">Hello poslední znak musí být písmeno, číslo nebo podtržítko.</span><span class="sxs-lookup"><span data-stu-id="8bc87-121">hello last character must be a letter, number or underscore.</span></span>
   * <span data-ttu-id="8bc87-122">**Poruch domén** -domén selhání definovat hello skupinu virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power.</span><span class="sxs-lookup"><span data-stu-id="8bc87-122">**Fault domains** - fault domains define hello group of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="8bc87-123">Ve výchozím nastavení hello virtuální počítače jsou oddělené v rámci až toothree domén selhání a může být změněné toobetween 1 a 3.</span><span class="sxs-lookup"><span data-stu-id="8bc87-123">By default, hello VMs  are separated across up toothree fault domains and can be changed toobetween 1 and 3.</span></span>
   * <span data-ttu-id="8bc87-124">**Aktualizovat domén** – ve výchozím nastavení jsou přiřazeny pět domén aktualizace, a to je možné nastavit toobetween 1 až 20.</span><span class="sxs-lookup"><span data-stu-id="8bc87-124">**Update domains** -  five update domains are assigned by default and this can be set toobetween 1 and 20.</span></span> <span data-ttu-id="8bc87-125">Aktualizace domén označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="8bc87-125">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="8bc87-126">Například pokud bychom zadat pět aktualizace, které domény, když je více než pět virtuální počítače jsou nastaveny v rámci jedné dostupnosti sadě, hello šesté virtuální počítač se umístí do hello jedné aktualizační doméně jako hello první virtuální počítač hello sedmá v hello, stejné UD jako hello druhý virtuální počítač a tak dále.</span><span class="sxs-lookup"><span data-stu-id="8bc87-126">For example, if we specify five update domains, when more than five virtual machines are configured within a single Availability Set, hello sixth virtual machine will be placed into hello same update domain as hello first virtual machine, hello seventh in hello same UD as hello second virtual machine, and so on.</span></span> <span data-ttu-id="8bc87-127">Hello pořadí hello restartování nemusí být po sobě jdoucích, ale bude restartován pouze jednu aktualizační doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="8bc87-127">hello order of hello reboots may not be sequential, but only one update domain will be rebooted at a time.</span></span>
   * <span data-ttu-id="8bc87-128">**Předplatné** -vyberte hello toouse předplatné, pokud máte více než jednou.</span><span class="sxs-lookup"><span data-stu-id="8bc87-128">**Subscription** - select hello subscription toouse if you have more than one.</span></span>
   * <span data-ttu-id="8bc87-129">**Skupina prostředků** -vyberte existující skupinu prostředků, klikněte na šipku hello a výběrem skupinu prostředků z hello rozevírací nabídku.</span><span class="sxs-lookup"><span data-stu-id="8bc87-129">**Resource group** - select an existing resource group by clicking hello arrow and selecting a resource group from hello drop down.</span></span> <span data-ttu-id="8bc87-130">Můžete také vytvořit novou skupinu prostředků tak, že zadáte v názvu.</span><span class="sxs-lookup"><span data-stu-id="8bc87-130">You can also create a new resource group by typing in a name.</span></span> <span data-ttu-id="8bc87-131">Hello název může obsahovat jakýkoli z hello následující znaky: písmena, číslice, tečky, pomlčky, podtržítka a otevírání nebo kulaté závorky.</span><span class="sxs-lookup"><span data-stu-id="8bc87-131">hello name can contain any of hello following characters: letters, numbers, periods, dashes, underscores and opening or closing parenthesis.</span></span> <span data-ttu-id="8bc87-132">Hello název nemůže končit tečkou.</span><span class="sxs-lookup"><span data-stu-id="8bc87-132">hello name cannot end in a period.</span></span> <span data-ttu-id="8bc87-133">Všechny hello virtuální počítače ve skupině dostupnosti hello musí toobe vytvořené v hello stejné skupině prostředků jako skupinu dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="8bc87-133">All of hello VMs in hello availability group need toobe created in hello same resource group as hello availability set.</span></span>
   * <span data-ttu-id="8bc87-134">**Umístění** -vyberte z rozevíracího seznamu hello umístění.</span><span class="sxs-lookup"><span data-stu-id="8bc87-134">**Location** - select a location from hello drop-down.</span></span>
   * <span data-ttu-id="8bc87-135">**Spravované** – vyberte *Ano* toocreate spravované dostupnosti nastavit toouse s virtuálními počítači, které používají spravovaný disků pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="8bc87-135">**Managed** - select *Yes* toocreate a managed availability set toouse with VMs that use Managed Disks for storage.</span></span> <span data-ttu-id="8bc87-136">Vyberte **ne** Pokud hello virtuálních počítačů, které budou v sadě hello použijte nespravované disky v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8bc87-136">Select **No** if hello VMs that will be in hello set use unmanaged disks in a storage account.</span></span>
   
4. <span data-ttu-id="8bc87-137">Při zadávání informací hello skončíte, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8bc87-137">When you are done entering hello information, click **Create**.</span></span> 

## <a name="use-hello-portal-toocreate-a-virtual-machine-and-an-availability-set-at-hello-same-time"></a><span data-ttu-id="8bc87-138">Použití portálu toocreate hello virtuální počítač a dostupnosti nastavit na hello stejný čas</span><span class="sxs-lookup"><span data-stu-id="8bc87-138">Use hello portal toocreate a virtual machine and an availability set at hello same time</span></span>
<span data-ttu-id="8bc87-139">Pokud vytváříte nový virtuální počítač pomocí hello portálu, můžete také vytvořit novou sadu dostupnosti pro hello virtuálních počítačů při vytváření hello první virtuální počítač v sadě hello.</span><span class="sxs-lookup"><span data-stu-id="8bc87-139">If you are creating a new VM using hello portal, you can also create a new availability set for hello VM while you create hello first VM in hello set.</span></span> <span data-ttu-id="8bc87-140">Pokud si zvolíte toouse spravované disky pro virtuální počítač, bude vytvořen sadu spravovaných dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="8bc87-140">If you choose toouse Managed Disks for your VM, a managed availability set will be created.</span></span>

![Snímek obrazovky zobrazující hello proces vytvoření nového dostupnosti nastavit při vytváření hello virtuálních počítačů.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-tooan-existing-availability-set-in-hello-portal"></a><span data-ttu-id="8bc87-142">Přidat nový virtuální počítač tooan stávající sadu dostupnosti hello portálu</span><span class="sxs-lookup"><span data-stu-id="8bc87-142">Add a new VM tooan existing availability set in hello portal</span></span>
<span data-ttu-id="8bc87-143">Pro každý další virtuální počítač, který vytvoříte, by měl patřit sady hello, ujistěte se, vytvořte ji v hello stejné **skupiny prostředků** a pak vyberte hello existující dostupnosti v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="8bc87-143">For each additional VM that you create that should belong in hello set, make sure that you create it in hello same **resource group** and then select hello existing availability set in Step 3.</span></span> 

![Snímek obrazovky ukazující, jak nastavit tooselect existující dostupnosti toouse pro virtuální počítač.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-toocreate-an-availability-set"></a><span data-ttu-id="8bc87-145">Pomocí prostředí PowerShell toocreate dostupnosti nastavit</span><span class="sxs-lookup"><span data-stu-id="8bc87-145">Use PowerShell toocreate an availability set</span></span>
<span data-ttu-id="8bc87-146">Tento příklad vytvoří sadu s názvem dostupnosti **myAvailabilitySet** v hello **myResourceGroup** skupiny prostředků v hello **západní USA** umístění.</span><span class="sxs-lookup"><span data-stu-id="8bc87-146">This example creates an availability set named **myAvailabilitySet** in hello **myResourceGroup** resource group in hello **West US** location.</span></span> <span data-ttu-id="8bc87-147">Tento krok je nutné provést před vytvořením hello toobe první virtuální počítač, který bude v sadě hello.</span><span class="sxs-lookup"><span data-stu-id="8bc87-147">This needs toobe done before you create hello first VM that will be in hello set.</span></span>

<span data-ttu-id="8bc87-148">Než začnete, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="8bc87-148">Before you begin, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="8bc87-149">Spustit hello následující příkaz tooinstall.</span><span class="sxs-lookup"><span data-stu-id="8bc87-149">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="8bc87-150">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8bc87-150">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


<span data-ttu-id="8bc87-151">Pokud používáte spravované disky pro virtuální počítače, zadejte:</span><span class="sxs-lookup"><span data-stu-id="8bc87-151">If you are using managed disks for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" -managed
```

<span data-ttu-id="8bc87-152">Pokud používáte vlastní účty úložiště pro virtuální počítače, zadejte:</span><span class="sxs-lookup"><span data-stu-id="8bc87-152">If you are using your own storage accounts for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" 
```

<span data-ttu-id="8bc87-153">Další informace najdete v tématu [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="8bc87-153">For more information, see [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="8bc87-154">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="8bc87-154">Troubleshooting</span></span>
* <span data-ttu-id="8bc87-155">Při vytváření virtuálního počítače, pokud skupina dostupnosti hello, které chcete není v rozevíracím seznamu hello portálu hello pravděpodobně vytvořena ho v jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="8bc87-155">When you create a VM, if hello availability set you want isn't in hello drop-down list in hello portal you may have created it in a different resource group.</span></span> <span data-ttu-id="8bc87-156">Pokud si nejste jisti hello skupinu prostředků pro vaše dostupnosti nastavit, přejděte v nabídce centra toohello a klikněte na tlačítko Procházet > toosee nastaví seznam vaší dostupnosti a které skupiny prostředků, které patří do sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="8bc87-156">If you don't know hello resource group for your availability set, go toohello hub menu and click Browse > Availability sets toosee a list of your availability sets and which resource groups they belong to.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bc87-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8bc87-157">Next steps</span></span>
<span data-ttu-id="8bc87-158">Přidejte další úložiště tooyour virtuálních počítačů tak, že přidáte další [datový disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8bc87-158">Add additional storage tooyour VM by adding an additional [data disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

