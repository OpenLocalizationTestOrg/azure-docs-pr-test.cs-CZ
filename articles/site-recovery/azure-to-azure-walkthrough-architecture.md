---
title: "Zkontrolujte architekturu pro replikaci virtuálních počítačů Azure mezi oblastmi Azure | Microsoft Docs"
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci virtuálních počítačů Azure mezi oblastmi Azure pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/12/2017
ms.author: raynew
ms.openlocfilehash: f471add4f4dee26482e2820fb06d010d6c0c0e88
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-1-review-the-architecture-for-azure-vm-replication-between-azure-regions"></a><span data-ttu-id="9c05c-103">Krok 1: Posouzení architekturu pro virtuální počítač Azure replikaci mezi oblastmi Azure</span><span class="sxs-lookup"><span data-stu-id="9c05c-103">Step 1: Review the architecture for Azure VM replication between Azure regions</span></span>


<span data-ttu-id="9c05c-104">Po zkontrolování [přehled kroků](azure-to-azure-walkthrough-overview.md) pro toto nasazení, přečtěte si tento článek vám pomůže porozumět součástech a procesech, které jsou používány pro replikaci a obnovení Azure virtuálních počítačů (VM) z jedné oblasti Azure do jiné, pomocí [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9c05c-104">After reviewing the [overview steps](azure-to-azure-walkthrough-overview.md) for this deployment, read this article to understand the components and processes used when replicating and recovering Azure virtual machines (VMs) from one Azure region to another, using [Azure Site Recovery](site-recovery-overview.md).</span></span>

- <span data-ttu-id="9c05c-105">Po dokončení článku měli vědět, jak funguje na jiné oblasti replikace virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9c05c-105">When you finish the article, you should have a clear understanding of how Azure VM replication to another region works.</span></span>
- <span data-ttu-id="9c05c-106">Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9c05c-106">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

>[!NOTE]
><span data-ttu-id="9c05c-107">Azure replikace virtuálního počítače se službou Site Recovery je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="9c05c-107">Azure VM replication with the Site Recovery service is currently in preview.</span></span>



## <a name="architectural-components"></a><span data-ttu-id="9c05c-108">Komponenty architektury</span><span class="sxs-lookup"><span data-stu-id="9c05c-108">Architectural components</span></span>

<span data-ttu-id="9c05c-109">Následující obrázek poskytuje podrobný pohled prostředí virtuálního počítače Azure v určité oblasti (v tomto příkladu umístění východní USA).</span><span class="sxs-lookup"><span data-stu-id="9c05c-109">The following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, the East US location).</span></span> <span data-ttu-id="9c05c-110">V prostředí virtuálního počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="9c05c-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="9c05c-111">Aplikace může mít spuštěný na virtuálních počítačích s disky, které šíří mezi různými účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="9c05c-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="9c05c-112">Virtuální počítače můžou být součástí jedné nebo více podsítí v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9c05c-112">The VMs can be included in one or more subnets within a virtual network.</span></span>

