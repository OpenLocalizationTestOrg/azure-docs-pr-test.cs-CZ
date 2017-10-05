---
title: "Změnit velikost virtuální počítač s Windows v modelu nasazení classic - Azure | Microsoft Docs"
description: "Změňte velikost virtuálního počítače s Windows vytvořené v modelu nasazení classic, pomocí Azure Powershell."
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
ms.openlocfilehash: 4277bc8394c7ba140291e9dc776162e87deab96b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a><span data-ttu-id="272ae-103">Změnit velikost virtuálního počítače Windows vytvořené v modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="272ae-103">Resize a Windows VM created in the classic deployment model</span></span>
<span data-ttu-id="272ae-104">Tento článek ukazuje, jak změnit velikost virtuálního počítače Windows, vytvořené v modelu nasazení classic pomocí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="272ae-104">This article shows you how to resize a Windows VM, created in the classic deployment model using Azure Powershell.</span></span>

<span data-ttu-id="272ae-105">Při výběru možnosti měnit velikost virtuálního počítače, existují dvě koncepty, které řídí rozsah velikostí, které jsou k dispozici ke změně velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="272ae-105">When considering the ability to resize a VM, there are two concepts which control the range of sizes available to resize the virtual machine.</span></span> <span data-ttu-id="272ae-106">První koncept je oblast, ve kterém je nasazený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="272ae-106">The first concept is the region in which your VM is deployed.</span></span> <span data-ttu-id="272ae-107">Seznam velikosti virtuálních počítačů v oblasti je na kartě služeb oblasti Azure webové stránky.</span><span class="sxs-lookup"><span data-stu-id="272ae-107">The list of VM sizes available in region is under the Services tab of the Azure Regions web page.</span></span> <span data-ttu-id="272ae-108">Druhý koncept je fyzický hardware aktuálně hostuje virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="272ae-108">The second concept is the physical hardware currently hosting your VM.</span></span> <span data-ttu-id="272ae-109">Fyzické servery, které hostují virtuální počítače jsou seskupeny dohromady v clusterech běžné fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="272ae-109">The physical servers hosting VMs are grouped together in clusters of common physical hardware.</span></span> <span data-ttu-id="272ae-110">Pokud požadovaná velikost nového virtuálního počítače je podporováno hardwaru cluster aktuálně hostuje virtuální počítač, se liší v závislosti na metodě změna velikosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="272ae-110">The method of changing a VM size differs depending on if the desired new VM size is supported by the hardware cluster currently hosting the VM.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="272ae-111">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="272ae-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="272ae-112">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="272ae-112">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="272ae-113">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="272ae-113">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="272ae-114">Můžete také [změnit velikost virtuálního počítače vytvořené v modelu nasazení Resource Manager](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="272ae-114">You can also [resize a VM created in the Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="add-your-account"></a><span data-ttu-id="272ae-115">Přidání účtu</span><span class="sxs-lookup"><span data-stu-id="272ae-115">Add your account</span></span>
<span data-ttu-id="272ae-116">Je nutné nakonfigurovat Azure PowerShell pro práci s klasické prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="272ae-116">You must configure Azure PowerShell to work with classic Azure resources.</span></span> <span data-ttu-id="272ae-117">Postupujte podle následujících kroků a konfigurace prostředí Azure PowerShell ke správě klasické prostředky.</span><span class="sxs-lookup"><span data-stu-id="272ae-117">Follow the steps below to configure Azure PowerShell to manage classic resources.</span></span>

1. <span data-ttu-id="272ae-118">Zadejte v příkazovém prostředí PowerShell, `Add-AzureAccount` a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="272ae-118">At the PowerShell prompt, type `Add-AzureAccount` and click **Enter**.</span></span> 
2. <span data-ttu-id="272ae-119">Zadejte e-mailová adresa spojená s předplatným Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="272ae-119">Type in the email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="272ae-120">Zadejte heslo k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="272ae-120">Type in the password for your account.</span></span> 
4. <span data-ttu-id="272ae-121">Klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="272ae-121">Click **Sign in**.</span></span> 

## <a name="resize-in-the-same-hardware-cluster"></a><span data-ttu-id="272ae-122">Změnit velikost ve stejném clusteru hardwaru</span><span class="sxs-lookup"><span data-stu-id="272ae-122">Resize in the same hardware cluster</span></span>
<span data-ttu-id="272ae-123">Ke změně velikosti virtuálního počítače na velikost, která je k dispozici v hardwaru clusteru, který je hostitelem virtuálního počítače, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="272ae-123">To resize a VM to a size available in the hardware cluster hosting the VM, perform the following steps.</span></span>

