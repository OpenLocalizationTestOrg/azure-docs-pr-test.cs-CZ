---
title: "Virtuální počítač restartováním nebo změnou velikosti problémy v Azure | Microsoft Docs"
description: "Řešení problémů nasazení Resource Manager s restartováním nebo změnou velikosti existující virtuální počítač Linux v Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 51f1610c-0290-464a-97d4-c2e485c7ae7f
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.workload: required
ms.date: 01/10/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e49322dbccf4ec1157bc7e3a109175869b53518d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-linux-vm-in-azure"></a><span data-ttu-id="07bd3-103">Řešení potíží s nasazením s restartováním nebo změnou velikosti existující virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="07bd3-103">Troubleshoot deployment issues with restarting or resizing an existing Linux VM in Azure</span></span>
<span data-ttu-id="07bd3-104">Při pokusu o spuštění virtuálního počítače pro zastavený Azure (VM), nebo přizpůsobit existující virtuální počítač Azure je běžnou chybou, které zaznamenáte chybu přidělení.</span><span class="sxs-lookup"><span data-stu-id="07bd3-104">When you try to start a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, the common error you encounter is an allocation failure.</span></span> <span data-ttu-id="07bd3-105">Tato chyba nastává clusteru nebo oblast buď nemá k dispozici prostředky nebo nemůže podporovat požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="07bd3-105">This error results when the cluster or region either does not have resources available or cannot support the requested VM size.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a><span data-ttu-id="07bd3-106">Shromážděte aktivity protokolů</span><span class="sxs-lookup"><span data-stu-id="07bd3-106">Collect activity logs</span></span>
<span data-ttu-id="07bd3-107">Pokud chcete spustit Poradce při potížích, shromážděte protokoly aktivity k identifikaci chyby související s problém.</span><span class="sxs-lookup"><span data-stu-id="07bd3-107">To start troubleshooting, collect the activity logs to identify the error associated with the issue.</span></span> <span data-ttu-id="07bd3-108">Následující odkazy obsahují podrobné informace o procesu:</span><span class="sxs-lookup"><span data-stu-id="07bd3-108">The following links contain detailed information on the process:</span></span>

[<span data-ttu-id="07bd3-109">Zobrazení operací nasazení</span><span class="sxs-lookup"><span data-stu-id="07bd3-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="07bd3-110">Zobrazit protokoly aktivity ke správě prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="07bd3-110">View activity logs to manage Azure resources</span></span>](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="07bd3-111">Problém: Chyba při spouštění zastaveného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="07bd3-111">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="07bd3-112">Pokoušíte se spustit zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="07bd3-112">You try to start a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="07bd3-113">Příčina</span><span class="sxs-lookup"><span data-stu-id="07bd3-113">Cause</span></span>
<span data-ttu-id="07bd3-114">Požadavek na spuštění zastaveného virtuálního počítače musí být pokus v původním clusteru, který je hostitelem cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="07bd3-114">The request to start the stopped VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="07bd3-115">Cluster nemá volné místo dostupné ke splnění tohoto požadavku.</span><span class="sxs-lookup"><span data-stu-id="07bd3-115">However, the cluster does not have free space available to fulfill the request.</span></span>

### <a name="resolution"></a><span data-ttu-id="07bd3-116">Řešení</span><span class="sxs-lookup"><span data-stu-id="07bd3-116">Resolution</span></span>
* <span data-ttu-id="07bd3-117">Zastavte všechny virtuální počítače v sadě dostupnosti a znovu spusťte každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="07bd3-117">Stop all the VMs in the availability set, and then restart each VM.</span></span>
  
  1. <span data-ttu-id="07bd3-118">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="07bd3-118">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="07bd3-119">Po zastavení všech virtuálních počítačích, vyberte jednotlivé zastaven virtuálních počítačů a klikněte na příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="07bd3-119">After all the VMs stop, select each of the stopped VMs and click Start.</span></span>
* <span data-ttu-id="07bd3-120">Opakujte žádost restartovat později.</span><span class="sxs-lookup"><span data-stu-id="07bd3-120">Retry the restart request at a later time.</span></span>

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="07bd3-121">Problém: Chyba při změně velikosti stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="07bd3-121">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="07bd3-122">Pokoušíte se změnit velikost existující virtuální počítač ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="07bd3-122">You try to resize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="07bd3-123">Příčina</span><span class="sxs-lookup"><span data-stu-id="07bd3-123">Cause</span></span>
<span data-ttu-id="07bd3-124">Žádost o změně velikosti virtuálního počítače musí být pokus v původním clusteru, který je hostitelem cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="07bd3-124">The request to resize the VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="07bd3-125">Cluster však nepodporuje požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="07bd3-125">However, the cluster does not support the requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="07bd3-126">Řešení</span><span class="sxs-lookup"><span data-stu-id="07bd3-126">Resolution</span></span>
* <span data-ttu-id="07bd3-127">Opakujte tuto žádost pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="07bd3-127">Retry the request using a smaller VM size.</span></span>
* <span data-ttu-id="07bd3-128">Pokud velikost požadovaný virtuální počítač nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="07bd3-128">If the size of the requested VM cannot be changed：</span></span>
  
  1. <span data-ttu-id="07bd3-129">Zastavte všechny virtuální počítače v sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="07bd3-129">Stop all the VMs in the availability set.</span></span>
     
     * <span data-ttu-id="07bd3-130">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="07bd3-130">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="07bd3-131">Po zastavení všech virtuálních počítačů, změňte velikost požadovaný virtuální počítač na větší velikost.</span><span class="sxs-lookup"><span data-stu-id="07bd3-131">After all the VMs stop, resize the desired VM to a larger size.</span></span>
  3. <span data-ttu-id="07bd3-132">Vyberte změněnou velikostí virtuálního počítače a klikněte na **spustit**, a následné spuštění všech virtuálních počítačích zastaven.</span><span class="sxs-lookup"><span data-stu-id="07bd3-132">Select the resized VM and click **Start**, and then start each of the stopped VMs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07bd3-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07bd3-133">Next steps</span></span>
<span data-ttu-id="07bd3-134">Pokud dojde k potížím při vytváření nového virtuálního počítače s Linuxem v Azure, najdete v části [řešení potíží s nasazením s vytvoření nového virtuálního počítače Linux v Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="07bd3-134">If you encounter issues when you create a new Linux VM in Azure, see [Troubleshoot deployment issues with creating a new Linux virtual machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

