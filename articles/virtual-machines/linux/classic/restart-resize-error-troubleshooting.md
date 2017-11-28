---
title: "aaaVM restartováním nebo změnou velikosti problémy | Microsoft Docs"
description: "Řešení problémů nasazení classic s restartováním nebo změnou velikosti existující virtuální počítač Linux v Azure"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 73f2672c-602e-4766-8948-2b180115d299
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: required
ms.date: 01/10/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: fb1dc88bb1b83043c434590118bc8810ad402872
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a><span data-ttu-id="e73df-103">Řešení problémů nasazení classic s restartováním nebo změnou velikosti existující virtuální počítač Linux v Azure</span><span class="sxs-lookup"><span data-stu-id="e73df-103">Troubleshoot classic deployment issues with restarting or resizing an existing Linux Virtual Machine in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e73df-104">Classic</span><span class="sxs-lookup"><span data-stu-id="e73df-104">Classic</span></span>](restart-resize-error-troubleshooting.md)
> * [<span data-ttu-id="e73df-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e73df-105">Resource Manager</span></span>](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<span data-ttu-id="e73df-106">Zkuste toostart ukončeno virtuálního počítače (virtuální počítač Azure) nebo přizpůsobit existující virtuální počítač Azure, hello běžnou chybou, které zaznamenáte při došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="e73df-106">When you try toostart a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, hello common error you encounter is an allocation failure.</span></span> <span data-ttu-id="e73df-107">Tato chyba nastává hello clusteru nebo oblast buď nemá k dispozici prostředky nebo nelze podporu hello požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e73df-107">This error results when hello cluster or region either does not have resources available or cannot support hello requested VM size.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="e73df-108">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e73df-108">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="e73df-109">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="e73df-109">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="e73df-110">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="e73df-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="e73df-111">Verze hello Resource Manager, najdete v části [zde](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e73df-111">For hello Resource Manager version, see [here](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a><span data-ttu-id="e73df-112">Protokoly auditu shromáždit</span><span class="sxs-lookup"><span data-stu-id="e73df-112">Collect audit logs</span></span>
<span data-ttu-id="e73df-113">řešení potíží, toostart auditu shromáždit hello zaznamená tooidentify hello chyby související s problémem hello.</span><span class="sxs-lookup"><span data-stu-id="e73df-113">toostart troubleshooting, collect hello audit logs tooidentify hello error associated with hello issue.</span></span>

<span data-ttu-id="e73df-114">V hello portálu Azure, klikněte na **Procházet** > **virtuální počítače** > *virtuální počítač Linux*  >   **Nastavení** > **protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="e73df-114">In hello Azure portal, click **Browse** > **Virtual machines** > *your Linux virtual machine* > **Settings** > **Audit logs**.</span></span>

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="e73df-115">Problém: Chyba při spouštění zastaveného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e73df-115">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="e73df-116">Zkuste toostart zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="e73df-116">You try toostart a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="e73df-117">Příčina</span><span class="sxs-lookup"><span data-stu-id="e73df-117">Cause</span></span>
<span data-ttu-id="e73df-118">žádost Hello toostart hello zastavena virtuálních počítačů má toobe na hello původní cluster, který je hostitelem hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e73df-118">hello request toostart hello stopped VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="e73df-119">Hello cluster však nebude mít volné místo k dispozici toofulfill hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="e73df-119">However, hello cluster does not have free space available toofulfill hello request.</span></span>

### <a name="resolution"></a><span data-ttu-id="e73df-120">Řešení</span><span class="sxs-lookup"><span data-stu-id="e73df-120">Resolution</span></span>
* <span data-ttu-id="e73df-121">Vytvořte novou cloudovou službu a přidružte ji k buď oblasti nebo na základě oblast virtuální sítě, ale není skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="e73df-121">Create a new cloud service and associate it with either a region or a region-based virtual network, but not an affinity group.</span></span>
* <span data-ttu-id="e73df-122">Odstranit hello zastavit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e73df-122">Delete hello stopped VM.</span></span>
* <span data-ttu-id="e73df-123">Znovu vytvořte hello virtuálních počítačů v hello novou cloudovou službu pomocí hello disky.</span><span class="sxs-lookup"><span data-stu-id="e73df-123">Recreate hello VM in hello new cloud service by using hello disks.</span></span>
* <span data-ttu-id="e73df-124">Spustit hello znovu vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e73df-124">Start hello re-created VM.</span></span>

<span data-ttu-id="e73df-125">Pokud dojde k chybě při pokusu o toocreate novou cloudovou službu, opakujte později nebo změnit hello oblast hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e73df-125">If you get an error when trying toocreate a new cloud service, either retry at a later time or change hello region for hello cloud service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e73df-126">Hello nová Cloudová služba bude mít nový název a VIP, takže bude potřeba toochange tyto informace pro všechny závislosti hello, které používají tyto informace pro hello stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="e73df-126">hello new cloud service will have a new name and VIP, so you will need toochange that information for all hello dependencies that use that information for hello existing cloud service.</span></span>
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="e73df-127">Problém: Chyba při změně velikosti stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e73df-127">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="e73df-128">Zkuste tooresize existující virtuální počítač ale získat došlo k chybě přidělení.</span><span class="sxs-lookup"><span data-stu-id="e73df-128">You try tooresize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="e73df-129">Příčina</span><span class="sxs-lookup"><span data-stu-id="e73df-129">Cause</span></span>
<span data-ttu-id="e73df-130">Hello tooresize hello virtuálních počítačů má toobe pokusem o zpracování požadavku v původním clusteru hello dané hostitelů hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e73df-130">hello request tooresize hello VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="e73df-131">Ale hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e73df-131">However, hello cluster does not support hello requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="e73df-132">Řešení</span><span class="sxs-lookup"><span data-stu-id="e73df-132">Resolution</span></span>
<span data-ttu-id="e73df-133">Snižte hello požadovaná velikost virtuálního počítače a opakujte hello změnit velikost žádosti.</span><span class="sxs-lookup"><span data-stu-id="e73df-133">Reduce hello requested VM size, and retry hello resize request.</span></span>

* <span data-ttu-id="e73df-134">Klikněte na tlačítko **procházet všechny** > **virtuálních počítačů (klasické)** > *virtuálního počítače* > **nastavení**  >  **Velikost**.</span><span class="sxs-lookup"><span data-stu-id="e73df-134">Click **Browse all** > **Virtual machines (classic)** > *your virtual machine* > **Settings** > **Size**.</span></span> <span data-ttu-id="e73df-135">Podrobné pokyny najdete v tématu [změnit velikost virtuálního počítače hello](https://msdn.microsoft.com/library/dn168976.aspx).</span><span class="sxs-lookup"><span data-stu-id="e73df-135">For detailed steps, see [Resize hello virtual machine](https://msdn.microsoft.com/library/dn168976.aspx).</span></span>

<span data-ttu-id="e73df-136">Pokud není možné tooreduce hello velikost virtuálního počítače, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e73df-136">If it is not possible tooreduce hello VM size, follow these steps:</span></span>

* <span data-ttu-id="e73df-137">Vytvořte novou cloudovou službu, zajistíte, že není propojená skupiny vztahů tooan a není přidružen k virtuální síti, která je propojená tooan skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="e73df-137">Create a new cloud service, ensuring it is not linked tooan affinity group and not associated with a virtual network that is linked tooan affinity group.</span></span>
* <span data-ttu-id="e73df-138">Vytvoření nové, větší velikost virtuálního počítače v ní.</span><span class="sxs-lookup"><span data-stu-id="e73df-138">Create a new, larger-sized VM in it.</span></span>

<span data-ttu-id="e73df-139">Můžete sloučit všechny virtuální počítače v hello stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e73df-139">You can consolidate all your VMs in hello same cloud service.</span></span> <span data-ttu-id="e73df-140">Pokud je přidružený k virtuální síti na základě oblast stávající cloudovou službu, můžete se připojit hello nové cloudové služby toohello existující virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e73df-140">If your existing cloud service is associated with a region-based virtual network, you can connect hello new cloud service toohello existing virtual network.</span></span>

<span data-ttu-id="e73df-141">Pokud není přidružený k virtuální síti na základě oblast hello stávající cloudovou službu, pak máte toodelete hello virtuálních počítačů v hello stávající cloudovou službu a je v hello novou cloudovou službu z jejich disky znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e73df-141">If hello existing cloud service is not associated with a region-based virtual network, then you have toodelete hello VMs in hello existing cloud service, and recreate them in hello new cloud service from their disks.</span></span> <span data-ttu-id="e73df-142">Je však důležité tooremember, hello nová Cloudová služba bude mít nový název a VIP, takže bude potřeba tooupdate tyto pro všechny hello závislosti, které používají tyto informace pro hello stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="e73df-142">However, it is important tooremember that hello new cloud service will have a new name and VIP, so you will need tooupdate these for all hello dependencies that currently use this information for hello existing cloud service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e73df-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e73df-143">Next steps</span></span>
<span data-ttu-id="e73df-144">Pokud dojde k potížím při vytváření nového virtuálního počítače s Linuxem v Azure, najdete v části [řešení potíží s nasazením s vytvoření nového virtuálního počítače Linux v Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e73df-144">If you encounter issues when you create a new Linux VM in Azure, see [Troubleshoot deployment issues with creating a new Linux virtual machine in Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

