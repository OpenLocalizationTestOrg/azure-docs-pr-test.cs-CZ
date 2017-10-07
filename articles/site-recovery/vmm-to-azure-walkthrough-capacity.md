---
title: "aaaPlan kapacity a škálování pro virtuální počítač Hyper-V tooAzure replikace (s VMM) s Azure Site Recovery | Microsoft Docs"
description: "Použijte tento článek tooplan kapacity a škálování při replikaci virtuálních počítačů Hyper-V v nástroji VMM cloudů tooAzure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 9818ada9bb21f60ac00b3894696201b06630cb2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-with-vmm-tooazure-replication"></a><span data-ttu-id="c3b22-103">Krok 3: Plánování kapacity a škálování pro replikaci tooAzure technologie Hyper-V (s nástrojem VMM)</span><span class="sxs-lookup"><span data-stu-id="c3b22-103">Step 3: Plan capacity and scaling for Hyper-V (with VMM) tooAzure replication</span></span>

<span data-ttu-id="c3b22-104">Po zkontrolování hello [požadavky nasazení](vmm-to-azure-walkthrough-prerequisites.md), použijte tento článek toofigure se plánování kapacity a škálování při replikaci místní virtuální počítače Hyper-V v cloudech tooAzure System Center Virtual Machine Manager (VMM), s [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c3b22-104">After you've reviewed hello [deployment prerequisites](vmm-to-azure-walkthrough-prerequisites.md), use this article toofigure out planning for capacity and scaling, when replicating on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds tooAzure, with [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="c3b22-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c3b22-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="how-do-i-start-capacity-planning"></a><span data-ttu-id="c3b22-106">Jak spustit plánování kapacity?</span><span class="sxs-lookup"><span data-stu-id="c3b22-106">How do I start capacity planning?</span></span>


<span data-ttu-id="c3b22-107">Můžete shromáždit informace o prostředí replikace a pak plánování kapacity pomocí hello data, společně s hello aspekty zvýrazněných v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c3b22-107">You gather information about your replication environment, and then plan capacity using hello data, together with hello considerations highlighted in this article.</span></span>


## <a name="gather-information"></a><span data-ttu-id="c3b22-108">Shromážděte informace</span><span class="sxs-lookup"><span data-stu-id="c3b22-108">Gather information</span></span>

1. <span data-ttu-id="c3b22-109">Získat informace o prostředí replikace, včetně virtuální počítačů, disků na virtuální počítač a úložišti na disk.</span><span class="sxs-lookup"><span data-stu-id="c3b22-109">Gather information about your replication environment, including VMs, disks per VMs, and storage per disk.</span></span>
2. <span data-ttu-id="c3b22-110">Určete vaše denní míry změn s ohledem pro replikovaná data.</span><span class="sxs-lookup"><span data-stu-id="c3b22-110">Identify your daily change (churn) rate for replicated data.</span></span> <span data-ttu-id="c3b22-111">Stáhnout hello [nástroj plánování kapacity služby technologie Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) míru změn tooget hello.</span><span class="sxs-lookup"><span data-stu-id="c3b22-111">Download hello [Hyper-V capacity planning tool](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello change rate.</span></span> <span data-ttu-id="c3b22-112">Doporučujeme, aby že tento nástroj spustíte přes toocapture týden průměry.</span><span class="sxs-lookup"><span data-stu-id="c3b22-112">We recommend you run this tool over a week toocapture averages.</span></span>
 

## <a name="figure-out-capacity"></a><span data-ttu-id="c3b22-113">Zjistěte, kapacity</span><span class="sxs-lookup"><span data-stu-id="c3b22-113">Figure out capacity</span></span>

<span data-ttu-id="c3b22-114">Na základě informací hello jste shromáždění spustíte hello [nástroje Plánovač kapacity obnovení lokality](http://aka.ms/asr-capacity-planner-excel) tooanalyze vaše zdrojové prostředí a úlohy, odhadnout požadavky na šířku pásma a server pro umístění zdroje hello a hello prostředky (virtuální počítače a úložiště atd), které je třeba v cílovém umístění hello.</span><span class="sxs-lookup"><span data-stu-id="c3b22-114">Based on hello information you've gather, you run hello [Site Recovery Capacity Planner Tool](http://aka.ms/asr-capacity-planner-excel) tooanalyze your source environment and workloads, estimate bandwidth needs and server resources for hello source location, and hello resources (virtual machines and storage etc), that you need in hello target location.</span></span> <span data-ttu-id="c3b22-115">Hello nástroj můžete spustit v několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="c3b22-115">You can run hello tool in a couple of modes:</span></span>

- <span data-ttu-id="c3b22-116">Rychlé plánování: Spusťte nástroj hello v tomto režimu tooget sítě a serveru projekce založené na průměrném počtu virtuálních počítačů, disků, úložiště a míru změn.</span><span class="sxs-lookup"><span data-stu-id="c3b22-116">Quick planning: Run hello tool in this mode tooget network and server projections based on an average number of VMs, disks, storage, and change rate.</span></span>
- <span data-ttu-id="c3b22-117">Podrobné plánování: Spusťte nástroj hello v tomto režimu a uveďte podrobné informace o každé zatížení na úrovni virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c3b22-117">Detailed planning: Run hello tool in this mode, and provide details of each workload at VM level.</span></span> <span data-ttu-id="c3b22-118">Analýza kompatibility virtuálních počítačů a získat projekce sítě a serveru.</span><span class="sxs-lookup"><span data-stu-id="c3b22-118">Analyze VM compatibility and get network and server projections.</span></span>

<span data-ttu-id="c3b22-119">Nyní spusťte nástroj hello:</span><span class="sxs-lookup"><span data-stu-id="c3b22-119">Now run hello tool:</span></span>

1. <span data-ttu-id="c3b22-120">Stáhnout hello [nástroj](http://aka.ms/asr-capacity-planner-excel)</span><span class="sxs-lookup"><span data-stu-id="c3b22-120">Download hello [tool](http://aka.ms/asr-capacity-planner-excel)</span></span>
2. <span data-ttu-id="c3b22-121">toorun hello rychlé planner, postupujte podle [tyto pokyny](site-recovery-capacity-planner.md#run-the-quick-planner)a vyberte hello scénář **tooAzure technologie Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="c3b22-121">toorun hello quick planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-quick-planner), and select hello scenario **Hyper-V tooAzure**.</span></span>
3. <span data-ttu-id="c3b22-122">toorun hello podrobné planner, postupujte podle [tyto pokyny](site-recovery-capacity-planner.md#run-the-detailed-planner)a vyberte hello scénář **technologie Hyper-V tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="c3b22-122">toorun hello detailed planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-detailed-planner), and select hello scenario **Hyper-V tooAzure**.</span></span>

## <a name="control-network-bandwidth"></a><span data-ttu-id="c3b22-123">Řídí šířku pásma sítě</span><span class="sxs-lookup"><span data-stu-id="c3b22-123">Control network bandwidth</span></span>

<span data-ttu-id="c3b22-124">Poté, co jste počítané hello šířky pásma, které potřebujete, máte několik možností pro ovládání hello objemu šířky pásma používané pro replikaci:</span><span class="sxs-lookup"><span data-stu-id="c3b22-124">After you've calculated hello bandwidth you need, you have a couple of options for controlling hello amount of bandwidth used for replication:</span></span>

* <span data-ttu-id="c3b22-125">**Omezení šířky pásma**: přenos Hyper-V, která se replikují tooAzure prochází konkrétního hostitele technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="c3b22-125">**Throttle bandwidth**: Hyper-V traffic that replicates tooAzure goes through a specific Hyper-V host.</span></span> <span data-ttu-id="c3b22-126">Můžete omezit šířku pásma na hostitelském serveru hello.</span><span class="sxs-lookup"><span data-stu-id="c3b22-126">You can throttle bandwidth on hello host server.</span></span>
* <span data-ttu-id="c3b22-127">**Ovlivnění šířky pásma**: můžete ovlivnit hello pásma sítě používané pro replikaci pomocí několika klíčů registru.</span><span class="sxs-lookup"><span data-stu-id="c3b22-127">**Influence bandwidth**: You can influence hello bandwidth used for replication using a couple of registry keys.</span></span>

### <a name="throttle-bandwidth"></a><span data-ttu-id="c3b22-128">Omezení šířky pásma</span><span class="sxs-lookup"><span data-stu-id="c3b22-128">Throttle bandwidth</span></span>
1. <span data-ttu-id="c3b22-129">Otevřete hello Microsoft Azure Backup konzoly MMC modul snap-in na serveru hostitele technologie Hyper-V hello.</span><span class="sxs-lookup"><span data-stu-id="c3b22-129">Open hello Microsoft Azure Backup MMC snap-in on hello Hyper-V host server.</span></span> <span data-ttu-id="c3b22-130">Ve výchozím nastavení je k dispozici na ploše hello nebo v C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="c3b22-130">By default a shortcut for Microsoft Azure Backup is available on hello desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="c3b22-131">V modulu snap-in hello klikněte na **změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c3b22-131">In hello snap-in click **Change Properties**.</span></span>
3. <span data-ttu-id="c3b22-132">Na hello **omezování** vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení**a nastavte hello limity pro pracovní a nepracovní dobu.</span><span class="sxs-lookup"><span data-stu-id="c3b22-132">On hello **Throttling** tab select **Enable internet bandwidth usage throttling for backup operations**, and set hello limits for work and non-work hours.</span></span> <span data-ttu-id="c3b22-133">Platné rozsahy jsou 512 kB/s too102 MB/s za sekundu.</span><span class="sxs-lookup"><span data-stu-id="c3b22-133">Valid ranges are from 512 Kbps too102 Mbps per second.</span></span>

    ![Omezení šířky pásma](./media/vmm-to-azure-walkthrough-capacity/throttle2.png)

<span data-ttu-id="c3b22-135">Můžete taky hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) rutiny tooset omezení.</span><span class="sxs-lookup"><span data-stu-id="c3b22-135">You can also use hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset throttling.</span></span> <span data-ttu-id="c3b22-136">Tady je ukázkový skript:</span><span class="sxs-lookup"><span data-stu-id="c3b22-136">Here's a sample:</span></span>

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

<span data-ttu-id="c3b22-137">**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.</span><span class="sxs-lookup"><span data-stu-id="c3b22-137">**Set-OBMachineSetting -NoThrottle** indicates that no throttling is required.</span></span>

### <a name="influence-network-bandwidth"></a><span data-ttu-id="c3b22-138">Ovlivnění šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="c3b22-138">Influence network bandwidth</span></span>
1. <span data-ttu-id="c3b22-139">V registru hello přejděte příliš**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span><span class="sxs-lookup"><span data-stu-id="c3b22-139">In hello registry navigate too**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span></span>
   * <span data-ttu-id="c3b22-140">tooinfluence hello šířky pásma provoz na replikaci disku, upravte hodnotu hello hello **UploadThreadsPerVM**, nebo vytvořte klíč hello, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c3b22-140">tooinfluence hello bandwidth traffic on a replicating disk, modify hello value hello **UploadThreadsPerVM**, or create hello key if it doesn't exist.</span></span>
   * <span data-ttu-id="c3b22-141">tooinfluence hello šířky pásma pro přenosy navrácení služeb po obnovení z Azure, upravte hodnotu hello **DownloadThreadsPerVM**.</span><span class="sxs-lookup"><span data-stu-id="c3b22-141">tooinfluence hello bandwidth for failback traffic from Azure, modify hello value **DownloadThreadsPerVM**.</span></span>
2. <span data-ttu-id="c3b22-142">Hello výchozí hodnota je 4.</span><span class="sxs-lookup"><span data-stu-id="c3b22-142">hello default value is 4.</span></span> <span data-ttu-id="c3b22-143">V síti "nadměrným zřízením" tyto klíče registru by mělo být změněno z hello výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c3b22-143">In an “overprovisioned” network, these registry keys should be changed from hello default values.</span></span> <span data-ttu-id="c3b22-144">maximální Hello je 32.</span><span class="sxs-lookup"><span data-stu-id="c3b22-144">hello maximum is 32.</span></span> <span data-ttu-id="c3b22-145">Monitorování přenosů toooptimize hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c3b22-145">Monitor traffic toooptimize hello value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3b22-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c3b22-146">Next steps</span></span>

<span data-ttu-id="c3b22-147">Přejděte příliš[krok 4: plánování sítě](vmm-to-azure-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="c3b22-147">Go too[Step 4: Plan networking](vmm-to-azure-walkthrough-network.md).</span></span>
