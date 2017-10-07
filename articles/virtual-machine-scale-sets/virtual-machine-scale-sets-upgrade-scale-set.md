---
title: "aaaUpgrade Azure škálovací sady virtuálních počítačů | Microsoft Docs"
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
ms.openlocfilehash: 068e98503f8d37ea71e45b8673a01da2e814f521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a><span data-ttu-id="83be5-103">Upgradovat škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="83be5-103">Upgrade a virtual machine scale set</span></span>
<span data-ttu-id="83be5-104">Tento článek popisuje, jak můžete vrátit se OS aktualizace tooan virtuální počítač Azure sad škálování bez odstávky.</span><span class="sxs-lookup"><span data-stu-id="83be5-104">This article describes how you can roll out an OS update tooan Azure virtual machine scale set without any downtime.</span></span> <span data-ttu-id="83be5-105">V tomto kontextu aktualizaci operačního systému zahrnuje změna verze hello nebo SKU hello operačního systému nebo změna hello URI vlastní image.</span><span class="sxs-lookup"><span data-stu-id="83be5-105">In this context, an OS update involves changing hello version or SKU of hello OS or changing hello URI of a custom image.</span></span> <span data-ttu-id="83be5-106">Aktualizace bez výpadku prostředků aktualizace virtuálních počítačů najednou, nebo ve skupinách (například jeden doména selhání najednou) namísto všechny najednou.</span><span class="sxs-lookup"><span data-stu-id="83be5-106">Updating without downtime means updating virtual machines one at a time or in groups (such as one fault domain at a time) rather than all at once.</span></span> <span data-ttu-id="83be5-107">Díky tomu můžete spouštět všechny virtuální počítače, které nejsou probíhá upgrade.</span><span class="sxs-lookup"><span data-stu-id="83be5-107">By doing so, any virtual machines that are not being upgraded can keep running.</span></span>

<span data-ttu-id="83be5-108">nejednoznačnosti tooavoid umožňuje rozlišit čtyři typy aktualizace operačního systému může být vhodné tooperform:</span><span class="sxs-lookup"><span data-stu-id="83be5-108">tooavoid ambiguity, let’s distinguish four types of OS update you might want tooperform:</span></span>

* <span data-ttu-id="83be5-109">Změna verze hello nebo SKU image platformy.</span><span class="sxs-lookup"><span data-stu-id="83be5-109">Changing hello version or SKU of a platform image.</span></span> <span data-ttu-id="83be5-110">Například změna Ubuntu 14.04.2-LTS verzi z 14.04.201506100 too14.04.201507060 nebo změna hello Ubuntu 15.10/nejnovější SKU too16.04.0-LTS/latest.</span><span class="sxs-lookup"><span data-stu-id="83be5-110">For example, changing Ubuntu 14.04.2-LTS version from 14.04.201506100 too14.04.201507060, or changing hello Ubuntu 15.10/latest SKU too16.04.0-LTS/latest.</span></span> <span data-ttu-id="83be5-111">Tento scénář je popsaná v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="83be5-111">This scenario is covered in this article.</span></span>
* <span data-ttu-id="83be5-112">Změna hello identifikátor URI, který ukazuje tooa novou verzi vlastní image, který jste vytvořili (**vlastnosti** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **image** > **uri**).</span><span class="sxs-lookup"><span data-stu-id="83be5-112">Changing hello URI that points tooa new version of a custom image you built (**properties** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **image** > **uri**).</span></span> <span data-ttu-id="83be5-113">Tento scénář je popsaná v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="83be5-113">This scenario is covered in this article.</span></span>
* <span data-ttu-id="83be5-114">Změna odkaz na obrázek hello škálovací sady, která byla vytvořena pomocí Azure spravované disky.</span><span class="sxs-lookup"><span data-stu-id="83be5-114">Changing hello image reference of a scale set that was created using Azure Managed Disks.</span></span>
* <span data-ttu-id="83be5-115">Opravy hello operačního systému z virtuálního počítače (to příklady instalace opravy zabezpečení a spuštění služby Windows Update).</span><span class="sxs-lookup"><span data-stu-id="83be5-115">Patching hello OS from within a virtual machine (examples of this include installing a security patch and running Windows Update).</span></span> <span data-ttu-id="83be5-116">Tento scénář je podporují, ale nejsou zahrnuté v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="83be5-116">This scenario is supported but not covered in this article.</span></span>

<span data-ttu-id="83be5-117">Sady škálování virtuálního počítače, které jsou nasazeny jako součást [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) clusteru nejsou popsané v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="83be5-117">Virtual machine scale sets that are deployed as part of an [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) cluster are not covered here.</span></span> <span data-ttu-id="83be5-118">V tématu [opravy operačního systému Windows v clusteru Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) Další informace o použití dílčích oprav Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="83be5-118">See [Patch Windows OS in your Service Fabric cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) for more information about patching Service Fabric.</span></span>

