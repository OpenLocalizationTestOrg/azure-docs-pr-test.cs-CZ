---
title: "Upgrade sadu škálování virtuálního počítače Azure | Microsoft Docs"
description: "Upgrade sadu škálování virtuálního počítače Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: guybo
ms.openlocfilehash: c7093e221ff8fe69ded1cfbce4f3ddeb1a195666
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a><span data-ttu-id="5b850-103">Upgradovat škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5b850-103">Upgrade a virtual machine scale set</span></span>
<span data-ttu-id="5b850-104">Tento článek popisuje, jak můžete zavádět aktualizaci operačního systému na virtuální počítač Azure měřítko nastavit bez odstávky.</span><span class="sxs-lookup"><span data-stu-id="5b850-104">This article describes how you can roll out an OS update to an Azure virtual machine scale set without any downtime.</span></span> <span data-ttu-id="5b850-105">V tomto kontextu aktualizaci operačního systému zahrnuje změna verze nebo skladová položka operačního systému nebo změna identifikátoru URI vlastní image.</span><span class="sxs-lookup"><span data-stu-id="5b850-105">In this context, an OS update involves changing the version or SKU of the OS or changing the URI of a custom image.</span></span> <span data-ttu-id="5b850-106">Aktualizace bez výpadku prostředků aktualizace virtuálních počítačů najednou, nebo ve skupinách (například jeden doména selhání najednou) namísto všechny najednou.</span><span class="sxs-lookup"><span data-stu-id="5b850-106">Updating without downtime means updating virtual machines one at a time or in groups (such as one fault domain at a time) rather than all at once.</span></span> <span data-ttu-id="5b850-107">Díky tomu můžete spouštět všechny virtuální počítače, které nejsou probíhá upgrade.</span><span class="sxs-lookup"><span data-stu-id="5b850-107">By doing so, any virtual machines that are not being upgraded can keep running.</span></span>

<span data-ttu-id="5b850-108">Aby se zabránilo nejednoznačnosti, umožňuje rozlišit čtyři typy aktualizaci operačního systému, kterou chcete provést:</span><span class="sxs-lookup"><span data-stu-id="5b850-108">To avoid ambiguity, let’s distinguish four types of OS update you might want to perform:</span></span>

* <span data-ttu-id="5b850-109">Změna verze nebo SKU image platformy.</span><span class="sxs-lookup"><span data-stu-id="5b850-109">Changing the version or SKU of a platform image.</span></span> <span data-ttu-id="5b850-110">Například změna Ubuntu 14.04.2-LTS verzi z 14.04.201506100 14.04.201507060, nebo změna 15.10/nejnovější Ubuntu SKU na 16.04.0-LTS/latest.</span><span class="sxs-lookup"><span data-stu-id="5b850-110">For example, changing Ubuntu 14.04.2-LTS version from 14.04.201506100 to 14.04.201507060, or changing the Ubuntu 15.10/latest SKU to 16.04.0-LTS/latest.</span></span> <span data-ttu-id="5b850-111">Tento scénář je popsaná v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5b850-111">This scenario is covered in this article.</span></span>
* <span data-ttu-id="5b850-112">Změna identifikátoru URI, který odkazuje na novou verzi vlastní image vytvořené (**vlastnosti** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **image** > **uri**).</span><span class="sxs-lookup"><span data-stu-id="5b850-112">Changing the URI that points to a new version of a custom image you built (**properties** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **image** > **uri**).</span></span> <span data-ttu-id="5b850-113">Tento scénář je popsaná v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5b850-113">This scenario is covered in this article.</span></span>
* <span data-ttu-id="5b850-114">Změna referenční bitové kopie sady škálování, která byla vytvořena pomocí Azure spravované disky.</span><span class="sxs-lookup"><span data-stu-id="5b850-114">Changing the image reference of a scale set that was created using Azure Managed Disks.</span></span>
* <span data-ttu-id="5b850-115">Opravy operačního systému z virtuálního počítače (to příklady instalace opravy zabezpečení a spuštění služby Windows Update).</span><span class="sxs-lookup"><span data-stu-id="5b850-115">Patching the OS from within a virtual machine (examples of this include installing a security patch and running Windows Update).</span></span> <span data-ttu-id="5b850-116">Tento scénář je podporují, ale nejsou zahrnuté v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5b850-116">This scenario is supported but not covered in this article.</span></span>

<span data-ttu-id="5b850-117">Sady škálování virtuálního počítače, které jsou nasazeny jako součást [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) clusteru nejsou popsané v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="5b850-117">Virtual machine scale sets that are deployed as part of an [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) cluster are not covered here.</span></span> <span data-ttu-id="5b850-118">V tématu [opravy operačního systému Windows v clusteru Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) Další informace o použití dílčích oprav Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5b850-118">See [Patch Windows OS in your Service Fabric cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) for more information about patching Service Fabric.</span></span>

