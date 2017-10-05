---
title: "Virtuální počítač restartováním nebo změnou velikosti problémy | Microsoft Docs"
description: "Řešení problémů nasazení classic s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure"
services: virtual-machines-windows
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 06/13/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: 7fe0636366c60d4679cfc69bd96cd532695b080e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a><span data-ttu-id="9d6bd-103">Řešení problémů nasazení classic s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="9d6bd-103">Troubleshoot classic deployment issues with restarting or resizing an existing Windows Virtual Machine in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9d6bd-104">Classic</span><span class="sxs-lookup"><span data-stu-id="9d6bd-104">Classic</span></span>](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [<span data-ttu-id="9d6bd-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9d6bd-105">Resource Manager</span></span>](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

<span data-ttu-id="9d6bd-106">Při pokusu o spuštění virtuálního počítače pro zastavený Azure (VM), nebo přizpůsobit existující virtuální počítač Azure je běžnou chybou, které zaznamenáte chybu přidělení.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-106">When you try to start a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, the common error you encounter is an allocation failure.</span></span> <span data-ttu-id="9d6bd-107">Tato chyba nastává clusteru nebo oblast buď nemá k dispozici prostředky nebo nemůže podporovat požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-107">This error results when the cluster or region either does not have resources available or cannot support the requested VM size.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9d6bd-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9d6bd-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="9d6bd-109">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-109">This article covers using the classic deployment model.</span></span> <span data-ttu-id="9d6bd-110">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a><span data-ttu-id="9d6bd-111">Protokoly auditu shromáždit</span><span class="sxs-lookup"><span data-stu-id="9d6bd-111">Collect audit logs</span></span>
<span data-ttu-id="9d6bd-112">Pokud chcete spustit Poradce při potížích, shromážděte protokoly auditu k identifikaci chyby související s problém.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-112">To start troubleshooting, collect the audit logs to identify the error associated with the issue.</span></span>

<span data-ttu-id="9d6bd-113">Na portálu Azure klikněte na tlačítko **Procházet** > **virtuální počítače** > *virtuálního počítače Windows* > **nastavení** > **protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-113">In the Azure portal, click **Browse** > **Virtual machines** > *your Windows virtual machine* > **Settings** > **Audit logs**.</span></span>

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="9d6bd-114">Problém: Chyba při spouštění zastaveného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="9d6bd-114">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="9d6bd-115">Pokoušíte se spustit zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-115">You try to start a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="9d6bd-116">Příčina</span><span class="sxs-lookup"><span data-stu-id="9d6bd-116">Cause</span></span>
<span data-ttu-id="9d6bd-117">Požadavek na spuštění zastaveného virtuálního počítače musí být pokus v původním clusteru, který je hostitelem cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-117">The request to start the stopped VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="9d6bd-118">Cluster nemá volné místo dostupné ke splnění tohoto požadavku.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-118">However, the cluster does not have free space available to fulfill the request.</span></span>

### <a name="resolution"></a><span data-ttu-id="9d6bd-119">Řešení</span><span class="sxs-lookup"><span data-stu-id="9d6bd-119">Resolution</span></span>
* <span data-ttu-id="9d6bd-120">Vytvořte novou cloudovou službu a přidružte ji k buď oblasti nebo na základě oblast virtuální sítě, ale není skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-120">Create a new cloud service and associate it with either a region or a region-based virtual network, but not an affinity group.</span></span>
* <span data-ttu-id="9d6bd-121">Odstraňte zastaveného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-121">Delete the stopped VM.</span></span>
* <span data-ttu-id="9d6bd-122">Znovu vytvořte virtuální počítač v rámci nové cloudové služby s použitím disků.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-122">Recreate the VM in the new cloud service by using the disks.</span></span>
* <span data-ttu-id="9d6bd-123">Spusťte znovu vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-123">Start the re-created VM.</span></span>

