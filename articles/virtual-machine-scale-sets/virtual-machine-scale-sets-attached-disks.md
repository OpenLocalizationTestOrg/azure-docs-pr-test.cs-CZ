---
title: "Virtuální počítač škálování sady dat disky připojené aaaAzure | Microsoft Docs"
description: "Zjistěte, jak připojit toouse datových disků s sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: guybo
ms.openlocfilehash: 77b66f80934c0aaf7bb1ad0de00a738052a878ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a><span data-ttu-id="797df-103">Škálovací sady virtuálních počítačů Azure a připojené datové disky</span><span class="sxs-lookup"><span data-stu-id="797df-103">Azure VM scale sets and attached data disks</span></span>
<span data-ttu-id="797df-104">[Škálovací sady virtuálních počítačů](/azure/virtual-machine-scale-sets/) Azure teď podporují virtuální počítače s připojenými datovými disky.</span><span class="sxs-lookup"><span data-stu-id="797df-104">Azure [virtual machine scale sets](/azure/virtual-machine-scale-sets/) now support virtual machines with attached data disks.</span></span> <span data-ttu-id="797df-105">V profilu úložiště hello sady škálování, které byly vytvořeny s disky spravované Azure může být definováno datových disků.</span><span class="sxs-lookup"><span data-stu-id="797df-105">Data disks can be defined in hello storage profile for scale sets that have been created with Azure Managed Disks.</span></span> <span data-ttu-id="797df-106">Dříve hello pouze možnosti přímo připojené úložiště k dispozici s virtuálními počítači v sadách škálování byly jednotky hello operačního systému a dočasného jednotek.</span><span class="sxs-lookup"><span data-stu-id="797df-106">Previously hello only directly attached storage options available with VMs in scale sets were hello OS drive and temp drives.</span></span>

> [!NOTE]
>  <span data-ttu-id="797df-107">Při vytváření škálování s připojené datových disků, které jsou definované, budete potřebovat toomount a formát hello disky z v rámci virtuálního počítače toouse je (pro samostatnou virtuální počítače Azure jenom jako).</span><span class="sxs-lookup"><span data-stu-id="797df-107">When you create a scale set with attached data disks defined, you still need toomount and format hello disks from within a VM toouse them (just like for standalone Azure VMs).</span></span> <span data-ttu-id="797df-108">Toodo pohodlný způsob je toouse rozšíření vlastních skriptů, které volá toopartition standardní skripty a naformátovat všechny hello datové disky na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="797df-108">A convenient way toodo this is toouse a custom script extension which calls a standard script toopartition and format all hello data disks on a VM.</span></span>

