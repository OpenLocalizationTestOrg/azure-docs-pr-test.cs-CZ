---
title: "aaaVM restartováním nebo změnou velikosti problémy | Microsoft Docs"
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
ms.openlocfilehash: 3d00ba17d9558941a37a29034604cb15e0803e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a><span data-ttu-id="b248b-103">Řešení problémů nasazení classic s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="b248b-103">Troubleshoot classic deployment issues with restarting or resizing an existing Windows Virtual Machine in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b248b-104">Classic</span><span class="sxs-lookup"><span data-stu-id="b248b-104">Classic</span></span>](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [<span data-ttu-id="b248b-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b248b-105">Resource Manager</span></span>](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

<span data-ttu-id="b248b-106">Zkuste toostart ukončeno virtuálního počítače (virtuální počítač Azure) nebo přizpůsobit existující virtuální počítač Azure, hello běžnou chybou, které zaznamenáte při došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="b248b-106">When you try toostart a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, hello common error you encounter is an allocation failure.</span></span> <span data-ttu-id="b248b-107">Tato chyba nastává hello clusteru nebo oblast buď nemá k dispozici prostředky nebo nelze podporu hello požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b248b-107">This error results when hello cluster or region either does not have resources available or cannot support hello requested VM size.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b248b-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b248b-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="b248b-109">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="b248b-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="b248b-110">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="b248b-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a><span data-ttu-id="b248b-111">Protokoly auditu shromáždit</span><span class="sxs-lookup"><span data-stu-id="b248b-111">Collect audit logs</span></span>
<span data-ttu-id="b248b-112">řešení potíží, toostart auditu shromáždit hello zaznamená tooidentify hello chyby související s problémem hello.</span><span class="sxs-lookup"><span data-stu-id="b248b-112">toostart troubleshooting, collect hello audit logs tooidentify hello error associated with hello issue.</span></span>

<span data-ttu-id="b248b-113">V hello portálu Azure, klikněte na **Procházet** > **virtuální počítače** > *virtuálního počítače Windows*  >   **Nastavení** > **protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="b248b-113">In hello Azure portal, click **Browse** > **Virtual machines** > *your Windows virtual machine* > **Settings** > **Audit logs**.</span></span>

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="b248b-114">Problém: Chyba při spouštění zastaveného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b248b-114">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="b248b-115">Zkuste toostart zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="b248b-115">You try toostart a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="b248b-116">Příčina</span><span class="sxs-lookup"><span data-stu-id="b248b-116">Cause</span></span>
<span data-ttu-id="b248b-117">žádost Hello toostart hello zastavena virtuálních počítačů má toobe na hello původní cluster, který je hostitelem hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b248b-117">hello request toostart hello stopped VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="b248b-118">Hello cluster však nebude mít volné místo k dispozici toofulfill hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="b248b-118">However, hello cluster does not have free space available toofulfill hello request.</span></span>

### <a name="resolution"></a><span data-ttu-id="b248b-119">Řešení</span><span class="sxs-lookup"><span data-stu-id="b248b-119">Resolution</span></span>
* <span data-ttu-id="b248b-120">Vytvořte novou cloudovou službu a přidružte ji k buď oblasti nebo na základě oblast virtuální sítě, ale není skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="b248b-120">Create a new cloud service and associate it with either a region or a region-based virtual network, but not an affinity group.</span></span>
* <span data-ttu-id="b248b-121">Odstranit hello zastavit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b248b-121">Delete hello stopped VM.</span></span>
* <span data-ttu-id="b248b-122">Znovu vytvořte hello virtuálních počítačů v hello novou cloudovou službu pomocí hello disky.</span><span class="sxs-lookup"><span data-stu-id="b248b-122">Recreate hello VM in hello new cloud service by using hello disks.</span></span>
* <span data-ttu-id="b248b-123">Spustit hello znovu vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b248b-123">Start hello re-created VM.</span></span>

