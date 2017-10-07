---
title: "aaaHow nemá virtuální počítač Azure replikace mezi pracovní oblasti v Azure Site Recovery?  | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci virtuálních počítačů Azure mezi oblastmi Azure pomocí služby Azure Site Recovery hello."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/29/2017
ms.author: sujayt
ms.openlocfilehash: 01eda83e490821f8afc8a2973c18a19453a2e84a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a><span data-ttu-id="fa846-104">Jak funguje replikace virtuálního počítače Azure ve službě Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="fa846-104">How does Azure VM replication work in Site Recovery?</span></span>


<span data-ttu-id="fa846-105">Tento článek popisuje hello součástech a procesech součástí replikaci a obnovení virtuálních počítačů Azure (VM) z jedné oblasti tooanother pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="fa846-105">This article describes hello components and processes involved in replicating and recovering Azure virtual machines (VMs) from one region tooanother by using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

>[!NOTE]
><span data-ttu-id="fa846-106">Azure replikace virtuálního počítače s hello služba Site Recovery je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="fa846-106">Azure VM replication with hello Site Recovery service is currently in preview.</span></span>

<span data-ttu-id="fa846-107">Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="fa846-107">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="architectural-components"></a><span data-ttu-id="fa846-108">Komponenty architektury</span><span class="sxs-lookup"><span data-stu-id="fa846-108">Architectural components</span></span>

<span data-ttu-id="fa846-109">Následující diagram Hello poskytuje podrobný pohled prostředí virtuálního počítače Azure v určité oblasti (v tomto příkladu hello umístění východní USA).</span><span class="sxs-lookup"><span data-stu-id="fa846-109">hello following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, hello East US location).</span></span> <span data-ttu-id="fa846-110">V prostředí virtuálního počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="fa846-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="fa846-111">Aplikace může mít spuštěný na virtuálních počítačích s disky, které šíří mezi různými účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="fa846-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="fa846-112">Hello virtuální počítače můžou být součástí jedné nebo více podsítí v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fa846-112">hello VMs can be included in one or more subnets within a virtual network.</span></span>