## <a name="create-a-scale-set-with-attached-data-disks"></a><span data-ttu-id="797df-109">Vytvoření škálovací sady s připojenými datovými disky</span><span class="sxs-lookup"><span data-stu-id="797df-109">Create a scale set with attached data disks</span></span>
<span data-ttu-id="797df-110">Nastavit škálování s připojené disky jednoduchý způsob toocreate je toouse hello [rozhraní příkazového řádku Azure](https://github.com/Azure/azure-cli) _vmss vytvořit_ příkaz.</span><span class="sxs-lookup"><span data-stu-id="797df-110">A simple way toocreate a scale set with attached disks is toouse hello [Azure CLI](https://github.com/Azure/azure-cli) _vmss create_ command.</span></span> <span data-ttu-id="797df-111">Hello následující ukázka vytvoří skupinu prostředků Azure a sadu škálování virtuálního počítače 10 virtuálních Ubuntu počítačů, každý s 2 připojených datových disků, 50 GB a 100 GB.</span><span class="sxs-lookup"><span data-stu-id="797df-111">hello following example creates an Azure resource group, and a VM scale set of 10 Ubuntu VMs, each with 2 attached data disks, of 50 GB and 100 GB respectively.</span></span>
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
<span data-ttu-id="797df-112">Všimněte si, že hello _vmss vytvořit_ příkaz výchozí určité hodnoty konfigurace, pokud nezadáte je.</span><span class="sxs-lookup"><span data-stu-id="797df-112">Note that hello _vmss create_ command defaults certain configuration values if you do not specify them.</span></span> <span data-ttu-id="797df-113">toosee hello dostupné možnosti, můžete přepsat zkuste:</span><span class="sxs-lookup"><span data-stu-id="797df-113">toosee hello available options that you can override try:</span></span>
```bash
az vmss create --help
```
<span data-ttu-id="797df-114">Zahrnout další způsob toocreate škálování s připojené datových disků je toodefine nastavení škálování v šablonu Azure Resource Manager _dataDisks_ část v hello _storageProfile_a nasadit hello Šablona.</span><span class="sxs-lookup"><span data-stu-id="797df-114">Another way toocreate a scale set with attached data disks is toodefine a scale set in an Azure Resource Manager template, include a _dataDisks_ section in hello _storageProfile_, and deploy hello template.</span></span> <span data-ttu-id="797df-115">Hello 50 GB a 100 GB disk výše uvedeném příkladu by být definován jako to v šabloně hello:</span><span class="sxs-lookup"><span data-stu-id="797df-115">hello 50 GB and 100 GB disk example above would be defined like this in hello template:</span></span>
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```
<span data-ttu-id="797df-116">Můžete zobrazit příklad dokončena, připraveno toodeploy šablony sady škálování s připojený disk zde definované: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).</span><span class="sxs-lookup"><span data-stu-id="797df-116">You can see a complete, ready toodeploy example of a scale set template with an attached disk defined here: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).</span></span>

## <a name="adding-a-data-disk-tooan-existing-scale-set"></a><span data-ttu-id="797df-117">Přidání existující škálování dat disku tooan nastavit</span><span class="sxs-lookup"><span data-stu-id="797df-117">Adding a data disk tooan existing scale set</span></span>
> [!NOTE]
>  <span data-ttu-id="797df-118">Je možné ještě připojit datové disky tooa škálování sady, který byl vytvořen s [Azure spravované disky](./virtual-machine-scale-sets-managed-disks.md).</span><span class="sxs-lookup"><span data-stu-id="797df-118">You can only attach data disks tooa scale set which has been created with [Azure Managed Disks](./virtual-machine-scale-sets-managed-disks.md).</span></span>

<span data-ttu-id="797df-119">Můžete přidat datový disk tooa virtuálních počítačů měřítkem nastavit pomocí rozhraní příkazového řádku Azure _připojit az vmss disk_ příkaz.</span><span class="sxs-lookup"><span data-stu-id="797df-119">You can add a data disk tooa VM scale set using Azure CLI _az vmss disk attach_ command.</span></span> <span data-ttu-id="797df-120">Ujistěte se, že zadáváte logickou jednotku (LUN), která se ještě nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="797df-120">Make sure you specify a lun which is not already in use.</span></span> <span data-ttu-id="797df-121">Následující ukázka rozhraní příkazového řádku Hello přidá 50 GB jednotky toolun 3:</span><span class="sxs-lookup"><span data-stu-id="797df-121">hello following CLI example adds a 50 GB drive toolun 3:</span></span>
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

<span data-ttu-id="797df-122">Následující příklad PowerShell Hello přidá 50 GB jednotky toolun 3:</span><span class="sxs-lookup"><span data-stu-id="797df-122">hello following PowerShell example adds a 50 GB drive toolun 3:</span></span>
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> <span data-ttu-id="797df-123">Různé velikosti virtuálních počítačů mají různé limity z hello počty připojené disky, které podporují.</span><span class="sxs-lookup"><span data-stu-id="797df-123">Different VM sizes have different limits on hello numbers of attached drives they support.</span></span> <span data-ttu-id="797df-124">Zkontrolujte hello [charakteristiky velikost virtuálního počítače](../virtual-machines/windows/sizes.md) před přidáním nového disku.</span><span class="sxs-lookup"><span data-stu-id="797df-124">Check hello [virtual machine size characteristics](../virtual-machines/windows/sizes.md) before adding a new disk.</span></span>

<span data-ttu-id="797df-125">Můžete také přidat disk přidáním nové položky toohello _dataDisks_ vlastnost hello _storageProfile_ rozsahu nastavit definice a použití změn hello.</span><span class="sxs-lookup"><span data-stu-id="797df-125">You can also add a disk by adding a new entry toohello _dataDisks_ property in hello _storageProfile_ of a scale set definition and applying hello change.</span></span> <span data-ttu-id="797df-126">tootest se najít existující definici sady škálování v hello [Průzkumníka prostředků Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="797df-126">tootest this, find an existing scale set definition in hello [Azure Resource Explorer](https://resources.azure.com/).</span></span> <span data-ttu-id="797df-127">Vyberte _upravit_ a přidejte nový disk toohello seznam datových disků.</span><span class="sxs-lookup"><span data-stu-id="797df-127">Select _Edit_ and add a new disk toohello list of data disks.</span></span> <span data-ttu-id="797df-128">Například</span><span class="sxs-lookup"><span data-stu-id="797df-128">E.g.</span></span> <span data-ttu-id="797df-129">pomocí výše uvedeném příkladu hello:</span><span class="sxs-lookup"><span data-stu-id="797df-129">using hello example above:</span></span>
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

<span data-ttu-id="797df-130">Potom vyberte _PUT_ tooapply hello změny tooyour škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="797df-130">Then select _PUT_ tooapply hello changes tooyour scale set.</span></span> <span data-ttu-id="797df-131">Tento příklad bude fungovat, pokud použijete velikost virtuálních počítačů, která podporuje více než dva připojené datové disky.</span><span class="sxs-lookup"><span data-stu-id="797df-131">This example would work as long as you are using a VM size which supports more than two attached data disks.</span></span>

> [!NOTE]
> <span data-ttu-id="797df-132">Při provádění změn tooa škálování nastavit definice například přidáním nebo odebráním datový disk, platí tooall nově vytvořený virtuální počítače, ale virtuální počítače tooexisting platí pouze v případě hello _upgradePolicy_ vlastnost je nastavena příliš "automatické".</span><span class="sxs-lookup"><span data-stu-id="797df-132">When you make a change tooa scale set definition such as adding or removing a data disk, it applies tooall newly created VMs, but only applies tooexisting VMs if hello _upgradePolicy_ property is set too"Automatic".</span></span> <span data-ttu-id="797df-133">Pokud je nastaven příliš "Ruční", je třeba toomanually použít nový model tooexisting hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="797df-133">If it is set too"Manual", you need toomanually apply hello new model tooexisting VMs.</span></span> <span data-ttu-id="797df-134">To provedete hello portálu pomocí hello _aktualizace AzureRmVmssInstance_ prostředí PowerShell příkaz nebo pomocí hello _az vmss aktualizace instance_ rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="797df-134">You can do this in hello portal, using hello _Update-AzureRmVmssInstance_ PowerShell command, or using hello _az vmss update-instances_ CLI command.</span></span>

## <a name="adding-pre-populated-data-disks-tooan-existent-scale-set"></a><span data-ttu-id="797df-135">Přidání předem vyplněná datové disky tooan stávající škálovací sadu</span><span class="sxs-lookup"><span data-stu-id="797df-135">Adding pre-populated data disks tooan existent scale set</span></span> 
> <span data-ttu-id="797df-136">Při přidávání disků tooan existující sady škálování model návrhu, hello disk bude vždy vytvořit prázdný.</span><span class="sxs-lookup"><span data-stu-id="797df-136">When you add disks tooan existent scale set model, by design, hello disk will always be created empty.</span></span> <span data-ttu-id="797df-137">Tento scénář také zahrnuje nové instance vytvořené sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="797df-137">This scenario also includes new instances created by hello scale set.</span></span> <span data-ttu-id="797df-138">Toto chování je, protože definice scaleset hello má prázdný datový disk.</span><span class="sxs-lookup"><span data-stu-id="797df-138">This behaviour is because hello scaleset definition has an empty data disk.</span></span> <span data-ttu-id="797df-139">V pořadí toocreate předem vyplněná datové jednotky pro model sadu svědčící škálování můžete zvolit jednu z následujících dvou možností:</span><span class="sxs-lookup"><span data-stu-id="797df-139">In order toocreate pre-populated data drives for an existent scale set model, you can choose either of next two options:</span></span>

* <span data-ttu-id="797df-140">Zkopírování dat z hello instanci 0 virtuálních počítačů toohello datové disky v hello ostatních virtuálních počítačů pomocí vlastního skriptu.</span><span class="sxs-lookup"><span data-stu-id="797df-140">Copy data from hello instance 0 VM toohello data disk(s) in hello other VMs by running a custom script.</span></span>
* <span data-ttu-id="797df-141">Vytvoření bitové kopie spravované se disk hello operačního systému a datový disk (s daty hello vyžaduje) a vytvořte nové scaleset s bitovou kopií hello.</span><span class="sxs-lookup"><span data-stu-id="797df-141">Create a managed image with hello OS disk plus data disk (with hello required data) and create a new scaleset with hello image.</span></span> <span data-ttu-id="797df-142">Tímto způsobem každých vytvořit nový virtuální počítač bude mít data na disku, která je součástí definice hello hello scaleset.</span><span class="sxs-lookup"><span data-stu-id="797df-142">This way every new VM created will have a data disk that that is provided in hello definition of hello scaleset.</span></span> <span data-ttu-id="797df-143">Vzhledem k tomu, že tuto definici se budou vztahovat tooan bitovou kopii s datový disk, který má přizpůsobený dat, každý virtuální počítač na hello scaleset bude automaticky spuštěna s tyto změny.</span><span class="sxs-lookup"><span data-stu-id="797df-143">Since this definition will refer tooan image with a data disk that has customized data, every virtual machine on hello scaleset will automatically come up with these changes.</span></span>

> <span data-ttu-id="797df-144">Hello způsob toocreate vlastní image naleznete zde: [vytvoříte spravované bitovou kopii zobecněný virtuální počítač v Azure](/azure/virtual-machines/windows/capture-image-resource/)</span><span class="sxs-lookup"><span data-stu-id="797df-144">hello way toocreate a custom image can be found here: [Create a managed image of a generalized VM in Azure](/azure/virtual-machines/windows/capture-image-resource/)</span></span> 

> <span data-ttu-id="797df-145">Hello uživatel potřebuje toocapture hello instance 0 virtuálního počítače, který má hello požadovaná data a pak použijte tento virtuální pevný disk pro definici hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="797df-145">hello user needs toocapture hello instance 0 VM which has hello required data, and then use that vhd for hello image definition.</span></span>

## <a name="removing-a-data-disk-from-a-scale-set"></a><span data-ttu-id="797df-146">Odebrání datového disku ze škálovací sady</span><span class="sxs-lookup"><span data-stu-id="797df-146">Removing a data disk from a scale set</span></span>
<span data-ttu-id="797df-147">Datový disk můžete ze škálovací sady virtuálních počítačů odebrat pomocí příkazu Azure CLI _az vmss disk detach_.</span><span class="sxs-lookup"><span data-stu-id="797df-147">You can remove a data disk from a VM scale set using Azure CLI _az vmss disk detach_ command.</span></span> <span data-ttu-id="797df-148">Například hello následující příkaz odebere hello disk definované na logickou jednotku lun 2:</span><span class="sxs-lookup"><span data-stu-id="797df-148">For example hello following command removes hello disk defined at lun 2:</span></span>
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
<span data-ttu-id="797df-149">Podobně můžete také odebrání disku z škálování nastavit odebráním položku z hello _dataDisks_ vlastnost hello _storageProfile_ a použití změn hello.</span><span class="sxs-lookup"><span data-stu-id="797df-149">Similarly you can also remove a disk from a scale set by removing an entry from hello _dataDisks_ property in hello _storageProfile_ and applying hello change.</span></span> 

## <a name="additional-notes"></a><span data-ttu-id="797df-150">Další poznámky</span><span class="sxs-lookup"><span data-stu-id="797df-150">Additional notes</span></span>
<span data-ttu-id="797df-151">Podpora pro Azure spravované disky a škálování, nastavte připojené datových disků je k dispozici ve verzi rozhraní API [ _2016-04-30-preview_ ](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) nebo novější z hello Microsoft.Compute rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="797df-151">Support for Azure Managed disks and scale set attached data disks is available in API version [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) or later of hello Microsoft.Compute API.</span></span>

<span data-ttu-id="797df-152">V implementaci počáteční hello připojený disk podpory pro sady škálování nelze připojit nebo odebrat datové disky z jednotlivé virtuální počítače ve škálovací sadě.</span><span class="sxs-lookup"><span data-stu-id="797df-152">In hello initial implementation of attached disk support for scale sets, you cannot attach or detach data disks to/from individual VMs in a scale set.</span></span>

<span data-ttu-id="797df-153">Podpora připojených datových disků ve škálovacích sadách je na webu Azure Portal zpočátku omezená.</span><span class="sxs-lookup"><span data-stu-id="797df-153">Azure portal support for attached data disks in scale sets is initially limited.</span></span> <span data-ttu-id="797df-154">V závislosti na požadavcích, že které můžete použít šablony Azure CLI, prostředí PowerShell, sady SDK a REST API toomanage připojenými disky.</span><span class="sxs-lookup"><span data-stu-id="797df-154">Depending on your requirements you can use Azure templates, CLI, PowerShell, SDKs, and REST API toomanage attached disks.</span></span>


