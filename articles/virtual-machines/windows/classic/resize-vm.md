---
title: "virtuální počítač s Windows v modelu nasazení classic hello - Azure aaaResize | Microsoft Docs"
description: "Změňte velikost virtuálního počítače s Windows vytvořené v modelu nasazení classic hello, pomocí Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e3038215-001c-406e-904d-e0f4e326a4c7
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 39fab14431e2351c9515b0611e850eccfec7a798
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-windows-vm-created-in-hello-classic-deployment-model"></a><span data-ttu-id="45a87-103">Změnit velikost virtuálního počítače Windows vytvořené v modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="45a87-103">Resize a Windows VM created in hello classic deployment model</span></span>
<span data-ttu-id="45a87-104">Tento článek ukazuje, jak tooresize virtuální počítač s Windows, vytvořené v modelu nasazení classic hello pomocí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="45a87-104">This article shows you how tooresize a Windows VM, created in hello classic deployment model using Azure Powershell.</span></span>

<span data-ttu-id="45a87-105">Při určování hello možnost tooresize virtuální počítač, existují dvě koncepty, které řídí hello rozsahu velikosti dostupné tooresize hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="45a87-105">When considering hello ability tooresize a VM, there are two concepts which control hello range of sizes available tooresize hello virtual machine.</span></span> <span data-ttu-id="45a87-106">První koncept Hello je hello oblast, ve kterém je nasazený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="45a87-106">hello first concept is hello region in which your VM is deployed.</span></span> <span data-ttu-id="45a87-107">Hello seznam velikosti virtuálních počítačů v oblasti je kartě hello služby hello oblasti Azure webové stránky.</span><span class="sxs-lookup"><span data-stu-id="45a87-107">hello list of VM sizes available in region is under hello Services tab of hello Azure Regions web page.</span></span> <span data-ttu-id="45a87-108">druhý koncept Hello je fyzický hardware hello aktuálně hostuje virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="45a87-108">hello second concept is hello physical hardware currently hosting your VM.</span></span> <span data-ttu-id="45a87-109">Hello fyzických serverů hostuje virtuální počítače jsou seskupeny dohromady v clusterech běžné fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="45a87-109">hello physical servers hosting VMs are grouped together in clusters of common physical hardware.</span></span> <span data-ttu-id="45a87-110">Metoda Hello změny velikost virtuálního počítače se liší v závislosti na tom, pokud hello požadovanou velikost nového virtuálního počítače je podporováno hello hardwaru cluster aktuálně hostuje hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="45a87-110">hello method of changing a VM size differs depending on if hello desired new VM size is supported by hello hardware cluster currently hosting hello VM.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="45a87-111">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="45a87-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="45a87-112">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="45a87-112">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="45a87-113">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="45a87-113">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="45a87-114">Můžete také [změnit velikost virtuálního počítače vytvořené v modelu nasazení Resource Manager hello](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45a87-114">You can also [resize a VM created in hello Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="add-your-account"></a><span data-ttu-id="45a87-115">Přidání účtu</span><span class="sxs-lookup"><span data-stu-id="45a87-115">Add your account</span></span>
<span data-ttu-id="45a87-116">Musíte nakonfigurovat prostředí Azure PowerShell toowork s klasické prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="45a87-116">You must configure Azure PowerShell toowork with classic Azure resources.</span></span> <span data-ttu-id="45a87-117">Postupujte podle kroků hello tooconfigure prostředí Azure PowerShell toomanage klasické prostředky.</span><span class="sxs-lookup"><span data-stu-id="45a87-117">Follow hello steps below tooconfigure Azure PowerShell toomanage classic resources.</span></span>

1. <span data-ttu-id="45a87-118">Zadejte v příkazovém prostředí PowerShell hello `Add-AzureAccount` a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="45a87-118">At hello PowerShell prompt, type `Add-AzureAccount` and click **Enter**.</span></span> 
2. <span data-ttu-id="45a87-119">Zadejte e-mailovou adresu hello spojené s předplatným Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="45a87-119">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="45a87-120">Zadejte heslo hello k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="45a87-120">Type in hello password for your account.</span></span> 
4. <span data-ttu-id="45a87-121">Klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="45a87-121">Click **Sign in**.</span></span> 

## <a name="resize-in-hello-same-hardware-cluster"></a><span data-ttu-id="45a87-122">Změna velikosti v hello stejný hardware clusteru</span><span class="sxs-lookup"><span data-stu-id="45a87-122">Resize in hello same hardware cluster</span></span>
<span data-ttu-id="45a87-123">tooresize velikost tooa virtuálního počítače, která je k dispozici v clusteru hardwaru hello hostování hello virtuálních počítačů, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="45a87-123">tooresize a VM tooa size available in hello hardware cluster hosting hello VM, perform hello following steps.</span></span>

1. <span data-ttu-id="45a87-124">Spusťte následující příkaz prostředí PowerShell, že k dispozici v clusteru hardwaru hello hostování hello Cloudová služba, která obsahuje velikosti virtuálních počítačů hello toolist hello virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="45a87-124">Run hello following PowerShell command toolist hello VM sizes available in hello hardware cluster hosting hello cloud service which contains hello VM.</span></span>
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. <span data-ttu-id="45a87-125">Spusťte následující příkazy tooresize hello virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="45a87-125">Run hello following commands tooresize hello VM.</span></span>
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a><span data-ttu-id="45a87-126">Změnit velikost na novém clusteru hardwaru</span><span class="sxs-lookup"><span data-stu-id="45a87-126">Resize on a new hardware cluster</span></span>
<span data-ttu-id="45a87-127">hello tooresize tooa velikost virtuálního počítače není k dispozici v hello hardwaru cluster hostitelem virtuálních počítačů, hello cloudové služby a všechny virtuální počítače v hello cloudové služby je nutné znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="45a87-127">tooresize a VM tooa size not available in hello hardware cluster hosting hello VM, hello cloud service and all VMs in hello cloud service must be recreated.</span></span> <span data-ttu-id="45a87-128">Jednotlivých cloudových služeb je hostované na klastru jeden hardwaru, takže všechny virtuální počítače v hello cloudové služby musí být velikost, která je podporována v clusteru s podporou hardwaru.</span><span class="sxs-lookup"><span data-stu-id="45a87-128">Each cloud service is hosted on a single hardware cluster so all VMs in hello cloud service must be a size that is supported on a hardware cluster.</span></span> <span data-ttu-id="45a87-129">Hello popisují tyto kroky budou jak hello tooresize odstranit a potom znovu vytvořit virtuální počítač cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="45a87-129">hello following steps will describe how tooresize a VM by deleting and then recreating hello cloud service.</span></span>

1. <span data-ttu-id="45a87-130">Spusťte následující prostředí PowerShell příkaz toolist hello velikosti virtuálních počítačů k dispozici v oblasti hello hello.</span><span class="sxs-lookup"><span data-stu-id="45a87-130">Run hello following PowerShell command toolist hello VM sizes available in hello region.</span></span> 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. <span data-ttu-id="45a87-131">Poznamenejte si všechna nastavení konfigurace pro každý virtuální počítač v hello cloudové služby, který obsahuje toobe hello virtuálních počítačů po změně velikosti.</span><span class="sxs-lookup"><span data-stu-id="45a87-131">Make note of all configuration settings for each VM in hello cloud service which contains hello VM toobe resized.</span></span> 
3. <span data-ttu-id="45a87-132">Odstraňte všechny virtuální počítače v cloudové službě hello výběr hello možnost tooretain hello disky pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="45a87-132">Delete all VMs in hello cloud service selecting hello option tooretain hello disks for each VM.</span></span>
4. <span data-ttu-id="45a87-133">Znovu vytvořte toobe hello virtuálních počítačů po změně velikosti pomocí hello potřeby velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="45a87-133">Recreate hello VM toobe resized using hello desired VM size.</span></span>
5. <span data-ttu-id="45a87-134">Znovu vytvořte všech ostatních virtuálních počítačů, které byly v rámci cloudové služby hello pomocí velikost virtuálního počítače, který je k dispozici v clusteru hardwaru hello nyní hostitelem hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="45a87-134">Recreate all other VMs which were in hello cloud service using a VM size available in hello hardware cluster now hosting hello cloud service.</span></span>

<span data-ttu-id="45a87-135">Ukázkový skript pro odstranit a znovu vytvořit cloudovou službu pomocí nové velikost virtuálního počítače lze nalézt [zde](https://github.com/Azure/azure-vm-scripts).</span><span class="sxs-lookup"><span data-stu-id="45a87-135">A sample script for deleting and recreating a cloud service using a new VM size can be found [here](https://github.com/Azure/azure-vm-scripts).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="45a87-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45a87-136">Next steps</span></span>
* <span data-ttu-id="45a87-137">[Změnit velikost virtuálního počítače vytvořené v modelu nasazení Resource Manager hello](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45a87-137">[Resize a VM created in hello Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

