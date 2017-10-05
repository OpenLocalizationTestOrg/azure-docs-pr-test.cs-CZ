---
title: "Virtuální počítač restartováním nebo změnou velikosti problémy v Azure | Microsoft Docs"
description: "Řešení problémů nasazení Resource Manager s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 06/13/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 078c4666f047604b1732e828d27e7e26383aa616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a><span data-ttu-id="1608a-103">Řešení potíží s nasazením s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="1608a-103">Troubleshoot deployment issues with restarting or resizing an existing Windows VM in Azure</span></span>
<span data-ttu-id="1608a-104">Při pokusu o spuštění virtuálního počítače pro zastavený Azure (VM), nebo přizpůsobit existující virtuální počítač Azure je běžnou chybou, které zaznamenáte chybu přidělení.</span><span class="sxs-lookup"><span data-stu-id="1608a-104">When you try to start a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, the common error you encounter is an allocation failure.</span></span> <span data-ttu-id="1608a-105">Tato chyba nastává clusteru nebo oblast buď nemá k dispozici prostředky nebo nemůže podporovat požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1608a-105">This error results when the cluster or region either does not have resources available or cannot support the requested VM size.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a><span data-ttu-id="1608a-106">Shromážděte aktivity protokolů</span><span class="sxs-lookup"><span data-stu-id="1608a-106">Collect activity logs</span></span>
<span data-ttu-id="1608a-107">Pokud chcete spustit Poradce při potížích, shromážděte protokoly aktivity k identifikaci chyby související s problém.</span><span class="sxs-lookup"><span data-stu-id="1608a-107">To start troubleshooting, collect the activity logs to identify the error associated with the issue.</span></span> <span data-ttu-id="1608a-108">Následující odkazy obsahují podrobné informace o procesu:</span><span class="sxs-lookup"><span data-stu-id="1608a-108">The following links contain detailed information on the process:</span></span>

[<span data-ttu-id="1608a-109">Zobrazení operací nasazení</span><span class="sxs-lookup"><span data-stu-id="1608a-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="1608a-110">Zobrazit protokoly aktivity ke správě prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="1608a-110">View activity logs to manage Azure resources</span></span>](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="1608a-111">Problém: Chyba při spouštění zastaveného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1608a-111">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="1608a-112">Pokoušíte se spustit zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="1608a-112">You try to start a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="1608a-113">Příčina</span><span class="sxs-lookup"><span data-stu-id="1608a-113">Cause</span></span>
<span data-ttu-id="1608a-114">Požadavek na spuštění zastaveného virtuálního počítače musí být pokus v původním clusteru, který je hostitelem cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="1608a-114">The request to start the stopped VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="1608a-115">Cluster nemá volné místo dostupné ke splnění tohoto požadavku.</span><span class="sxs-lookup"><span data-stu-id="1608a-115">However, the cluster does not have free space available to fulfill the request.</span></span>

### <a name="resolution"></a><span data-ttu-id="1608a-116">Řešení</span><span class="sxs-lookup"><span data-stu-id="1608a-116">Resolution</span></span>
* <span data-ttu-id="1608a-117">Zastavte všechny virtuální počítače v sadě dostupnosti a znovu spusťte každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1608a-117">Stop all the VMs in the availability set, and then restart each VM.</span></span>
  
  1. <span data-ttu-id="1608a-118">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="1608a-118">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="1608a-119">Po zastavení všech virtuálních počítačích, vyberte jednotlivé zastaven virtuálních počítačů a klikněte na příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="1608a-119">After all the VMs stop, select each of the stopped VMs and click Start.</span></span>
* <span data-ttu-id="1608a-120">Opakujte žádost restartovat později.</span><span class="sxs-lookup"><span data-stu-id="1608a-120">Retry the restart request at a later time.</span></span>

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="1608a-121">Problém: Chyba při změně velikosti stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1608a-121">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="1608a-122">Pokoušíte se změnit velikost existující virtuální počítač ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="1608a-122">You try to resize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="1608a-123">Příčina</span><span class="sxs-lookup"><span data-stu-id="1608a-123">Cause</span></span>
<span data-ttu-id="1608a-124">Žádost o změně velikosti virtuálního počítače musí být pokus v původním clusteru, který je hostitelem cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="1608a-124">The request to resize the VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="1608a-125">Cluster však nepodporuje požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1608a-125">However, the cluster does not support the requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="1608a-126">Řešení</span><span class="sxs-lookup"><span data-stu-id="1608a-126">Resolution</span></span>
* <span data-ttu-id="1608a-127">Opakujte tuto žádost pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1608a-127">Retry the request using a smaller VM size.</span></span>
* <span data-ttu-id="1608a-128">Pokud velikost požadovaný virtuální počítač nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="1608a-128">If the size of the requested VM cannot be changed：</span></span>
  
  1. <span data-ttu-id="1608a-129">Zastavte všechny virtuální počítače v sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="1608a-129">Stop all the VMs in the availability set.</span></span>
     
     * <span data-ttu-id="1608a-130">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="1608a-130">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="1608a-131">Po zastavení všech virtuálních počítačů, změňte velikost požadovaný virtuální počítač na větší velikost.</span><span class="sxs-lookup"><span data-stu-id="1608a-131">After all the VMs stop, resize the desired VM to a larger size.</span></span>
  3. <span data-ttu-id="1608a-132">Vyberte změněnou velikostí virtuálního počítače a klikněte na **spustit**, a následné spuštění všech virtuálních počítačích zastaven.</span><span class="sxs-lookup"><span data-stu-id="1608a-132">Select the resized VM and click **Start**, and then start each of the stopped VMs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1608a-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1608a-133">Next steps</span></span>
<span data-ttu-id="1608a-134">Pokud dojde k potížím při vytvoření nového virtuálního počítače s Windows v Azure, najdete v části [řešení potíží s nasazením s vytvoření nového virtuálního počítače Windows v Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1608a-134">If you encounter issues when you create a new Windows VM in Azure, see [Troubleshoot deployment issues with creating a new Windows virtual machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