<span data-ttu-id="5b850-119">Změna verze nebo SKU operačního systému z image platformy nebo identifikátor URI vlastní image základní pořadí vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="5b850-119">The basic sequence for changing the OS version/SKU of a platform image or the URI of a custom image looks as follows:</span></span>

1. <span data-ttu-id="5b850-120">Získáte modelu sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b850-120">Get the virtual machine scale set model.</span></span>
2. <span data-ttu-id="5b850-121">Změna verze SKU, odkaz na obrázek nebo hodnota identifikátoru URI v modelu.</span><span class="sxs-lookup"><span data-stu-id="5b850-121">Change the version, SKU, image reference, or URI value in the model.</span></span>
3. <span data-ttu-id="5b850-122">Aktualizace modelu.</span><span class="sxs-lookup"><span data-stu-id="5b850-122">Update the model.</span></span>
4. <span data-ttu-id="5b850-123">Proveďte *manualUpgrade* volání na virtuálních počítačích v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="5b850-123">Do a *manualUpgrade* call on the virtual machines in the scale set.</span></span> <span data-ttu-id="5b850-124">Tento krok platí jen pokud *upgradePolicy* je nastaven na **ruční** v sadě vaší škálování.</span><span class="sxs-lookup"><span data-stu-id="5b850-124">This step is only relevant if *upgradePolicy* is set to **Manual** in your scale set.</span></span> <span data-ttu-id="5b850-125">Pokud je nastaven na hodnotu **automatické**, všechny virtuální počítače jsou upgradovány najednou, což způsobuje výpadek.</span><span class="sxs-lookup"><span data-stu-id="5b850-125">If it is set to **Automatic**, all the virtual machines are upgraded at once, thus causing downtime.</span></span>

<span data-ttu-id="5b850-126">Tyto informace v paměti a podíváme, jak může aktualizovat verzi škálování, nastavte v prostředí PowerShell a pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="5b850-126">With this information in mind, let’s see how you could update the version of a scale set in PowerShell, and by using the REST API.</span></span> <span data-ttu-id="5b850-127">Tyto příklady zahrnují v případě image platformy, ale tento článek poskytuje dostatek informací, můžete přizpůsobit tento proces na vlastní image.</span><span class="sxs-lookup"><span data-stu-id="5b850-127">These examples cover the case of a platform image, but this article provides enough information for you to adapt this process to a custom image.</span></span>