<span data-ttu-id="b248b-124">Pokud dojde k chybě při pokusu o toocreate novou cloudovou službu, opakujte akci později nebo změnit hello oblast hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b248b-124">If you get an error when trying toocreate a new cloud service, either retry later or change hello region for hello cloud service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b248b-125">Hello nová Cloudová služba bude mít nový název a VIP, takže bude potřeba toochange tyto informace pro všechny závislosti hello, které používají tyto informace pro hello stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="b248b-125">hello new cloud service will have a new name and VIP, so you will need toochange that information for all hello dependencies that use that information for hello existing cloud service.</span></span>
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="b248b-126">Problém: Chyba při změně velikosti stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b248b-126">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="b248b-127">Zkuste tooresize existující virtuální počítač ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="b248b-127">You try tooresize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="b248b-128">Příčina</span><span class="sxs-lookup"><span data-stu-id="b248b-128">Cause</span></span>
<span data-ttu-id="b248b-129">Hello tooresize hello virtuálních počítačů má toobe pokusem o zpracování požadavku v původním clusteru hello dané hostitelů hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b248b-129">hello request tooresize hello VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="b248b-130">Ale hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b248b-130">However, hello cluster does not support hello requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="b248b-131">Řešení</span><span class="sxs-lookup"><span data-stu-id="b248b-131">Resolution</span></span>
<span data-ttu-id="b248b-132">Snižte hello požadovaná velikost virtuálního počítače a opakujte hello změnit velikost žádosti.</span><span class="sxs-lookup"><span data-stu-id="b248b-132">Reduce hello requested VM size, and retry hello resize request.</span></span>

* <span data-ttu-id="b248b-133">Klikněte na tlačítko **procházet všechny** > **virtuálních počítačů (klasické)** > *virtuálního počítače* > **nastavení**  >  **Velikost**.</span><span class="sxs-lookup"><span data-stu-id="b248b-133">Click **Browse all** > **Virtual machines (classic)** > *your virtual machine* > **Settings** > **Size**.</span></span> <span data-ttu-id="b248b-134">Podrobné pokyny najdete v tématu [změnit velikost virtuálního počítače hello](https://msdn.microsoft.com/library/dn168976.aspx).</span><span class="sxs-lookup"><span data-stu-id="b248b-134">For detailed steps, see [Resize hello virtual machine](https://msdn.microsoft.com/library/dn168976.aspx).</span></span>

<span data-ttu-id="b248b-135">Pokud není možné tooreduce hello velikost virtuálního počítače, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b248b-135">If it is not possible tooreduce hello VM size, follow these steps:</span></span>

* <span data-ttu-id="b248b-136">Vytvořte novou cloudovou službu, zajistíte, že není propojená skupiny vztahů tooan a není přidružen k virtuální síti, která je propojená tooan skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="b248b-136">Create a new cloud service, ensuring it is not linked tooan affinity group and not associated with a virtual network that is linked tooan affinity group.</span></span>
* <span data-ttu-id="b248b-137">Vytvoření nové, větší velikost virtuálního počítače v ní.</span><span class="sxs-lookup"><span data-stu-id="b248b-137">Create a new, larger-sized VM in it.</span></span>

<span data-ttu-id="b248b-138">Můžete sloučit všechny virtuální počítače v hello stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b248b-138">You can consolidate all your VMs in hello same cloud service.</span></span> <span data-ttu-id="b248b-139">Pokud je přidružený k virtuální síti na základě oblast stávající cloudovou službu, můžete se připojit hello nové cloudové služby toohello existující virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b248b-139">If your existing cloud service is associated with a region-based virtual network, you can connect hello new cloud service toohello existing virtual network.</span></span>

<span data-ttu-id="b248b-140">Pokud není přidružený k virtuální síti na základě oblast hello stávající cloudovou službu, pak máte toodelete hello virtuálních počítačů v hello stávající cloudovou službu a je v hello novou cloudovou službu z jejich disky znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b248b-140">If hello existing cloud service is not associated with a region-based virtual network, then you have toodelete hello VMs in hello existing cloud service, and recreate them in hello new cloud service from their disks.</span></span> <span data-ttu-id="b248b-141">Je však důležité tooremember, hello nová Cloudová služba bude mít nový název a VIP, takže bude potřeba tooupdate tyto pro všechny hello závislosti, které používají tyto informace pro hello stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="b248b-141">However, it is important tooremember that hello new cloud service will have a new name and VIP, so you will need tooupdate these for all hello dependencies that currently use this information for hello existing cloud service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b248b-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b248b-142">Next steps</span></span>
<span data-ttu-id="b248b-143">Pokud dojde k potížím při vytvoření virtuálního počítače s Windows v Azure, najdete v části [řešení potíží s nasazením s vytvoření virtuálního počítače s Windows v Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b248b-143">If you encounter issues when you create a Windows VM in Azure, see [Troubleshoot deployment issues with creating a Windows virtual machine in Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

