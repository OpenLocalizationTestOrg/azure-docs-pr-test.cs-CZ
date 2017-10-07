---
title: "aaaVM restartováním nebo změnou velikosti problémy v Azure | Microsoft Docs"
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
ms.openlocfilehash: 2cf7c2d19bf5f79fab4ffc0eff9ccc1182d601c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a><span data-ttu-id="79882-103">Řešení potíží s nasazením s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="79882-103">Troubleshoot deployment issues with restarting or resizing an existing Windows VM in Azure</span></span>
<span data-ttu-id="79882-104">Zkuste toostart ukončeno virtuálního počítače (virtuální počítač Azure) nebo přizpůsobit existující virtuální počítač Azure, hello běžnou chybou, které zaznamenáte při došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="79882-104">When you try toostart a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, hello common error you encounter is an allocation failure.</span></span> <span data-ttu-id="79882-105">Tato chyba nastává hello clusteru nebo oblast buď nemá k dispozici prostředky nebo nelze podporu hello požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79882-105">This error results when hello cluster or region either does not have resources available or cannot support hello requested VM size.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a><span data-ttu-id="79882-106">Shromážděte aktivity protokolů</span><span class="sxs-lookup"><span data-stu-id="79882-106">Collect activity logs</span></span>
<span data-ttu-id="79882-107">řešení potíží, toostart shromažďování hello aktivity zaznamená tooidentify hello chyby související s problémem hello.</span><span class="sxs-lookup"><span data-stu-id="79882-107">toostart troubleshooting, collect hello activity logs tooidentify hello error associated with hello issue.</span></span> <span data-ttu-id="79882-108">Hello následující odkazy obsahují podrobné informace o procesu hello:</span><span class="sxs-lookup"><span data-stu-id="79882-108">hello following links contain detailed information on hello process:</span></span>

[<span data-ttu-id="79882-109">Zobrazení operací nasazení</span><span class="sxs-lookup"><span data-stu-id="79882-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="79882-110">Zobrazit protokoly toomanage aktivity Azure prostředky</span><span class="sxs-lookup"><span data-stu-id="79882-110">View activity logs toomanage Azure resources</span></span>](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="79882-111">Problém: Chyba při spouštění zastaveného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="79882-111">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="79882-112">Zkuste toostart zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="79882-112">You try toostart a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="79882-113">Příčina</span><span class="sxs-lookup"><span data-stu-id="79882-113">Cause</span></span>
<span data-ttu-id="79882-114">žádost Hello toostart hello zastavena virtuálních počítačů má toobe na hello původní cluster, který je hostitelem hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="79882-114">hello request toostart hello stopped VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="79882-115">Hello cluster však nebude mít volné místo k dispozici toofulfill hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="79882-115">However, hello cluster does not have free space available toofulfill hello request.</span></span>

### <a name="resolution"></a><span data-ttu-id="79882-116">Řešení</span><span class="sxs-lookup"><span data-stu-id="79882-116">Resolution</span></span>
* <span data-ttu-id="79882-117">Zastavte všechny hello virtuálních počítačů v hello dostupnosti nastavení a pak restartujte každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="79882-117">Stop all hello VMs in hello availability set, and then restart each VM.</span></span>
  
  1. <span data-ttu-id="79882-118">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="79882-118">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="79882-119">Po všech hello zastavení virtuálních počítačů, vyberte jednotlivé virtuální počítače hello zastavena a klikněte na příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="79882-119">After all hello VMs stop, select each of hello stopped VMs and click Start.</span></span>
* <span data-ttu-id="79882-120">Opakujte žádost restartování hello později.</span><span class="sxs-lookup"><span data-stu-id="79882-120">Retry hello restart request at a later time.</span></span>

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="79882-121">Problém: Chyba při změně velikosti stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="79882-121">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="79882-122">Zkuste tooresize existující virtuální počítač ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="79882-122">You try tooresize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="79882-123">Příčina</span><span class="sxs-lookup"><span data-stu-id="79882-123">Cause</span></span>
<span data-ttu-id="79882-124">Hello tooresize hello virtuálních počítačů má toobe pokusem o zpracování požadavku v původním clusteru hello dané hostitelů hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="79882-124">hello request tooresize hello VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="79882-125">Ale hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79882-125">However, hello cluster does not support hello requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="79882-126">Řešení</span><span class="sxs-lookup"><span data-stu-id="79882-126">Resolution</span></span>
* <span data-ttu-id="79882-127">Opakujte žádost hello pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79882-127">Retry hello request using a smaller VM size.</span></span>
* <span data-ttu-id="79882-128">Pokud hello požadovaná velikost hello že virtuálních počítačů nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="79882-128">If hello size of hello requested VM cannot be changed：</span></span>
  
  1. <span data-ttu-id="79882-129">Zastavte všechny virtuální počítače hello v sadě dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="79882-129">Stop all hello VMs in hello availability set.</span></span>
     
     * <span data-ttu-id="79882-130">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="79882-130">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="79882-131">Po všech hello zastavení virtuálních počítačů, změna velikosti větší velikost tooa hello potřeby virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79882-131">After all hello VMs stop, resize hello desired VM tooa larger size.</span></span>
  3. <span data-ttu-id="79882-132">Vyberte hello po změně velikosti virtuálních počítačů a klikněte na tlačítko **spustit**, a pak spusťte každý hello zastavena virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="79882-132">Select hello resized VM and click **Start**, and then start each of hello stopped VMs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79882-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79882-133">Next steps</span></span>
<span data-ttu-id="79882-134">Pokud dojde k potížím při vytvoření nového virtuálního počítače s Windows v Azure, najdete v části [řešení potíží s nasazením s vytvoření nového virtuálního počítače Windows v Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="79882-134">If you encounter issues when you create a new Windows VM in Azure, see [Troubleshoot deployment issues with creating a new Windows virtual machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

