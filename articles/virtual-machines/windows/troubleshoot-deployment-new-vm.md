---
title: "aaaTroubleshoot nasazení virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Řešení problémů nasazení Resource Manager, když vytvoříte nový virtuální počítač Windows v Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c27d4f63b67db2d1c9f117f05a2eba9ef130ef37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a><span data-ttu-id="30987-103">Řešení problémů nasazení, při vytváření nového virtuálního počítače Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="30987-103">Troubleshoot deployment issues when creating a new Windows VM in Azure</span></span>
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a><span data-ttu-id="30987-104">Nejdůležitější problémy</span><span class="sxs-lookup"><span data-stu-id="30987-104">Top issues</span></span>
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

<span data-ttu-id="30987-105">Pro další problémy při nasazení virtuálního počítače a dotazy, najdete v části [potíží nasazení systému Windows virtuálního počítače v Azure](troubleshoot-deploy-vm.md).</span><span class="sxs-lookup"><span data-stu-id="30987-105">For other VM deployment issues and questions, see [Troubleshoot deploying Windows virtual machine issues in Azure](troubleshoot-deploy-vm.md).</span></span>

## <a name="collect-activity-logs"></a><span data-ttu-id="30987-106">Shromážděte aktivity protokolů</span><span class="sxs-lookup"><span data-stu-id="30987-106">Collect activity logs</span></span>
<span data-ttu-id="30987-107">řešení potíží, toostart shromažďování hello aktivity zaznamená tooidentify hello chyby související s problémem hello.</span><span class="sxs-lookup"><span data-stu-id="30987-107">toostart troubleshooting, collect hello activity logs tooidentify hello error associated with hello issue.</span></span> <span data-ttu-id="30987-108">Hello následující odkazy obsahují podrobné informace o procesu toofollow hello.</span><span class="sxs-lookup"><span data-stu-id="30987-108">hello following links contain detailed information on hello process toofollow.</span></span>