<span data-ttu-id="83be5-119">Hello základní pořadí pro změnu hello verze operačního systému skladová položka Image platformy nebo hello URI vlastní image vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="83be5-119">hello basic sequence for changing hello OS version/SKU of a platform image or hello URI of a custom image looks as follows:</span></span>

1. <span data-ttu-id="83be5-120">Získáte modelu sady škálování virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="83be5-120">Get hello virtual machine scale set model.</span></span>
2. <span data-ttu-id="83be5-121">Změnit hello verzi, SKU, odkaz na obrázek nebo hodnota identifikátoru URI v modelu hello.</span><span class="sxs-lookup"><span data-stu-id="83be5-121">Change hello version, SKU, image reference, or URI value in hello model.</span></span>
3. <span data-ttu-id="83be5-122">Aktualizace modelu hello.</span><span class="sxs-lookup"><span data-stu-id="83be5-122">Update hello model.</span></span>
4. <span data-ttu-id="83be5-123">Proveďte *manualUpgrade* volání na hello virtuální počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="83be5-123">Do a *manualUpgrade* call on hello virtual machines in hello scale set.</span></span> <span data-ttu-id="83be5-124">Tento krok platí jen pokud *upgradePolicy* je nastaven příliš**ruční** v sadě vaší škálování.</span><span class="sxs-lookup"><span data-stu-id="83be5-124">This step is only relevant if *upgradePolicy* is set too**Manual** in your scale set.</span></span> <span data-ttu-id="83be5-125">Pokud je nastaveno příliš**automatické**, všechny virtuální počítače hello upgradují najednou, což způsobuje výpadek.</span><span class="sxs-lookup"><span data-stu-id="83be5-125">If it is set too**Automatic**, all hello virtual machines are upgraded at once, thus causing downtime.</span></span>

<span data-ttu-id="83be5-126">Tyto informace v paměti a podíváme, jak může aktualizovat verzi hello měřítka, nastavte v prostředí PowerShell a pomocí hello REST API.</span><span class="sxs-lookup"><span data-stu-id="83be5-126">With this information in mind, let’s see how you could update hello version of a scale set in PowerShell, and by using hello REST API.</span></span> <span data-ttu-id="83be5-127">Tyto příklady zahrnují hello malá image platformy, ale tento článek poskytuje dostatek informací pro tooadapt můžete tento proces tooa vlastní image.</span><span class="sxs-lookup"><span data-stu-id="83be5-127">These examples cover hello case of a platform image, but this article provides enough information for you tooadapt this process tooa custom image.</span></span>

