---
title: "Řešení potíží s nasazení virtuálního počítače s Linuxem-RM | Microsoft Docs"
description: "Řešení problémů nasazení Resource Manager, když vytvoříte nový virtuální počítač Linux v Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: aea5db05843b0175b8ef8b713944e12262e33010
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a><span data-ttu-id="22995-103">Řešení problémů nasazení Resource Manager pomocí vytvoření nového virtuálního počítače Linux v Azure</span><span class="sxs-lookup"><span data-stu-id="22995-103">Troubleshoot Resource Manager deployment issues with creating a new Linux virtual machine in Azure</span></span>
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a><span data-ttu-id="22995-104">Nejdůležitější problémy</span><span class="sxs-lookup"><span data-stu-id="22995-104">Top issues</span></span>
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

<span data-ttu-id="22995-105">Pro další problémy při nasazení virtuálního počítače a dotazy, najdete v části [potíží nasazení Linux virtuálního počítače v Azure](troubleshoot-deploy-vm.md).</span><span class="sxs-lookup"><span data-stu-id="22995-105">For other VM deployment issues and questions, see [Troubleshoot deploying Linux virtual machine issues in Azure](troubleshoot-deploy-vm.md).</span></span>
## <a name="collect-activity-logs"></a><span data-ttu-id="22995-106">Shromážděte aktivity protokolů</span><span class="sxs-lookup"><span data-stu-id="22995-106">Collect activity logs</span></span>
<span data-ttu-id="22995-107">Pokud chcete spustit Poradce při potížích, shromážděte protokoly aktivity k identifikaci chyby související s problém.</span><span class="sxs-lookup"><span data-stu-id="22995-107">To start troubleshooting, collect the activity logs to identify the error associated with the issue.</span></span> <span data-ttu-id="22995-108">Následující odkazy obsahují podrobné informace o na postupujte podle procesu.</span><span class="sxs-lookup"><span data-stu-id="22995-108">The following links contain detailed information on the process to follow.</span></span>