<span data-ttu-id="9d6bd-124">Pokud dojde k chybě při pokusu o vytvoření novou cloudovou službu, opakujte akci později nebo změnit oblast pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-124">If you get an error when trying to create a new cloud service, either retry later or change the region for the cloud service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9d6bd-125">Novou cloudovou službu bude mít nový název a VIP, takže budete muset změnit tyto informace pro všechny závislosti, které používají tyto informace pro stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-125">The new cloud service will have a new name and VIP, so you will need to change that information for all the dependencies that use that information for the existing cloud service.</span></span>
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="9d6bd-126">Problém: Chyba při změně velikosti stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="9d6bd-126">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="9d6bd-127">Pokoušíte se změnit velikost existující virtuální počítač ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-127">You try to resize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="9d6bd-128">Příčina</span><span class="sxs-lookup"><span data-stu-id="9d6bd-128">Cause</span></span>
<span data-ttu-id="9d6bd-129">Žádost o změně velikosti virtuálního počítače musí být pokus v původním clusteru, který je hostitelem cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-129">The request to resize the VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="9d6bd-130">Cluster však nepodporuje požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-130">However, the cluster does not support the requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="9d6bd-131">Řešení</span><span class="sxs-lookup"><span data-stu-id="9d6bd-131">Resolution</span></span>
<span data-ttu-id="9d6bd-132">Snižte požadovaná velikost virtuálního počítače a opakujte žádost změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-132">Reduce the requested VM size, and retry the resize request.</span></span>

* <span data-ttu-id="9d6bd-133">Klikněte na tlačítko **procházet všechny** > **virtuálních počítačů (klasické)** > *virtuálního počítače* > **nastavení**  >  **Velikost**.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-133">Click **Browse all** > **Virtual machines (classic)** > *your virtual machine* > **Settings** > **Size**.</span></span> <span data-ttu-id="9d6bd-134">Podrobné pokyny najdete v tématu [změnit velikost virtuálního počítače](https://msdn.microsoft.com/library/dn168976.aspx).</span><span class="sxs-lookup"><span data-stu-id="9d6bd-134">For detailed steps, see [Resize the virtual machine](https://msdn.microsoft.com/library/dn168976.aspx).</span></span>

<span data-ttu-id="9d6bd-135">Pokud není možné snížit velikost virtuálního počítače, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9d6bd-135">If it is not possible to reduce the VM size, follow these steps:</span></span>

* <span data-ttu-id="9d6bd-136">Vytvořte novou cloudovou službu, zajištění není propojená do skupiny vztahů a nejsou spojené s virtuální síť, která je propojená do skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-136">Create a new cloud service, ensuring it is not linked to an affinity group and not associated with a virtual network that is linked to an affinity group.</span></span>
* <span data-ttu-id="9d6bd-137">Vytvoření nové, větší velikost virtuálního počítače v ní.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-137">Create a new, larger-sized VM in it.</span></span>

<span data-ttu-id="9d6bd-138">Můžete sloučit všechny virtuální počítače v rámci stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-138">You can consolidate all your VMs in the same cloud service.</span></span> <span data-ttu-id="9d6bd-139">Pokud je přidružený k virtuální síti na základě oblast stávající cloudovou službu, můžete připojit nové cloudové služby na existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-139">If your existing cloud service is associated with a region-based virtual network, you can connect the new cloud service to the existing virtual network.</span></span>

<span data-ttu-id="9d6bd-140">Pokud existující cloudové služby není přidružený k virtuální síti založené na oblasti, budete muset odstranit virtuální počítače ve stávající cloudovou službu a vytvořte je znovu v rámci nové cloudové služby z jejich disky.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-140">If the existing cloud service is not associated with a region-based virtual network, then you have to delete the VMs in the existing cloud service, and recreate them in the new cloud service from their disks.</span></span> <span data-ttu-id="9d6bd-141">Je důležité si pamatovat, že novou cloudovou službu bude mít nový název a VIP, tak budete muset aktualizovat tyto pro všechny závislosti, které používají tyto informace pro stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="9d6bd-141">However, it is important to remember that the new cloud service will have a new name and VIP, so you will need to update these for all the dependencies that currently use this information for the existing cloud service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d6bd-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d6bd-142">Next steps</span></span>
<span data-ttu-id="9d6bd-143">Pokud dojde k potížím při vytvoření virtuálního počítače s Windows v Azure, najdete v části [řešení potíží s nasazením s vytvoření virtuálního počítače s Windows v Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9d6bd-143">If you encounter issues when you create a Windows VM in Azure, see [Troubleshoot deployment issues with creating a Windows virtual machine in Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

