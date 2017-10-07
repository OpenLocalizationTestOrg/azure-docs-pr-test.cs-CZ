---
title: "Architektura hello aaaReview pro replikaci virtuálních počítačů Azure mezi oblastmi Azure | Microsoft Docs"
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci virtuálních počítačů Azure mezi oblastmi Azure pomocí služby Azure Site Recovery hello."
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
ms.openlocfilehash: 4caab4e7a764040f317201d1345c40c73f836d81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-azure-vm-replication-between-azure-regions"></a><span data-ttu-id="4977d-103">Krok 1: Posouzení hello architektura pro virtuální počítač Azure replikaci mezi oblastmi Azure</span><span class="sxs-lookup"><span data-stu-id="4977d-103">Step 1: Review hello architecture for Azure VM replication between Azure regions</span></span>


<span data-ttu-id="4977d-104">Po zkontrolování hello [přehled kroků](azure-to-azure-walkthrough-overview.md) pro toto nasazení, přečtěte si tento článek toounderstand hello součástech a procesech, které používají při replikaci a obnovení virtuálních počítačů Azure (VM) z jedné oblasti Azure tooanother pomocí [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4977d-104">After reviewing hello [overview steps](azure-to-azure-walkthrough-overview.md) for this deployment, read this article toounderstand hello components and processes used when replicating and recovering Azure virtual machines (VMs) from one Azure region tooanother, using [Azure Site Recovery](site-recovery-overview.md).</span></span>

- <span data-ttu-id="4977d-105">Po dokončení hello článku měli vědět, jak funguje oblast tooanother replikace virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="4977d-105">When you finish hello article, you should have a clear understanding of how Azure VM replication tooanother region works.</span></span>
- <span data-ttu-id="4977d-106">Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4977d-106">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

>[!NOTE]
><span data-ttu-id="4977d-107">Azure replikace virtuálního počítače s hello služba Site Recovery je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="4977d-107">Azure VM replication with hello Site Recovery service is currently in preview.</span></span>



## <a name="architectural-components"></a><span data-ttu-id="4977d-108">Komponenty architektury</span><span class="sxs-lookup"><span data-stu-id="4977d-108">Architectural components</span></span>

<span data-ttu-id="4977d-109">Následující diagram Hello poskytuje podrobný pohled prostředí virtuálního počítače Azure v určité oblasti (v tomto příkladu hello umístění východní USA).</span><span class="sxs-lookup"><span data-stu-id="4977d-109">hello following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, hello East US location).</span></span> <span data-ttu-id="4977d-110">V prostředí virtuálního počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="4977d-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="4977d-111">Aplikace může mít spuštěný na virtuálních počítačích s disky, které šíří mezi různými účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="4977d-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="4977d-112">Hello virtuální počítače můžou být součástí jedné nebo více podsítí v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4977d-112">hello VMs can be included in one or more subnets within a virtual network.</span></span>