[<span data-ttu-id="22995-109">Zobrazení operací nasazení</span><span class="sxs-lookup"><span data-stu-id="22995-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="22995-110">Zobrazit protokoly aktivity ke správě prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="22995-110">View activity logs to manage Azure resources</span></span>](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

<span data-ttu-id="22995-111">**/ Y:** Pokud operačního systému Linux zobecněn a je nahrál nebo zachytit pomocí nastavení zobecněný, potom nebudou všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="22995-111">**Y:** If the OS is Linux generalized, and it is uploaded and/or captured with the generalized setting, then there won’t be any errors.</span></span> <span data-ttu-id="22995-112">Podobně pokud je operačním systémem Linux specializovaný a je nahrál nebo zachytit pomocí specializované nastavení a potom nebudou všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="22995-112">Similarly, if the OS is Linux specialized, and it is uploaded and/or captured with the specialized setting, then there won’t be any errors.</span></span>

<span data-ttu-id="22995-113">**Odešlete chyby:**</span><span class="sxs-lookup"><span data-stu-id="22995-113">**Upload Errors:**</span></span>

<span data-ttu-id="22995-114">**N<sup>1</sup>:** Pokud operačního systému Linux zobecněn, a nahraje jako specializovaný, zobrazí se zřizování vypršení časového limitu vzhledem k tomu, že virtuální počítač zasekl ve fázi zřizování.</span><span class="sxs-lookup"><span data-stu-id="22995-114">**N<sup>1</sup>:** If the OS is Linux generalized, and it is uploaded as specialized, you will get a provisioning timeout error because the VM is stuck at the provisioning stage.</span></span>

<span data-ttu-id="22995-115">**N<sup>2</sup>:** Pokud operačního systému Linux specializuje a nahraje jako zobecněn, zřizování selhání chyba se zobrazí, protože nový virtuální počítač je spuštěn s původní název počítače, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="22995-115">**N<sup>2</sup>:** If the OS is Linux specialized, and it is uploaded as generalized, you will get a provisioning failure error because the new VM is running with the original computer name, username and password.</span></span>

<span data-ttu-id="22995-116">**Řešení:**</span><span class="sxs-lookup"><span data-stu-id="22995-116">**Resolution:**</span></span>

<span data-ttu-id="22995-117">Obě tyto chyby vyřešíte nahrajte původní virtuální pevný disk, k dispozici místní, stejné nastavení jako pro operační systém (zobecněn/specializuje).</span><span class="sxs-lookup"><span data-stu-id="22995-117">To resolve both these errors, upload the original VHD, available on-prem, with the same setting as that for the OS (generalized/specialized).</span></span> <span data-ttu-id="22995-118">Pokud chcete nahrát jako zobecněn, nezapomeňte spustit - nejprve zrušit jejich zřízení.</span><span class="sxs-lookup"><span data-stu-id="22995-118">To upload as generalized, remember to run -deprovision first.</span></span>

<span data-ttu-id="22995-119">**Zaznamenat chyby:**</span><span class="sxs-lookup"><span data-stu-id="22995-119">**Capture Errors:**</span></span>

<span data-ttu-id="22995-120">**N<sup>3</sup>:** Pokud operačního systému Linux zobecněn, a se zaznamená jako specializovaný, zobrazí se zřizování vypršení časového limitu vzhledem k tomu, že původní virtuální počítač je nepoužitelný, jako je označena jako zobecněn.</span><span class="sxs-lookup"><span data-stu-id="22995-120">**N<sup>3</sup>:** If the OS is Linux generalized, and it is captured as specialized, you will get a provisioning timeout error because the original VM is not usable as it is marked as generalized.</span></span>

<span data-ttu-id="22995-121">**N<sup>4</sup>:** Pokud operačního systému Linux specializuje a z něj zachytí jako zobecněn, zřizování selhání chyba se zobrazí, protože nový virtuální počítač je spuštěn s původní název počítače, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="22995-121">**N<sup>4</sup>:** If the OS is Linux specialized, and it is captured as generalized, you will get a provisioning failure error because the new VM is running with the original computer name, username and password.</span></span> <span data-ttu-id="22995-122">Navíc původní virtuální počítač je nepoužitelný, protože je označeno jako specializované.</span><span class="sxs-lookup"><span data-stu-id="22995-122">Also, the original VM is not usable because it is marked as specialized.</span></span>

<span data-ttu-id="22995-123">**Řešení:**</span><span class="sxs-lookup"><span data-stu-id="22995-123">**Resolution:**</span></span>

<span data-ttu-id="22995-124">Chcete-li obě tyto chyby, odstraňte aktuální image z portálu, a [kopii z aktuální virtuální pevné disky](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) stejné nastavení jako pro operační systém (zobecněn/specializuje).</span><span class="sxs-lookup"><span data-stu-id="22995-124">To resolve both these errors, delete the current image from the portal, and [recapture it from the current VHDs](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) with the same setting as that for the OS (generalized/specialized).</span></span>

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a><span data-ttu-id="22995-125">Problém: Vlastní nebo Galerie / marketplace obrázku; došlo k chybě přidělení</span><span class="sxs-lookup"><span data-stu-id="22995-125">Issue: Custom/ gallery/ marketplace image; allocation failure</span></span>
<span data-ttu-id="22995-126">Tato chyba nastane v situacích, když novou žádost o virtuální počítač umístěn v clusteru, který nepodporuje požadovanou velikost virtuálního počítače, nebo nemá dostupné volné místo pro uložení žádosti.</span><span class="sxs-lookup"><span data-stu-id="22995-126">This error arises in situations when the new VM request is pinned to a cluster that either cannot support the VM size being requested, or does not have available free space to accommodate the request.</span></span>

<span data-ttu-id="22995-127">**Příčina 1:** clusteru nepodporuje požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22995-127">**Cause 1:** The cluster cannot support the requested VM size.</span></span>

<span data-ttu-id="22995-128">**Řešení 1:**</span><span class="sxs-lookup"><span data-stu-id="22995-128">**Resolution 1:**</span></span>

* <span data-ttu-id="22995-129">Opakujte tuto žádost pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22995-129">Retry the request using a smaller VM size.</span></span>
* <span data-ttu-id="22995-130">Pokud velikost požadovaný virtuální počítač nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="22995-130">If the size of the requested VM cannot be changed:</span></span>
  * <span data-ttu-id="22995-131">Zastavte všechny virtuální počítače v sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="22995-131">Stop all the VMs in the availability set.</span></span>
    <span data-ttu-id="22995-132">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="22995-132">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  * <span data-ttu-id="22995-133">Po zastavení všech virtuálních počítačů, vytvořte nový virtuální počítač v požadovaná velikost.</span><span class="sxs-lookup"><span data-stu-id="22995-133">After all the VMs stop, create the new VM in the desired size.</span></span>
  * <span data-ttu-id="22995-134">Nejprve spusťte nový virtuální počítač a potom vyberte jednotlivé zastaven virtuálních počítačů a klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="22995-134">Start the new VM first, and then select each of the stopped VMs and click **Start**.</span></span>

<span data-ttu-id="22995-135">**Příčina 2:** clusteru nemá uvolnění prostředků.</span><span class="sxs-lookup"><span data-stu-id="22995-135">**Cause 2:** The cluster does not have free resources.</span></span>

<span data-ttu-id="22995-136">**Řešení 2:**</span><span class="sxs-lookup"><span data-stu-id="22995-136">**Resolution 2:**</span></span>

* <span data-ttu-id="22995-137">Opakujte požadavek později.</span><span class="sxs-lookup"><span data-stu-id="22995-137">Retry the request at a later time.</span></span>
* <span data-ttu-id="22995-138">Pokud nový virtuální počítač může být součástí skupiny s jinou dostupnosti</span><span class="sxs-lookup"><span data-stu-id="22995-138">If the new VM can be part of a different availability set</span></span>
  * <span data-ttu-id="22995-139">Vytvoření nového virtuálního počítače v různých dostupnosti nastavit (ve stejné oblasti).</span><span class="sxs-lookup"><span data-stu-id="22995-139">Create a new VM in a different availability set (in the same region).</span></span>
  * <span data-ttu-id="22995-140">Přidáte nový virtuální počítač do stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="22995-140">Add the new VM to the same virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22995-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22995-141">Next steps</span></span>
<span data-ttu-id="22995-142">Pokud dojde k potížím při spuštění zastavený virtuální počítač s Linuxem nebo změnit jeho velikost existující virtuální počítač s Linuxem v Azure, najdete v části [problémy při nasazení Resource Manager řešení potíží s restartováním nebo změnou velikosti existující virtuální počítač Linux v Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="22995-142">If you encounter issues when you start a stopped Linux VM or resize an existing Linux VM in Azure, see [Troubleshoot Resource Manager deployment issues with restarting or resizing an existing Linux Virtual Machine in Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