1. <span data-ttu-id="272ae-124">Spusťte následující příkaz prostředí PowerShell k zobrazení seznamu velikosti virtuálních počítačů v clusteru hardwaru hostování Cloudová služba, která obsahuje virtuální počítač k dispozici.</span><span class="sxs-lookup"><span data-stu-id="272ae-124">Run the following PowerShell command to list the VM sizes available in the hardware cluster hosting the cloud service which contains the VM.</span></span>
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. <span data-ttu-id="272ae-125">Spusťte následující příkazy ke změně velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="272ae-125">Run the following commands to resize the VM.</span></span>
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a><span data-ttu-id="272ae-126">Změnit velikost na novém clusteru hardwaru</span><span class="sxs-lookup"><span data-stu-id="272ae-126">Resize on a new hardware cluster</span></span>
<span data-ttu-id="272ae-127">Ke změně velikosti virtuálního počítače na velikost není k dispozici v clusteru hardwaru hostování virtuálních počítačů cloudu služby a všechny virtuální počítače v rámci cloudové služby je nutné znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="272ae-127">To resize a VM to a size not available in the hardware cluster hosting the VM, the cloud service and all VMs in the cloud service must be recreated.</span></span> <span data-ttu-id="272ae-128">Jednotlivých cloudových služeb je hostované na klastru jeden hardwaru, takže všechny virtuální počítače v rámci cloudové služby musí být velikost, která je podporována v clusteru s podporou hardwaru.</span><span class="sxs-lookup"><span data-stu-id="272ae-128">Each cloud service is hosted on a single hardware cluster so all VMs in the cloud service must be a size that is supported on a hardware cluster.</span></span> <span data-ttu-id="272ae-129">Následující kroky se popisují, jak změnit velikost virtuálního počítače odstranit a potom znovu vytvořit cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="272ae-129">The following steps will describe how to resize a VM by deleting and then recreating the cloud service.</span></span>

1. <span data-ttu-id="272ae-130">Spusťte následující příkaz prostředí PowerShell k zobrazení seznamu velikosti virtuálních počítačů k dispozici v oblasti.</span><span class="sxs-lookup"><span data-stu-id="272ae-130">Run the following PowerShell command to list the VM sizes available in the region.</span></span> 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. <span data-ttu-id="272ae-131">Poznamenejte si všechna nastavení konfigurace pro každý virtuální počítač v rámci cloudové služby, který obsahuje virtuální počítač na změnu velikosti.</span><span class="sxs-lookup"><span data-stu-id="272ae-131">Make note of all configuration settings for each VM in the cloud service which contains the VM to be resized.</span></span> 
3. <span data-ttu-id="272ae-132">Odstraňte všechny virtuální počítače v rámci cloudové služby, vyberte možnost zachovat disky pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="272ae-132">Delete all VMs in the cloud service selecting the option to retain the disks for each VM.</span></span>
4. <span data-ttu-id="272ae-133">Znovu vytvořte virtuální počítač na změnu velikosti pomocí požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="272ae-133">Recreate the VM to be resized using the desired VM size.</span></span>
5. <span data-ttu-id="272ae-134">Znovu vytvořte všech ostatních virtuálních počítačů, které byly v rámci cloudové služby pomocí velikost virtuálního počítače, který je k dispozici v hardwaru clusteru nyní hostitelem cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="272ae-134">Recreate all other VMs which were in the cloud service using a VM size available in the hardware cluster now hosting the cloud service.</span></span>

<span data-ttu-id="272ae-135">Ukázkový skript pro odstranit a znovu vytvořit cloudovou službu pomocí nové velikost virtuálního počítače lze nalézt [zde](https://github.com/Azure/azure-vm-scripts).</span><span class="sxs-lookup"><span data-stu-id="272ae-135">A sample script for deleting and recreating a cloud service using a new VM size can be found [here](https://github.com/Azure/azure-vm-scripts).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="272ae-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="272ae-136">Next steps</span></span>
* <span data-ttu-id="272ae-137">[Změnit velikost virtuálního počítače vytvořené v modelu nasazení Resource Manager](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="272ae-137">[Resize a VM created in the Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