## <a name="powershell"></a><span data-ttu-id="83be5-128">PowerShell</span><span class="sxs-lookup"><span data-stu-id="83be5-128">PowerShell</span></span>
<span data-ttu-id="83be5-129">Tento příklad aktualizuje sadu škálování virtuálního počítače systému Windows (vytváření toohello novou verzi 4.0.20160229.</span><span class="sxs-lookup"><span data-stu-id="83be5-129">This example updates a Windows virtual machine scale set (creating toohello new version 4.0.20160229.</span></span> <span data-ttu-id="83be5-130">Po aktualizaci hello modelu se dělá aktualizace jedna instance virtuálního počítače v čase.</span><span class="sxs-lookup"><span data-stu-id="83be5-130">After updating hello model, it does an update one virtual machine instance at a time.</span></span>

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get hello VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update hello virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

<span data-ttu-id="83be5-131">Pokud aktualizujete hello identifikátor URI pro vlastní image místo změna image verze platformy, nahraďte hello "nastavit hello novou verzi" řádku pomocí příkazu, který se aktualizuje hello zdrojové bitové kopie identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="83be5-131">If you are updating hello URI for a custom image instead of changing a platform image version, replace hello “set hello new version” line with a command that will update hello source image URI.</span></span> <span data-ttu-id="83be5-132">Například pokud hello škálovací sadu byl vytvořen bez použití Azure spravované disky, hello aktualizace by vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="83be5-132">For example, if hello scale set was created without using Azure Managed Disks, hello update would look like this:</span></span>

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

<span data-ttu-id="83be5-133">Pokud vlastní image na základě škálovací sadu byl vytvořen pomocí spravované disky Azure, pak by aktualizovat odkaz na obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="83be5-133">If a custom image based scale set was created using Azure Managed Disks, then hello image reference would be updated.</span></span> <span data-ttu-id="83be5-134">Například:</span><span class="sxs-lookup"><span data-stu-id="83be5-134">For example:</span></span>

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="hello-rest-api"></a><span data-ttu-id="83be5-135">Hello REST API</span><span class="sxs-lookup"><span data-stu-id="83be5-135">hello REST API</span></span>
<span data-ttu-id="83be5-136">Tady je několik příkladů Python, které pomocí rozhraní REST API Azure tooroll hello se aktualizaci verze operačního systému.</span><span class="sxs-lookup"><span data-stu-id="83be5-136">Here are a couple of Python examples that use hello Azure REST API tooroll out an OS version update.</span></span> <span data-ttu-id="83be5-137">Používejte hello lightweight [azurerm](https://pypi.python.org/pypi/azurerm) knihovny rozhraní REST API Azure obálku funkce toodo GET na škálování hello nastavit modelu, za nímž následuje PUT s aktualizovaném modelu.</span><span class="sxs-lookup"><span data-stu-id="83be5-137">Both use hello lightweight [azurerm](https://pypi.python.org/pypi/azurerm) library of Azure REST API wrapper functions toodo a GET on hello scale set model, followed by a PUT with an updated model.</span></span> <span data-ttu-id="83be5-138">Se taky podívejte se na zobrazení instance virtuálního počítače tooidentify hello virtuální počítače pomocí aktualizaci domény.</span><span class="sxs-lookup"><span data-stu-id="83be5-138">They also look at virtual machine instances views tooidentify hello virtual machines by update domain.</span></span>

### <a name="vmssupgrade"></a><span data-ttu-id="83be5-139">Vmssupgrade</span><span class="sxs-lookup"><span data-stu-id="83be5-139">Vmssupgrade</span></span>
 <span data-ttu-id="83be5-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) je skript Python, který byl použit tooroll se upgradu tooa OS spuštění virtuálního počítače nastavit jednu aktualizační doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="83be5-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) is a Python script that's used tooroll out an OS upgrade tooa running virtual machine scale set one update domain at a time.</span></span>

![Skript Vmssupgrade pro výběr virtuálních počítačů nebo v doméně aktualizace](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

<span data-ttu-id="83be5-142">Tento skript umožňuje vybrat tooupdate konkrétních virtuálních počítačů nebo zadat doménu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="83be5-142">This script lets you choose specific virtual machines tooupdate or specify an update domain.</span></span> <span data-ttu-id="83be5-143">Podporuje změna image verze platformy nebo změna hello URI vlastní image.</span><span class="sxs-lookup"><span data-stu-id="83be5-143">It supports changing a platform image version or changing hello URI of a custom image.</span></span>

### <a name="vmsseditor"></a><span data-ttu-id="83be5-144">Vmsseditor</span><span class="sxs-lookup"><span data-stu-id="83be5-144">Vmsseditor</span></span>
<span data-ttu-id="83be5-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) je obecný editor pro sady škálování virtuálního počítače, které ukazuje virtuální počítač stav jako heatmap, kde jeden řádek představuje jednu aktualizační doménu.</span><span class="sxs-lookup"><span data-stu-id="83be5-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) is a general-purpose editor for virtual machine scale sets that shows virtual machine status as a heatmap where one row represents one update domain.</span></span> <span data-ttu-id="83be5-146">Kromě jiných věcí můžete aktualizovat hello model pro sadu škálování s novou verzi, SKU nebo vlastní image URI a pak vyberte tooupgrade domény selhání.</span><span class="sxs-lookup"><span data-stu-id="83be5-146">Among other things, you can update hello model for a scale set with a new version, SKU, or custom image URI, and then pick fault domains tooupgrade.</span></span> <span data-ttu-id="83be5-147">Pokud tak učiníte, jsou všechny hello virtuální počítače v této doméně aktualizace upgradovaného toohello nový model.</span><span class="sxs-lookup"><span data-stu-id="83be5-147">When you do so, all hello virtual machines in that update domain are upgraded toohello new model.</span></span> <span data-ttu-id="83be5-148">Alternativně můžete provést postupného upgradu na základě velikosti dávky hello podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="83be5-148">Alternatively, you can do a rolling upgrade based on hello batch size of your choice.</span></span>  

<span data-ttu-id="83be5-149">Hello následující snímek obrazovky ukazuje model škálování pro Ubuntu 14.04-2LTS verze 14.04.201507060 nastavit.</span><span class="sxs-lookup"><span data-stu-id="83be5-149">hello following screenshot shows a model of a scale set for Ubuntu 14.04-2LTS version 14.04.201507060.</span></span> <span data-ttu-id="83be5-150">Mnoho dalších možností přidané toothis nástroj od doby pořízení tomto snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="83be5-150">Many more options have been added toothis tool since this screenshot was taken.</span></span>

![Model Vmsseditor měřítka, nastavte pro Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

<span data-ttu-id="83be5-152">Po kliknutí na tlačítko **Upgrade** a potom **získat podrobnosti o**, virtuální počítače v UD 0 spustí tooupdate.</span><span class="sxs-lookup"><span data-stu-id="83be5-152">After you click **Upgrade** and then **Get Details**, virtual machines in UD 0 start tooupdate.</span></span>

![Probíhá aktualizace zobrazení Vmsseditor](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