![prostředí zákazníka](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a><span data-ttu-id="9c05c-114">Proces replikace</span><span class="sxs-lookup"><span data-stu-id="9c05c-114">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="9c05c-115">Krok 1</span><span class="sxs-lookup"><span data-stu-id="9c05c-115">Step 1</span></span>

<span data-ttu-id="9c05c-116">Když povolíte replikaci virtuálního počítače Azure na portálu Azure, prostředky uvedené v následující obrázek a tabulka se automaticky vytvoří v cílové oblasti.</span><span class="sxs-lookup"><span data-stu-id="9c05c-116">When you enable Azure VM replication in the Azure portal, the resources shown in the following diagram and table are automatically created in the target region.</span></span> <span data-ttu-id="9c05c-117">Ve výchozím nastavení prostředky jsou vytvořeny na základě nastavení oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="9c05c-117">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="9c05c-118">Nastavení cílového podle potřeby můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="9c05c-118">You can customize the target settings as required.</span></span> <span data-ttu-id="9c05c-119">[Další informace](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9c05c-119">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Povolit replikaci proces, krok 1](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

<span data-ttu-id="9c05c-121">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="9c05c-121">**Resource**</span></span> | <span data-ttu-id="9c05c-122">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="9c05c-122">**Details**</span></span>
--- | ---
<span data-ttu-id="9c05c-123">**Cílová skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="9c05c-123">**Target resource group**</span></span> | <span data-ttu-id="9c05c-124">Skupinu prostředků, do které patří replikované virtuální počítače po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9c05c-124">The resource group to which replicated VMs belong after failover.</span></span>
<span data-ttu-id="9c05c-125">**Cílová virtuální síť**</span><span class="sxs-lookup"><span data-stu-id="9c05c-125">**Target virtual network**</span></span> | <span data-ttu-id="9c05c-126">Virtuální síť, ve kterém jsou replikované virtuální počítače umístěné po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9c05c-126">The virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="9c05c-127">Mapování sítě se vytvoří mezi zdrojovými a cílovými virtuální sítě a naopak.</span><span class="sxs-lookup"><span data-stu-id="9c05c-127">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="9c05c-128">**Účty úložiště mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="9c05c-128">**Cache storage accounts**</span></span> | <span data-ttu-id="9c05c-129">Předtím, než změny na zdrojové virtuální počítače jsou replikovány do cílového účtu úložiště, jsou sledovány a odešle na účet úložiště mezipaměti v cílovém umístění.</span><span class="sxs-lookup"><span data-stu-id="9c05c-129">Before changes on source VMs are replicated to the target storage account, they are tracked and sent to the cache storage account in the target location.</span></span> <span data-ttu-id="9c05c-130">Tím se zajistí minimální dopad na produkční aplikace běžící na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="9c05c-130">This ensures minimal impact on production apps running on the VM.</span></span>
<span data-ttu-id="9c05c-131">**Cílové úložiště účty**</span><span class="sxs-lookup"><span data-stu-id="9c05c-131">**Target storage accounts**</span></span>  | <span data-ttu-id="9c05c-132">Účty úložiště v cílovém umístění, do které se replikují data.</span><span class="sxs-lookup"><span data-stu-id="9c05c-132">Storage accounts in the target location to which the data is replicated.</span></span>
<span data-ttu-id="9c05c-133">**Cílové skupiny dostupnosti**</span><span class="sxs-lookup"><span data-stu-id="9c05c-133">**Target availability sets**</span></span>  | <span data-ttu-id="9c05c-134">Sady dostupnosti, které jsou replikované virtuální počítače umístěné po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9c05c-134">Availability sets in which the replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="9c05c-135">Krok 2</span><span class="sxs-lookup"><span data-stu-id="9c05c-135">Step 2</span></span>

<span data-ttu-id="9c05c-136">Jak je zapnutá replikace, rozšíření Site Recovery Mobility service se automaticky nainstaluje na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="9c05c-136">As replication is enabled, the Site Recovery extension Mobility service is automatically installed on the VM.</span></span> <span data-ttu-id="9c05c-137">Dojde k následujícímu:</span><span class="sxs-lookup"><span data-stu-id="9c05c-137">The following occurs:</span></span>

1. <span data-ttu-id="9c05c-138">Virtuální počítač není zaregistrována Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9c05c-138">The VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="9c05c-139">Pro virtuální počítač je nakonfigurován průběžnou replikaci.</span><span class="sxs-lookup"><span data-stu-id="9c05c-139">Continuous replication is configured for the VM.</span></span> <span data-ttu-id="9c05c-140">Datové zápisy na disky virtuálních počítačů se přenášejí nepřetržitě k účtu úložiště mezipaměti ve zdrojovém umístění.</span><span class="sxs-lookup"><span data-stu-id="9c05c-140">Data writes on the VM disks are continuously transferred to the cache storage account in the source location.</span></span>

   ![Povolit replikaci proces, krok 2](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  <span data-ttu-id="9c05c-142">Všimněte si, že Site Recovery nikdy potřebuje příchozí připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="9c05c-142">Note that Site Recovery never needs inbound connectivity to the VM.</span></span> <span data-ttu-id="9c05c-143">Je potřeba jenom odchozí připojení k Site Recovery service adresy URL nebo IP adresy, Office 365 ověřování adresy URL nebo IP adres a adres IP účet úložiště mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9c05c-143">Only outbound connectivity to Site Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses is needed.</span></span> 

## <a name="continuous-replication-process"></a><span data-ttu-id="9c05c-144">Proces průběžná replikace</span><span class="sxs-lookup"><span data-stu-id="9c05c-144">Continuous replication process</span></span>

<span data-ttu-id="9c05c-145">Po průběžné replikace funguje, se k účtu úložiště mezipaměti okamžitě přenesou zápisy na disk.</span><span class="sxs-lookup"><span data-stu-id="9c05c-145">After continuous replication is working, disk writes are immediately transferred to the cache storage account.</span></span> <span data-ttu-id="9c05c-146">Site Recovery zpracovává data a odešle ji do cílového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9c05c-146">Site Recovery processes the data and sends it to the target storage account.</span></span> <span data-ttu-id="9c05c-147">Po zpracování dat, body obnovení jsou generovány v cílový účet úložiště každých několik minut.</span><span class="sxs-lookup"><span data-stu-id="9c05c-147">After the data is processed, recovery points are generated in the target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="9c05c-148">Proces převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="9c05c-148">Failover process</span></span>

<span data-ttu-id="9c05c-149">Při zahájení převzetí služeb při selhání, virtuální počítače jsou vytvořeny v cílové skupině prostředků, cílová virtuální síť, cílové podsíti a skupina dostupnosti cíl.</span><span class="sxs-lookup"><span data-stu-id="9c05c-149">When you initiate a failover, the VMs are created in the target resource group, target virtual network, target subnet, and the target availability set.</span></span> <span data-ttu-id="9c05c-150">Při selhání můžete použít jakýkoli bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="9c05c-150">During a failover, you can use any recovery point.</span></span>

![Proces převzetí služeb při selhání](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="9c05c-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c05c-152">Next steps</span></span>

<span data-ttu-id="9c05c-153">Přejděte na [krok 2: ověření předpoklady a omezení](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="9c05c-153">Go to [Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>