![prostředí zákazníka](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a><span data-ttu-id="4977d-114">Proces replikace</span><span class="sxs-lookup"><span data-stu-id="4977d-114">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="4977d-115">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4977d-115">Step 1</span></span>

<span data-ttu-id="4977d-116">Když povolíte replikaci virtuálního počítače Azure v hello portálu Azure, hello prostředků ukazuje hello následujícím diagramu a v tabulce jsou automaticky vytvářeny v hello cílová oblast.</span><span class="sxs-lookup"><span data-stu-id="4977d-116">When you enable Azure VM replication in hello Azure portal, hello resources shown in hello following diagram and table are automatically created in hello target region.</span></span> <span data-ttu-id="4977d-117">Ve výchozím nastavení prostředky jsou vytvořeny na základě nastavení oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="4977d-117">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="4977d-118">Nastavení cílového hello podle potřeby můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="4977d-118">You can customize hello target settings as required.</span></span> <span data-ttu-id="4977d-119">[Další informace](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="4977d-119">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Povolit replikaci proces, krok 1](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

<span data-ttu-id="4977d-121">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="4977d-121">**Resource**</span></span> | <span data-ttu-id="4977d-122">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="4977d-122">**Details**</span></span>
--- | ---
<span data-ttu-id="4977d-123">**Cílová skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="4977d-123">**Target resource group**</span></span> | <span data-ttu-id="4977d-124">Hello toowhich skupiny prostředků patří replikované virtuální počítače po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="4977d-124">hello resource group toowhich replicated VMs belong after failover.</span></span>
<span data-ttu-id="4977d-125">**Cílová virtuální síť**</span><span class="sxs-lookup"><span data-stu-id="4977d-125">**Target virtual network**</span></span> | <span data-ttu-id="4977d-126">Hello virtuální sítě ve kterém jsou replikované virtuální počítače umístěné po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="4977d-126">hello virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="4977d-127">Mapování sítě se vytvoří mezi zdrojovými a cílovými virtuální sítě a naopak.</span><span class="sxs-lookup"><span data-stu-id="4977d-127">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="4977d-128">**Účty úložiště mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="4977d-128">**Cache storage accounts**</span></span> | <span data-ttu-id="4977d-129">Předtím, než změny na zdroje, které virtuální počítače jsou replikovány toohello cílový účet úložiště, jsou sledovány a odeslat účet úložiště mezipaměti toohello v cílovém umístění hello.</span><span class="sxs-lookup"><span data-stu-id="4977d-129">Before changes on source VMs are replicated toohello target storage account, they are tracked and sent toohello cache storage account in hello target location.</span></span> <span data-ttu-id="4977d-130">Tím se zajistí minimální dopad na produkční aplikace běžící na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4977d-130">This ensures minimal impact on production apps running on hello VM.</span></span>
<span data-ttu-id="4977d-131">**Cílové úložiště účty**</span><span class="sxs-lookup"><span data-stu-id="4977d-131">**Target storage accounts**</span></span>  | <span data-ttu-id="4977d-132">Účty úložiště ve hello cílová umístění toowhich hello data se replikují.</span><span class="sxs-lookup"><span data-stu-id="4977d-132">Storage accounts in hello target location toowhich hello data is replicated.</span></span>
<span data-ttu-id="4977d-133">**Cílové skupiny dostupnosti**</span><span class="sxs-lookup"><span data-stu-id="4977d-133">**Target availability sets**</span></span>  | <span data-ttu-id="4977d-134">V které hello replikovat virtuální počítače se po převzetí služeb při selhání nacházejí sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="4977d-134">Availability sets in which hello replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="4977d-135">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4977d-135">Step 2</span></span>

<span data-ttu-id="4977d-136">Jak je zapnutá replikace, hello rozšíření Site Recovery Mobility service se automaticky nainstaluje na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4977d-136">As replication is enabled, hello Site Recovery extension Mobility service is automatically installed on hello VM.</span></span> <span data-ttu-id="4977d-137">dojde k následujícímu Hello:</span><span class="sxs-lookup"><span data-stu-id="4977d-137">hello following occurs:</span></span>

1. <span data-ttu-id="4977d-138">Hello virtuálního počítače je registrovaný pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="4977d-138">hello VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="4977d-139">Průběžná replikace je nakonfigurován pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4977d-139">Continuous replication is configured for hello VM.</span></span> <span data-ttu-id="4977d-140">Data zapíše na hello virtuálních počítačů jsou disky nepřetržitě přenést toohello účet úložiště mezipaměti v umístění zdroje hello.</span><span class="sxs-lookup"><span data-stu-id="4977d-140">Data writes on hello VM disks are continuously transferred toohello cache storage account in hello source location.</span></span>

   ![Povolit replikaci proces, krok 2](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  <span data-ttu-id="4977d-142">Všimněte si, že Site Recovery nikdy potřebuje, příchozí připojení toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4977d-142">Note that Site Recovery never needs inbound connectivity toohello VM.</span></span> <span data-ttu-id="4977d-143">Pouze odchozí připojení služby zotavení tooSite adresy URL nebo IP adresy, adresy URL nebo IP adresy ověřování Office 365 a mezipaměti úložiště účet IP adresy je potřeba.</span><span class="sxs-lookup"><span data-stu-id="4977d-143">Only outbound connectivity tooSite Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses is needed.</span></span> 

## <a name="continuous-replication-process"></a><span data-ttu-id="4977d-144">Proces průběžná replikace</span><span class="sxs-lookup"><span data-stu-id="4977d-144">Continuous replication process</span></span>

<span data-ttu-id="4977d-145">Po průběžné replikace funguje, disk, který provede zápis jsou okamžitě přenést toohello účet úložiště mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4977d-145">After continuous replication is working, disk writes are immediately transferred toohello cache storage account.</span></span> <span data-ttu-id="4977d-146">Site Recovery zpracovává hello data a odešle ji toohello cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4977d-146">Site Recovery processes hello data and sends it toohello target storage account.</span></span> <span data-ttu-id="4977d-147">Po zpracování dat hello body obnovení jsou generovány v hello cílový účet úložiště každých několik minut.</span><span class="sxs-lookup"><span data-stu-id="4977d-147">After hello data is processed, recovery points are generated in hello target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="4977d-148">Proces převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="4977d-148">Failover process</span></span>

<span data-ttu-id="4977d-149">Když iniciujete převzetí služeb při selhání, cílové hello, které jsou virtuální počítače vytvořené v hello cílová skupina prostředků, cílová virtuální síť, cílové podsíti a hello skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="4977d-149">When you initiate a failover, hello VMs are created in hello target resource group, target virtual network, target subnet, and hello target availability set.</span></span> <span data-ttu-id="4977d-150">Při selhání můžete použít jakýkoli bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="4977d-150">During a failover, you can use any recovery point.</span></span>

![Proces převzetí služeb při selhání](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="4977d-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4977d-152">Next steps</span></span>

<span data-ttu-id="4977d-153">Přejděte příliš[krok 2: ověření předpoklady a omezení](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="4977d-153">Go too[Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>