## <a name="powershell"></a><span data-ttu-id="5b850-128">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5b850-128">PowerShell</span></span>
<span data-ttu-id="5b850-129">Tento příklad aktualizace škálování virtuálních počítačů Windows nastavte (vytváření 4.0.20160229 na novou verzi.</span><span class="sxs-lookup"><span data-stu-id="5b850-129">This example updates a Windows virtual machine scale set (creating to the new version 4.0.20160229.</span></span> <span data-ttu-id="5b850-130">Po aktualizaci modelu se dělá instance aktualizace jeden virtuální počítač najednou.</span><span class="sxs-lookup"><span data-stu-id="5b850-130">After updating the model, it does an update one virtual machine instance at a time.</span></span>

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

<span data-ttu-id="5b850-131">Při aktualizaci identifikátor URI pro vlastní image místo změna image verze platformy, nahraďte "nastavena na novou verzi" řádku příkaz, který se bude aktualizovat zdrojové bitové kopie identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="5b850-131">If you are updating the URI for a custom image instead of changing a platform image version, replace the “set the new version” line with a command that will update the source image URI.</span></span> <span data-ttu-id="5b850-132">Například pokud byly sadou škálování byl vytvořen bez použití Azure spravované disky, aktualizace bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="5b850-132">For example, if the scale set was created without using Azure Managed Disks, the update would look like this:</span></span>

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

<span data-ttu-id="5b850-133">Pokud vlastní image na základě škálovací sadu byl vytvořen pomocí spravované disky Azure, pak by aktualizovat odkaz na obrázek.</span><span class="sxs-lookup"><span data-stu-id="5b850-133">If a custom image based scale set was created using Azure Managed Disks, then the image reference would be updated.</span></span> <span data-ttu-id="5b850-134">Například:</span><span class="sxs-lookup"><span data-stu-id="5b850-134">For example:</span></span>

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="the-rest-api"></a><span data-ttu-id="5b850-135">Rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="5b850-135">The REST API</span></span>
<span data-ttu-id="5b850-136">Tady je několik příkladů Python, které zavádět aktualizaci verze operačního systému pomocí rozhraní REST API Azure.</span><span class="sxs-lookup"><span data-stu-id="5b850-136">Here are a couple of Python examples that use the Azure REST API to roll out an OS version update.</span></span> <span data-ttu-id="5b850-137">Jak používat jednoduchá [azurerm](https://pypi.python.org/pypi/azurerm) knihovna funkcí rozhraní REST API Azure obálku udělat GET na rozsahu nastavit modelu, za nímž následuje PUT s aktualizovaném modelu.</span><span class="sxs-lookup"><span data-stu-id="5b850-137">Both use the lightweight [azurerm](https://pypi.python.org/pypi/azurerm) library of Azure REST API wrapper functions to do a GET on the scale set model, followed by a PUT with an updated model.</span></span> <span data-ttu-id="5b850-138">Také zobrazit v zobrazení instance virtuálního počítače můžete identifikovat virtuální počítače podle aktualizaci domény.</span><span class="sxs-lookup"><span data-stu-id="5b850-138">They also look at virtual machine instances views to identify the virtual machines by update domain.</span></span>

### <a name="vmssupgrade"></a><span data-ttu-id="5b850-139">Vmssupgrade</span><span class="sxs-lookup"><span data-stu-id="5b850-139">Vmssupgrade</span></span>
 <span data-ttu-id="5b850-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) je skript jazyka Python, který se používá k zavedení upgrade operačního systému spuštěné škálování virtuálního počítače nastavit jednu aktualizační doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="5b850-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) is a Python script that's used to roll out an OS upgrade to a running virtual machine scale set one update domain at a time.</span></span>

![Skript Vmssupgrade pro výběr virtuálních počítačů nebo v doméně aktualizace](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

<span data-ttu-id="5b850-142">Tento skript umožňuje vybrat konkrétní virtuální počítače a aktualizovat nebo zadejte doménu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5b850-142">This script lets you choose specific virtual machines to update or specify an update domain.</span></span> <span data-ttu-id="5b850-143">Podporuje změna image verze platformy nebo změna identifikátoru URI vlastní image.</span><span class="sxs-lookup"><span data-stu-id="5b850-143">It supports changing a platform image version or changing the URI of a custom image.</span></span>

### <a name="vmsseditor"></a><span data-ttu-id="5b850-144">Vmsseditor</span><span class="sxs-lookup"><span data-stu-id="5b850-144">Vmsseditor</span></span>
<span data-ttu-id="5b850-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) je obecný editor pro sady škálování virtuálního počítače, které ukazuje virtuální počítač stav jako heatmap, kde jeden řádek představuje jednu aktualizační doménu.</span><span class="sxs-lookup"><span data-stu-id="5b850-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) is a general-purpose editor for virtual machine scale sets that shows virtual machine status as a heatmap where one row represents one update domain.</span></span> <span data-ttu-id="5b850-146">Kromě jiných věcí můžete aktualizovat model pro sadu škálování s novou verzi, SKU nebo vlastní image URI a pak vyberte domén selhání upgradu.</span><span class="sxs-lookup"><span data-stu-id="5b850-146">Among other things, you can update the model for a scale set with a new version, SKU, or custom image URI, and then pick fault domains to upgrade.</span></span> <span data-ttu-id="5b850-147">Pokud tak učiníte, všechny virtuální počítače v této doméně aktualizace upgradují na nový model.</span><span class="sxs-lookup"><span data-stu-id="5b850-147">When you do so, all the virtual machines in that update domain are upgraded to the new model.</span></span> <span data-ttu-id="5b850-148">Alternativně můžete provést postupného upgradu podle velikost dávky podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="5b850-148">Alternatively, you can do a rolling upgrade based on the batch size of your choice.</span></span>  

<span data-ttu-id="5b850-149">Následující snímek obrazovky ukazuje model škálování pro Ubuntu 14.04-2LTS verze 14.04.201507060 nastavit.</span><span class="sxs-lookup"><span data-stu-id="5b850-149">The following screenshot shows a model of a scale set for Ubuntu 14.04-2LTS version 14.04.201507060.</span></span> <span data-ttu-id="5b850-150">Mnoho dalších možností byly přidány k tomuto nástroji od doby pořízení tomto snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="5b850-150">Many more options have been added to this tool since this screenshot was taken.</span></span>

![Model Vmsseditor měřítka, nastavte pro Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

<span data-ttu-id="5b850-152">Po kliknutí na tlačítko **Upgrade** a potom **získat podrobnosti o**, virtuální počítače v UD 0 spustí aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="5b850-152">After you click **Upgrade** and then **Get Details**, virtual machines in UD 0 start to update.</span></span>

![Probíhá aktualizace zobrazení Vmsseditor](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

