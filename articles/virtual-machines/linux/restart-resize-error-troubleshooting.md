---
title: "aaaVM restartováním nebo změnou velikosti problémy v Azure | Microsoft Docs"
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
ms.openlocfilehash: d9ff76b64b6646b04565eb93110759c4ebc858e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-linux-vm-in-azure"></a><span data-ttu-id="fe9e0-103">Řešení potíží s nasazením s restartováním nebo změnou velikosti existující virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="fe9e0-103">Troubleshoot deployment issues with restarting or resizing an existing Linux VM in Azure</span></span>
<span data-ttu-id="fe9e0-104">Zkuste toostart ukončeno virtuálního počítače (virtuální počítač Azure) nebo přizpůsobit existující virtuální počítač Azure, hello běžnou chybou, které zaznamenáte při došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-104">When you try toostart a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, hello common error you encounter is an allocation failure.</span></span> <span data-ttu-id="fe9e0-105">Tato chyba nastává hello clusteru nebo oblast buď nemá k dispozici prostředky nebo nelze podporu hello požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-105">This error results when hello cluster or region either does not have resources available or cannot support hello requested VM size.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a><span data-ttu-id="fe9e0-106">Shromážděte aktivity protokolů</span><span class="sxs-lookup"><span data-stu-id="fe9e0-106">Collect activity logs</span></span>
<span data-ttu-id="fe9e0-107">řešení potíží, toostart shromažďování hello aktivity zaznamená tooidentify hello chyby související s problémem hello.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-107">toostart troubleshooting, collect hello activity logs tooidentify hello error associated with hello issue.</span></span> <span data-ttu-id="fe9e0-108">Hello následující odkazy obsahují podrobné informace o procesu hello:</span><span class="sxs-lookup"><span data-stu-id="fe9e0-108">hello following links contain detailed information on hello process:</span></span>

[<span data-ttu-id="fe9e0-109">Zobrazení operací nasazení</span><span class="sxs-lookup"><span data-stu-id="fe9e0-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="fe9e0-110">Zobrazit protokoly toomanage aktivity Azure prostředky</span><span class="sxs-lookup"><span data-stu-id="fe9e0-110">View activity logs toomanage Azure resources</span></span>](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="fe9e0-111">Problém: Chyba při spouštění zastaveného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fe9e0-111">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="fe9e0-112">Zkuste toostart zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-112">You try toostart a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="fe9e0-113">Příčina</span><span class="sxs-lookup"><span data-stu-id="fe9e0-113">Cause</span></span>
<span data-ttu-id="fe9e0-114">žádost Hello toostart hello zastavena virtuálních počítačů má toobe na hello původní cluster, který je hostitelem hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-114">hello request toostart hello stopped VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="fe9e0-115">Hello cluster však nebude mít volné místo k dispozici toofulfill hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-115">However, hello cluster does not have free space available toofulfill hello request.</span></span>

### <a name="resolution"></a><span data-ttu-id="fe9e0-116">Řešení</span><span class="sxs-lookup"><span data-stu-id="fe9e0-116">Resolution</span></span>
* <span data-ttu-id="fe9e0-117">Zastavte všechny hello virtuálních počítačů v hello dostupnosti nastavení a pak restartujte každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-117">Stop all hello VMs in hello availability set, and then restart each VM.</span></span>
  
  1. <span data-ttu-id="fe9e0-118">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-118">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="fe9e0-119">Po všech hello zastavení virtuálních počítačů, vyberte jednotlivé virtuální počítače hello zastavena a klikněte na příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-119">After all hello VMs stop, select each of hello stopped VMs and click Start.</span></span>
* <span data-ttu-id="fe9e0-120">Opakujte žádost restartování hello později.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-120">Retry hello restart request at a later time.</span></span>

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="fe9e0-121">Problém: Chyba při změně velikosti stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fe9e0-121">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="fe9e0-122">Zkuste tooresize existující virtuální počítač ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-122">You try tooresize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="fe9e0-123">Příčina</span><span class="sxs-lookup"><span data-stu-id="fe9e0-123">Cause</span></span>
<span data-ttu-id="fe9e0-124">Hello tooresize hello virtuálních počítačů má toobe pokusem o zpracování požadavku v původním clusteru hello dané hostitelů hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-124">hello request tooresize hello VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="fe9e0-125">Ale hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-125">However, hello cluster does not support hello requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="fe9e0-126">Řešení</span><span class="sxs-lookup"><span data-stu-id="fe9e0-126">Resolution</span></span>
* <span data-ttu-id="fe9e0-127">Opakujte žádost hello pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-127">Retry hello request using a smaller VM size.</span></span>
* <span data-ttu-id="fe9e0-128">Pokud hello požadovaná velikost hello že virtuálních počítačů nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="fe9e0-128">If hello size of hello requested VM cannot be changed：</span></span>
  
  1. <span data-ttu-id="fe9e0-129">Zastavte všechny virtuální počítače hello v sadě dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-129">Stop all hello VMs in hello availability set.</span></span>
     
     * <span data-ttu-id="fe9e0-130">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-130">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="fe9e0-131">Po všech hello zastavení virtuálních počítačů, změna velikosti větší velikost tooa hello potřeby virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-131">After all hello VMs stop, resize hello desired VM tooa larger size.</span></span>
  3. <span data-ttu-id="fe9e0-132">Vyberte hello po změně velikosti virtuálních počítačů a klikněte na tlačítko **spustit**, a pak spusťte každý hello zastavena virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fe9e0-132">Select hello resized VM and click **Start**, and then start each of hello stopped VMs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe9e0-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe9e0-133">Next steps</span></span>
<span data-ttu-id="fe9e0-134">Pokud dojde k potížím při vytváření nového virtuálního počítače s Linuxem v Azure, najdete v části [řešení potíží s nasazením s vytvoření nového virtuálního počítače Linux v Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fe9e0-134">If you encounter issues when you create a new Linux VM in Azure, see [Troubleshoot deployment issues with creating a new Linux virtual machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

