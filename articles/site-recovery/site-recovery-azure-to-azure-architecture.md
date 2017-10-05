---
title: "Jak funguje virtuální počítač Azure replikace mezi oblastmi Azure ve službě Azure Site Recovery?  | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci virtuálních počítačů Azure mezi oblastmi Azure pomocí služby Azure Site Recovery."
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
ms.openlocfilehash: ec397eaeda963f257d1bd996f1f57189bcde17ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a><span data-ttu-id="9df52-104">Jak funguje replikace virtuálního počítače Azure ve službě Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="9df52-104">How does Azure VM replication work in Site Recovery?</span></span>


<span data-ttu-id="9df52-105">Tento článek popisuje součástech a procesech součástí replikaci a obnovení virtuálních počítačů Azure (VM) z jedné oblasti na jiný pomocí [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="9df52-105">This article describes the components and processes involved in replicating and recovering Azure virtual machines (VMs) from one region to another by using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

>[!NOTE]
><span data-ttu-id="9df52-106">Azure replikace virtuálního počítače se službou Site Recovery je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="9df52-106">Azure VM replication with the Site Recovery service is currently in preview.</span></span>

<span data-ttu-id="9df52-107">Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9df52-107">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="architectural-components"></a><span data-ttu-id="9df52-108">Komponenty architektury</span><span class="sxs-lookup"><span data-stu-id="9df52-108">Architectural components</span></span>

<span data-ttu-id="9df52-109">Následující obrázek poskytuje podrobný pohled prostředí virtuálního počítače Azure v určité oblasti (v tomto příkladu umístění východní USA).</span><span class="sxs-lookup"><span data-stu-id="9df52-109">The following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, the East US location).</span></span> <span data-ttu-id="9df52-110">V prostředí virtuálního počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="9df52-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="9df52-111">Aplikace může mít spuštěný na virtuálních počítačích s disky, které šíří mezi různými účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="9df52-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="9df52-112">Virtuální počítače můžou být součástí jedné nebo více podsítí v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9df52-112">The VMs can be included in one or more subnets within a virtual network.</span></span>