![prostředí zákazníka](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

<span data-ttu-id="fa846-114">Další informace o požadavcích pro nasazení hello a požadavky v hello [matici podpory](site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="fa846-114">Learn about hello deployment prerequisites and requirements in hello [support matrix](site-recovery-support-matrix-azure-to-azure.md).</span></span>

## <a name="replication-process"></a><span data-ttu-id="fa846-115">Proces replikace</span><span class="sxs-lookup"><span data-stu-id="fa846-115">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="fa846-116">Krok 1</span><span class="sxs-lookup"><span data-stu-id="fa846-116">Step 1</span></span>

<span data-ttu-id="fa846-117">Když povolíte replikaci virtuálního počítače Azure v hello portálu Azure, hello prostředků ukazuje hello následujícím diagramu a v tabulce jsou automaticky vytvářeny v hello cílová oblast.</span><span class="sxs-lookup"><span data-stu-id="fa846-117">When you enable Azure VM replication in hello Azure portal, hello resources shown in hello following diagram and table are automatically created in hello target region.</span></span> <span data-ttu-id="fa846-118">Ve výchozím nastavení prostředky jsou vytvořeny na základě nastavení oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="fa846-118">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="fa846-119">Nastavení cílového hello podle potřeby můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="fa846-119">You can customize hello target settings as required.</span></span> <span data-ttu-id="fa846-120">[Další informace](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="fa846-120">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Povolit replikaci proces, krok 1](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

<span data-ttu-id="fa846-122">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="fa846-122">**Resource**</span></span> | <span data-ttu-id="fa846-123">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="fa846-123">**Details**</span></span>
--- | ---
<span data-ttu-id="fa846-124">**Cílová skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="fa846-124">**Target resource group**</span></span> | <span data-ttu-id="fa846-125">Hello toowhich skupiny prostředků patří replikované virtuální počítače po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="fa846-125">hello resource group toowhich replicated VMs belong after failover.</span></span>
<span data-ttu-id="fa846-126">**Cílová virtuální síť**</span><span class="sxs-lookup"><span data-stu-id="fa846-126">**Target virtual network**</span></span> | <span data-ttu-id="fa846-127">Hello virtuální sítě ve kterém jsou replikované virtuální počítače umístěné po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="fa846-127">hello virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="fa846-128">Mapování sítě se vytvoří mezi zdrojovými a cílovými virtuální sítě a naopak.</span><span class="sxs-lookup"><span data-stu-id="fa846-128">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="fa846-129">**Účty úložiště mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="fa846-129">**Cache storage accounts**</span></span> | <span data-ttu-id="fa846-130">Předtím, než změny na zdroje, které virtuální počítače jsou replikovány toohello cílový účet úložiště, jsou sledovány a odeslat účet úložiště mezipaměti toohello v cílovém umístění hello.</span><span class="sxs-lookup"><span data-stu-id="fa846-130">Before changes on source VMs are replicated toohello target storage account, they are tracked and sent toohello cache storage account in hello target location.</span></span> <span data-ttu-id="fa846-131">Tím se zajistí minimální dopad na produkční aplikace běžící na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fa846-131">This ensures minimal impact on production apps running on hello VM.</span></span>
<span data-ttu-id="fa846-132">**Cílové úložiště účty**</span><span class="sxs-lookup"><span data-stu-id="fa846-132">**Target storage accounts**</span></span>  | <span data-ttu-id="fa846-133">Účty úložiště ve hello cílová umístění toowhich hello data se replikují.</span><span class="sxs-lookup"><span data-stu-id="fa846-133">Storage accounts in hello target location toowhich hello data is replicated.</span></span>
<span data-ttu-id="fa846-134">**Cílové skupiny dostupnosti**</span><span class="sxs-lookup"><span data-stu-id="fa846-134">**Target availability sets**</span></span>  | <span data-ttu-id="fa846-135">V které hello replikovat virtuální počítače se po převzetí služeb při selhání nacházejí sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="fa846-135">Availability sets in which hello replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="fa846-136">Krok 2</span><span class="sxs-lookup"><span data-stu-id="fa846-136">Step 2</span></span>

<span data-ttu-id="fa846-137">Jak je zapnutá replikace, hello rozšíření Site Recovery Mobility service se automaticky nainstaluje na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fa846-137">As replication is enabled, hello Site Recovery extension Mobility service is automatically installed on hello VM.</span></span> <span data-ttu-id="fa846-138">dojde k následujícímu Hello:</span><span class="sxs-lookup"><span data-stu-id="fa846-138">hello following occurs:</span></span>

1. <span data-ttu-id="fa846-139">Hello virtuálního počítače je registrovaný pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="fa846-139">hello VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="fa846-140">Průběžná replikace je nakonfigurován pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fa846-140">Continuous replication is configured for hello VM.</span></span> <span data-ttu-id="fa846-141">Data zapíše na hello virtuálních počítačů jsou disky nepřetržitě přenést toohello účet úložiště mezipaměti v umístění zdroje hello.</span><span class="sxs-lookup"><span data-stu-id="fa846-141">Data writes on hello VM disks are continuously transferred toohello cache storage account in hello source location.</span></span>

   ![Povolit replikaci proces, krok 2](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > <span data-ttu-id="fa846-143">Site Recovery nikdy musí toohello příchozí připojení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fa846-143">Site Recovery never needs inbound connectivity toohello VM.</span></span> <span data-ttu-id="fa846-144">Hello virtuálního počítače je potřeba jenom odchozí připojení tooSite obnovení služby adresy URL nebo IP adresy, Office 365 ověřování adresy URL nebo IP adresy a mezipaměti úložiště účet IP adresy.</span><span class="sxs-lookup"><span data-stu-id="fa846-144">hello VM needs only outbound connectivity tooSite Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses.</span></span> <span data-ttu-id="fa846-145">Další informace najdete v tématu hello [pokyny sítě pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md) článku.</span><span class="sxs-lookup"><span data-stu-id="fa846-145">For more information, see hello [Networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md) article.</span></span>

## <a name="continuous-replication-process"></a><span data-ttu-id="fa846-146">Proces průběžná replikace</span><span class="sxs-lookup"><span data-stu-id="fa846-146">Continuous replication process</span></span>

<span data-ttu-id="fa846-147">Po průběžné replikace funguje, disk, který provede zápis jsou okamžitě přenést toohello účet úložiště mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="fa846-147">After continuous replication is working, disk writes are immediately transferred toohello cache storage account.</span></span> <span data-ttu-id="fa846-148">Site Recovery zpracovává hello data a odešle ji toohello cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="fa846-148">Site Recovery processes hello data and sends it toohello target storage account.</span></span> <span data-ttu-id="fa846-149">Po zpracování dat hello body obnovení jsou generovány v hello cílový účet úložiště každých několik minut.</span><span class="sxs-lookup"><span data-stu-id="fa846-149">After hello data is processed, recovery points are generated in hello target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="fa846-150">Proces převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="fa846-150">Failover process</span></span>

<span data-ttu-id="fa846-151">Když iniciujete převzetí služeb při selhání, cílové hello, které jsou virtuální počítače vytvořené v hello cílová skupina prostředků, cílová virtuální síť, cílové podsíti a hello skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="fa846-151">When you initiate a failover, hello VMs are created in hello target resource group, target virtual network, target subnet, and hello target availability set.</span></span> <span data-ttu-id="fa846-152">Při selhání můžete použít jakýkoli bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="fa846-152">During a failover, you can use any recovery point.</span></span>

![Proces převzetí služeb při selhání](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="fa846-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa846-154">Next steps</span></span>

- <span data-ttu-id="fa846-155">Další informace o [sítě](site-recovery-azure-to-azure-networking-guidance.md) pro replikaci virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="fa846-155">Learn about [networking](site-recovery-azure-to-azure-networking-guidance.md) for Azure VM replication.</span></span>
- <span data-ttu-id="fa846-156">Postupujte podle návod příliš[replikovat virtuální počítače Azure.](site-recovery-azure-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="fa846-156">Follow a walkthrough too[replicate Azure VMs.](site-recovery-azure-to-azure.md)</span></span>