[<span data-ttu-id="30987-109">Zobrazení operací nasazení</span><span class="sxs-lookup"><span data-stu-id="30987-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="30987-110">Zobrazit protokoly toomanage aktivity Azure prostředky</span><span class="sxs-lookup"><span data-stu-id="30987-110">View activity logs toomanage Azure resources</span></span>](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

<span data-ttu-id="30987-111">**/ Y:** Pokud hello operační systém je Windows zobecněn a je nahrál nebo zachytit pomocí hello zobecněn nastavení, pak nebudou všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="30987-111">**Y:** If hello OS is Windows generalized, and it is uploaded and/or captured with hello generalized setting, then there won’t be any errors.</span></span> <span data-ttu-id="30987-112">Podobně pokud hello operačního systému se specializuje Windows a je nahrál nebo zachytit pomocí hello speciální nastavení, pak nebude existovat žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="30987-112">Similarly, if hello OS is Windows specialized, and it is uploaded and/or captured with hello specialized setting, then there won’t be any errors.</span></span>

<span data-ttu-id="30987-113">**Odešlete chyby:**</span><span class="sxs-lookup"><span data-stu-id="30987-113">**Upload Errors:**</span></span>

<span data-ttu-id="30987-114">**N<sup>1</sup>:** Pokud hello operačního systému Windows zobecněn, a nahraje jako specializovaný, budete mít zřizování chyba časového limitu s hello virtuálních počítačů zablokované ve úvodní obrazovka při prvním zapnutí.</span><span class="sxs-lookup"><span data-stu-id="30987-114">**N<sup>1</sup>:** If hello OS is Windows generalized, and it is uploaded as specialized, you will get a provisioning timeout error with hello VM stuck at hello OOBE screen.</span></span>

<span data-ttu-id="30987-115">**N<sup>2</sup>:** Pokud hello operačního systému Windows specializuje a nahraje jako zobecněn, zobrazí se zřizování chyba selhání s hello virtuálních počítačů zablokované ve úvodní obrazovka při prvním zapnutí, protože hello nový virtuální počítač je spuštěn s původní počítač hello název, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="30987-115">**N<sup>2</sup>:** If hello OS is Windows specialized, and it is uploaded as generalized, you will get a provisioning failure error with hello VM stuck at hello OOBE screen because hello new VM is running with hello original computer name, username and password.</span></span>

<span data-ttu-id="30987-116">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="30987-116">**Resolution**</span></span>

<span data-ttu-id="30987-117">použít obě tyto chyby tooresolve [přidat AzureRmVhd tooupload hello původní virtuální pevný disk](https://msdn.microsoft.com/library/mt603554.aspx), k dispozici místně, s hello stejné nastavení jako hello operačního systému (zobecněn/specializuje).</span><span class="sxs-lookup"><span data-stu-id="30987-117">tooresolve both these errors, use [Add-AzureRmVhd tooupload hello original VHD](https://msdn.microsoft.com/library/mt603554.aspx), available on-premises, with hello same setting as that for hello OS (generalized/specialized).</span></span> <span data-ttu-id="30987-118">tooupload jako zobecněn, nezapomeňte nejprve toorun nástroje sysprep.</span><span class="sxs-lookup"><span data-stu-id="30987-118">tooupload as generalized, remember toorun sysprep first.</span></span>

<span data-ttu-id="30987-119">**Zaznamenat chyby:**</span><span class="sxs-lookup"><span data-stu-id="30987-119">**Capture Errors:**</span></span>

<span data-ttu-id="30987-120">**N<sup>3</sup>:** Pokud hello operačního systému Windows zobecněn, a se zaznamená jako specializovaný, zobrazí se zřizování vypršení časového limitu protože hello původní virtuální počítač je nepoužitelný je označen jako zobecněn.</span><span class="sxs-lookup"><span data-stu-id="30987-120">**N<sup>3</sup>:** If hello OS is Windows generalized, and it is captured as specialized, you will get a provisioning timeout error because hello original VM is not usable as it is marked as generalized.</span></span>

<span data-ttu-id="30987-121">**N<sup>4</sup>:** Pokud hello operačního systému Windows specializuje a z něj zachytí jako zobecněn, zřizování selhání chyba se zobrazí, protože hello nový virtuální počítač je spuštěn s hello původní název počítače, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="30987-121">**N<sup>4</sup>:** If hello OS is Windows specialized, and it is captured as generalized, you will get a provisioning failure error because hello new VM is running with hello original computer name, username, and password.</span></span> <span data-ttu-id="30987-122">Navíc hello původní virtuální počítač je nepoužitelný, protože je označeno jako specializované.</span><span class="sxs-lookup"><span data-stu-id="30987-122">Also, hello original VM is not usable because it is marked as specialized.</span></span>

<span data-ttu-id="30987-123">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="30987-123">**Resolution**</span></span>

<span data-ttu-id="30987-124">tooresolve obě tyto chyby odstranit hello aktuální image z portálu hello a [kopii z hello aktuální virtuální pevné disky](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) s hello stejné nastavení jako hello operačního systému (zobecněn/specializuje).</span><span class="sxs-lookup"><span data-stu-id="30987-124">tooresolve both these errors, delete hello current image from hello portal, and [recapture it from hello current VHDs](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) with hello same setting as that for hello OS (generalized/specialized).</span></span>

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a><span data-ttu-id="30987-125">Problém: Vlastní nebo Galerie/marketplace obrázku; došlo k chybě přidělení</span><span class="sxs-lookup"><span data-stu-id="30987-125">Issue: Custom/gallery/marketplace image; allocation failure</span></span>
<span data-ttu-id="30987-126">Tato chyba nastane v situacích, když hello novou žádost o virtuální počítač je definovaného tooa cluster, který nepodporuje požadovanou velikost virtuálního počítače hello, nebo nemá požadavek hello tooaccommodate dostupné volné místo.</span><span class="sxs-lookup"><span data-stu-id="30987-126">This error arises in situations when hello new VM request is pinned tooa cluster that either cannot support hello VM size being requested, or does not have available free space tooaccommodate hello request.</span></span>

<span data-ttu-id="30987-127">**Příčina 1:** hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="30987-127">**Cause 1:** hello cluster cannot support hello requested VM size.</span></span>

<span data-ttu-id="30987-128">**Řešení 1:**</span><span class="sxs-lookup"><span data-stu-id="30987-128">**Resolution 1:**</span></span>

* <span data-ttu-id="30987-129">Opakujte žádost hello pomocí menší velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="30987-129">Retry hello request using a smaller VM size.</span></span>
* <span data-ttu-id="30987-130">Pokud hello požadovaná velikost hello že virtuálních počítačů nelze změnit:</span><span class="sxs-lookup"><span data-stu-id="30987-130">If hello size of hello requested VM cannot be changed:</span></span>
  * <span data-ttu-id="30987-131">Zastavte všechny virtuální počítače hello v sadě dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="30987-131">Stop all hello VMs in hello availability set.</span></span>
    <span data-ttu-id="30987-132">Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="30987-132">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  * <span data-ttu-id="30987-133">Po všech hello zastavit virtuální počítače, vytvořte hello nový virtuální počítač v hello potřeby velikost.</span><span class="sxs-lookup"><span data-stu-id="30987-133">After all hello VMs stop, create hello new VM in hello desired size.</span></span>
  * <span data-ttu-id="30987-134">Spusťte nejprve hello nový virtuální počítač a potom vyberete jednotlivé hello zastavena virtuální počítače a klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="30987-134">Start hello new VM first, and then select each of hello stopped VMs and click **Start**.</span></span>

<span data-ttu-id="30987-135">**Příčina 2:** hello cluster nebude mít uvolnění prostředků.</span><span class="sxs-lookup"><span data-stu-id="30987-135">**Cause 2:** hello cluster does not have free resources.</span></span>

<span data-ttu-id="30987-136">**Řešení 2:**</span><span class="sxs-lookup"><span data-stu-id="30987-136">**Resolution 2:**</span></span>

* <span data-ttu-id="30987-137">Opakujte žádost hello později.</span><span class="sxs-lookup"><span data-stu-id="30987-137">Retry hello request at a later time.</span></span>
* <span data-ttu-id="30987-138">Pokud můžete technologie hello nový virtuální počítač součástí jiné dostupnosti nastavit</span><span class="sxs-lookup"><span data-stu-id="30987-138">If hello new VM can be part of a different availability set</span></span>
  * <span data-ttu-id="30987-139">Vytvoření nového virtuálního počítače v sadě dostupnosti jinou (v hello stejné oblasti).</span><span class="sxs-lookup"><span data-stu-id="30987-139">Create a new VM in a different availability set (in hello same region).</span></span>
  * <span data-ttu-id="30987-140">Přidat nový virtuální počítač toohello hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="30987-140">Add hello new VM toohello same virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="30987-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30987-141">Next steps</span></span>
<span data-ttu-id="30987-142">Pokud dojde k potížím při spuštění zastaveného virtuálního počítače Windows nebo přizpůsobit existující virtuální počítač Windows v Azure, najdete v části [problémy při nasazení Resource Manager řešení potíží s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="30987-142">If you encounter issues when you start a stopped Windows VM or resize an existing Windows VM in Azure, see [Troubleshoot Resource Manager deployment issues with restarting or resizing an existing Windows Virtual Machine in Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