![prostředí zákazníka](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

<span data-ttu-id="9df52-114">Další informace o požadavcích pro nasazení a požadavky [matici podpory](site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9df52-114">Learn about the deployment prerequisites and requirements in the [support matrix](site-recovery-support-matrix-azure-to-azure.md).</span></span>

## <a name="replication-process"></a><span data-ttu-id="9df52-115">Proces replikace</span><span class="sxs-lookup"><span data-stu-id="9df52-115">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="9df52-116">Krok 1</span><span class="sxs-lookup"><span data-stu-id="9df52-116">Step 1</span></span>

<span data-ttu-id="9df52-117">Když povolíte replikaci virtuálního počítače Azure na portálu Azure, prostředky uvedené v následující obrázek a tabulka se automaticky vytvoří v cílové oblasti.</span><span class="sxs-lookup"><span data-stu-id="9df52-117">When you enable Azure VM replication in the Azure portal, the resources shown in the following diagram and table are automatically created in the target region.</span></span> <span data-ttu-id="9df52-118">Ve výchozím nastavení prostředky jsou vytvořeny na základě nastavení oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="9df52-118">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="9df52-119">Nastavení cílového podle potřeby můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="9df52-119">You can customize the target settings as required.</span></span> <span data-ttu-id="9df52-120">[Další informace](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9df52-120">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Povolit replikaci proces, krok 1](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

<span data-ttu-id="9df52-122">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="9df52-122">**Resource**</span></span> | <span data-ttu-id="9df52-123">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="9df52-123">**Details**</span></span>
--- | ---
<span data-ttu-id="9df52-124">**Cílová skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="9df52-124">**Target resource group**</span></span> | <span data-ttu-id="9df52-125">Skupinu prostředků, do které patří replikované virtuální počítače po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9df52-125">The resource group to which replicated VMs belong after failover.</span></span>
<span data-ttu-id="9df52-126">**Cílová virtuální síť**</span><span class="sxs-lookup"><span data-stu-id="9df52-126">**Target virtual network**</span></span> | <span data-ttu-id="9df52-127">Virtuální síť, ve kterém jsou replikované virtuální počítače umístěné po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9df52-127">The virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="9df52-128">Mapování sítě se vytvoří mezi zdrojovými a cílovými virtuální sítě a naopak.</span><span class="sxs-lookup"><span data-stu-id="9df52-128">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="9df52-129">**Účty úložiště mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="9df52-129">**Cache storage accounts**</span></span> | <span data-ttu-id="9df52-130">Předtím, než změny na zdrojové virtuální počítače jsou replikovány do cílového účtu úložiště, jsou sledovány a odešle na účet úložiště mezipaměti v cílovém umístění.</span><span class="sxs-lookup"><span data-stu-id="9df52-130">Before changes on source VMs are replicated to the target storage account, they are tracked and sent to the cache storage account in the target location.</span></span> <span data-ttu-id="9df52-131">Tím se zajistí minimální dopad na produkční aplikace běžící na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="9df52-131">This ensures minimal impact on production apps running on the VM.</span></span>
<span data-ttu-id="9df52-132">**Cílové úložiště účty**</span><span class="sxs-lookup"><span data-stu-id="9df52-132">**Target storage accounts**</span></span>  | <span data-ttu-id="9df52-133">Účty úložiště v cílovém umístění, do které se replikují data.</span><span class="sxs-lookup"><span data-stu-id="9df52-133">Storage accounts in the target location to which the data is replicated.</span></span>
<span data-ttu-id="9df52-134">**Cílové skupiny dostupnosti**</span><span class="sxs-lookup"><span data-stu-id="9df52-134">**Target availability sets**</span></span>  | <span data-ttu-id="9df52-135">Sady dostupnosti, které jsou replikované virtuální počítače umístěné po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9df52-135">Availability sets in which the replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="9df52-136">Krok 2</span><span class="sxs-lookup"><span data-stu-id="9df52-136">Step 2</span></span>

<span data-ttu-id="9df52-137">Jak je zapnutá replikace, rozšíření Site Recovery Mobility service se automaticky nainstaluje na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="9df52-137">As replication is enabled, the Site Recovery extension Mobility service is automatically installed on the VM.</span></span> <span data-ttu-id="9df52-138">Dojde k následujícímu:</span><span class="sxs-lookup"><span data-stu-id="9df52-138">The following occurs:</span></span>

1. <span data-ttu-id="9df52-139">Virtuální počítač není zaregistrována Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9df52-139">The VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="9df52-140">Pro virtuální počítač je nakonfigurován průběžnou replikaci.</span><span class="sxs-lookup"><span data-stu-id="9df52-140">Continuous replication is configured for the VM.</span></span> <span data-ttu-id="9df52-141">Datové zápisy na disky virtuálních počítačů se přenášejí nepřetržitě k účtu úložiště mezipaměti ve zdrojovém umístění.</span><span class="sxs-lookup"><span data-stu-id="9df52-141">Data writes on the VM disks are continuously transferred to the cache storage account in the source location.</span></span>

   ![Povolit replikaci proces, krok 2](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > <span data-ttu-id="9df52-143">Site Recovery nikdy potřebuje příchozí připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="9df52-143">Site Recovery never needs inbound connectivity to the VM.</span></span> <span data-ttu-id="9df52-144">Virtuální počítač potřebuje pouze odchozí připojení k Site Recovery service adresy URL nebo IP adresy, Office 365 ověřování adresy URL nebo IP adres a adres IP účet úložiště mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9df52-144">The VM needs only outbound connectivity to Site Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses.</span></span> <span data-ttu-id="9df52-145">Další informace najdete v tématu [pokyny sítě pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9df52-145">For more information, see the [Networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md) article.</span></span>

## <a name="continuous-replication-process"></a><span data-ttu-id="9df52-146">Proces průběžná replikace</span><span class="sxs-lookup"><span data-stu-id="9df52-146">Continuous replication process</span></span>

<span data-ttu-id="9df52-147">Po průběžné replikace funguje, se k účtu úložiště mezipaměti okamžitě přenesou zápisy na disk.</span><span class="sxs-lookup"><span data-stu-id="9df52-147">After continuous replication is working, disk writes are immediately transferred to the cache storage account.</span></span> <span data-ttu-id="9df52-148">Site Recovery zpracovává data a odešle ji do cílového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9df52-148">Site Recovery processes the data and sends it to the target storage account.</span></span> <span data-ttu-id="9df52-149">Po zpracování dat, body obnovení jsou generovány v cílový účet úložiště každých několik minut.</span><span class="sxs-lookup"><span data-stu-id="9df52-149">After the data is processed, recovery points are generated in the target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="9df52-150">Proces převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="9df52-150">Failover process</span></span>

<span data-ttu-id="9df52-151">Při zahájení převzetí služeb při selhání, virtuální počítače jsou vytvořeny v cílové skupině prostředků, cílová virtuální síť, cílové podsíti a skupina dostupnosti cíl.</span><span class="sxs-lookup"><span data-stu-id="9df52-151">When you initiate a failover, the VMs are created in the target resource group, target virtual network, target subnet, and the target availability set.</span></span> <span data-ttu-id="9df52-152">Při selhání můžete použít jakýkoli bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="9df52-152">During a failover, you can use any recovery point.</span></span>

![Proces převzetí služeb při selhání](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="9df52-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9df52-154">Next steps</span></span>

- <span data-ttu-id="9df52-155">Další informace o [sítě](site-recovery-azure-to-azure-networking-guidance.md) pro replikaci virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9df52-155">Learn about [networking](site-recovery-azure-to-azure-networking-guidance.md) for Azure VM replication.</span></span>
- <span data-ttu-id="9df52-156">Postupujte podle návodu k [replikovat virtuální počítače Azure.](site-recovery-azure-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="9df52-156">Follow a walkthrough to [replicate Azure VMs.](site-recovery-azure-to-azure.md)</span></span>
