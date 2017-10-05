---
title: "Plánování kapacity a škálování pro replikaci virtuálních počítačů technologie Hyper-V (bez VMM) do Azure s Azure Site Recovery | Microsoft Docs"
description: "Použijte tento článek k plánování kapacity a škálování při replikaci virtuálních počítačů Hyper-V do Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8687af60-5b50-481c-98ee-0750cbbc2c57
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: c7891c188c2cecbbf056fa79672a13bb16fa7fcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-to-azure-replication"></a><span data-ttu-id="c41ce-103">Krok 3: Plánování kapacity a škálování pro Hyper-V na Azure replikaci</span><span class="sxs-lookup"><span data-stu-id="c41ce-103">Step 3: Plan capacity and scaling for Hyper-V to Azure replication</span></span>

<span data-ttu-id="c41ce-104">Pomocí tohoto článku a pokuste se zjistit plánování kapacity a škálování, při replikaci virtuálních počítačů technologie Hyper-V v místě (bez System Center VMM) do Azure s [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c41ce-104">Use this article to figure out planning for capacity and scaling, when replicating on-premises Hyper-V VMs (without System Center VMM) to Azure with [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="c41ce-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c41ce-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="how-do-i-start-capacity-planning"></a><span data-ttu-id="c41ce-106">Jak spustit plánování kapacity?</span><span class="sxs-lookup"><span data-stu-id="c41ce-106">How do I start capacity planning?</span></span>


<span data-ttu-id="c41ce-107">Shromážděte informace o prostředí replikace a pak plánování kapacity, na základě těchto informací, společně s aspekty zvýrazněných v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c41ce-107">You gather information about your replication environment, and then plan capacity using this information, together with the considerations highlighted in this article.</span></span>


## <a name="gather-information"></a><span data-ttu-id="c41ce-108">Shromážděte informace</span><span class="sxs-lookup"><span data-stu-id="c41ce-108">Gather information</span></span>

1. <span data-ttu-id="c41ce-109">Získat informace o prostředí replikace, včetně virtuální počítačů, disků na virtuální počítač a úložišti na disk.</span><span class="sxs-lookup"><span data-stu-id="c41ce-109">Gather information about your replication environment, including VMs, disks per VMs, and storage per disk.</span></span>
2. <span data-ttu-id="c41ce-110">Určete vaše denní míry změn s ohledem pro replikovaná data.</span><span class="sxs-lookup"><span data-stu-id="c41ce-110">Identify your daily change (churn) rate for replicated data.</span></span> <span data-ttu-id="c41ce-111">Stažení [nástroj plánování kapacity služby technologie Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) získat míru změn.</span><span class="sxs-lookup"><span data-stu-id="c41ce-111">Download the [Hyper-V capacity planning tool](https://www.microsoft.com/download/details.aspx?id=39057) to get the change rate.</span></span> <span data-ttu-id="c41ce-112">Doporučujeme, aby že tento nástroj spustíte přes týden k zachycení průměry.</span><span class="sxs-lookup"><span data-stu-id="c41ce-112">We recommend you run this tool over a week to capture averages.</span></span>
 

## <a name="figure-out-capacity"></a><span data-ttu-id="c41ce-113">Zjistěte, kapacity</span><span class="sxs-lookup"><span data-stu-id="c41ce-113">Figure out capacity</span></span>

<span data-ttu-id="c41ce-114">Na základě informací jste shromáždění, spusťte [nástroje Plánovač kapacity obnovení lokality](http://aka.ms/asr-capacity-planner-excel) analyzovat vaše zdrojové prostředí a úlohy, odhadnout požadavky na šířku pásma a prostředky serveru pro umístění zdroje a prostředky (virtuální počítače a úložiště atd), které je třeba v cílovém umístění.</span><span class="sxs-lookup"><span data-stu-id="c41ce-114">Based on the information you've gather, you run the [Site Recovery Capacity Planner Tool](http://aka.ms/asr-capacity-planner-excel) to analyze your source environment and workloads, estimate bandwidth needs and server resources for the source location, and the resources (virtual machines and storage etc), that you need in the target location.</span></span> <span data-ttu-id="c41ce-115">Nástroj můžete spustit v několika režimy:</span><span class="sxs-lookup"><span data-stu-id="c41ce-115">You can run the tool in a couple of modes:</span></span>

- <span data-ttu-id="c41ce-116">Rychlé plánování: spuštění nástroje v tomto režimu získat sítě a serveru projekce založené na průměrném počtu virtuálních počítačů, disků, úložiště a míru změn.</span><span class="sxs-lookup"><span data-stu-id="c41ce-116">Quick planning: Run the tool in this mode to get network and server projections based on an average number of VMs, disks, storage, and change rate.</span></span>
- <span data-ttu-id="c41ce-117">Podrobné plánování: Spusťte nástroj v tomto režimu a uveďte podrobné informace o každé zatížení na úrovni virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c41ce-117">Detailed planning: Run the tool in this mode, and provide details of each workload at VM level.</span></span> <span data-ttu-id="c41ce-118">Analýza kompatibility virtuálních počítačů a získat projekce sítě a serveru.</span><span class="sxs-lookup"><span data-stu-id="c41ce-118">Analyze VM compatibility and get network and server projections.</span></span>

<span data-ttu-id="c41ce-119">Nyní spusťte nástroj:</span><span class="sxs-lookup"><span data-stu-id="c41ce-119">Now run the tool:</span></span>

1. <span data-ttu-id="c41ce-120">Stažení [nástroj](http://aka.ms/asr-capacity-planner-excel)</span><span class="sxs-lookup"><span data-stu-id="c41ce-120">Download the [tool](http://aka.ms/asr-capacity-planner-excel)</span></span>
2. <span data-ttu-id="c41ce-121">Chcete-li spustit Rychlý planner, postupujte podle [tyto pokyny](site-recovery-capacity-planner.md#run-the-quick-planner)a vyberte scénáři **technologie Hyper-V do Azure**.</span><span class="sxs-lookup"><span data-stu-id="c41ce-121">To run the quick planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-quick-planner), and select the scenario **Hyper-V to Azure**.</span></span>
3. <span data-ttu-id="c41ce-122">Chcete-li spustit podrobné planner, postupujte podle [tyto pokyny](site-recovery-capacity-planner.md#run-the-detailed-planner)a vyberte scénáři **technologie Hyper-V do Azure**.</span><span class="sxs-lookup"><span data-stu-id="c41ce-122">To run the detailed planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-detailed-planner), and select the scenario **Hyper-V to Azure**.</span></span>

## <a name="control-network-bandwidth"></a><span data-ttu-id="c41ce-123">Řídí šířku pásma sítě</span><span class="sxs-lookup"><span data-stu-id="c41ce-123">Control network bandwidth</span></span>

<span data-ttu-id="c41ce-124">Poté, co jste vypočítat šířku pásma, které potřebujete, máte několik možností pro řízení šířky pásma sítě používané pro replikaci:</span><span class="sxs-lookup"><span data-stu-id="c41ce-124">After you've calculated the bandwidth you need, you have a couple of options for controlling the amount of bandwidth used for replication:</span></span>

* <span data-ttu-id="c41ce-125">**Omezení šířky pásma**: přenos Hyper-V, která se replikují do Azure prochází konkrétního hostitele technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="c41ce-125">**Throttle bandwidth**: Hyper-V traffic that replicates to Azure goes through a specific Hyper-V host.</span></span> <span data-ttu-id="c41ce-126">Šířku pásma na hostitelském serveru můžete omezit.</span><span class="sxs-lookup"><span data-stu-id="c41ce-126">You can throttle bandwidth on the host server.</span></span>
* <span data-ttu-id="c41ce-127">**Ovlivnění šířky pásma**: můžete ovlivnit šířku pásma používanou pro replikaci pomocí několika klíčů registru.</span><span class="sxs-lookup"><span data-stu-id="c41ce-127">**Influence bandwidth**: You can influence the bandwidth used for replication using a couple of registry keys.</span></span>

### <a name="throttle-bandwidth"></a><span data-ttu-id="c41ce-128">Omezení šířky pásma</span><span class="sxs-lookup"><span data-stu-id="c41ce-128">Throttle bandwidth</span></span>
1. <span data-ttu-id="c41ce-129">Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostitelském serveru Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="c41ce-129">Open the Microsoft Azure Backup MMC snap-in on the Hyper-V host server.</span></span> <span data-ttu-id="c41ce-130">Ve výchozím nastavení je na ploše nebo v umístění C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="c41ce-130">By default a shortcut for Microsoft Azure Backup is available on the desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="c41ce-131">V modulu snap-in klikněte na **Změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c41ce-131">In the snap-in click **Change Properties**.</span></span>
3. <span data-ttu-id="c41ce-132">Na kartě **Omezování** vyberte **Povolit omezování šířky pásma internetu u operací zálohování** a nastavte limity pro pracovní a nepracovní dobu.</span><span class="sxs-lookup"><span data-stu-id="c41ce-132">On the **Throttling** tab select **Enable internet bandwidth usage throttling for backup operations**, and set the limits for work and non-work hours.</span></span> <span data-ttu-id="c41ce-133">Platné rozsahy jsou 512 kB/s až 102 MB/s.</span><span class="sxs-lookup"><span data-stu-id="c41ce-133">Valid ranges are from 512 Kbps to 102 Mbps per second.</span></span>

    ![Omezení šířky pásma](./media/hyper-v-site-walkthrough-capacity/throttle2.png)

<span data-ttu-id="c41ce-135">Pro nastavení omezování můžete také použít rutinu [Set OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx).</span><span class="sxs-lookup"><span data-stu-id="c41ce-135">You can also use the [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet to set throttling.</span></span> <span data-ttu-id="c41ce-136">Tady je ukázkový skript:</span><span class="sxs-lookup"><span data-stu-id="c41ce-136">Here's a sample:</span></span>

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

<span data-ttu-id="c41ce-137">**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.</span><span class="sxs-lookup"><span data-stu-id="c41ce-137">**Set-OBMachineSetting -NoThrottle** indicates that no throttling is required.</span></span>

### <a name="influence-network-bandwidth"></a><span data-ttu-id="c41ce-138">Ovlivnění šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="c41ce-138">Influence network bandwidth</span></span>
1. <span data-ttu-id="c41ce-139">V registru přejděte na **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span><span class="sxs-lookup"><span data-stu-id="c41ce-139">In the registry navigate to **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span></span>
   * <span data-ttu-id="c41ce-140">K ovlivnění šířky pásma provoz na replikaci disku, změňte hodnotu **UploadThreadsPerVM**, nebo vytvořte klíč, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c41ce-140">To influence the bandwidth traffic on a replicating disk, modify the value the **UploadThreadsPerVM**, or create the key if it doesn't exist.</span></span>
   * <span data-ttu-id="c41ce-141">K ovlivnění šířky pásma pro přenosy navrácení služeb po obnovení z Azure, změňte hodnotu **DownloadThreadsPerVM**.</span><span class="sxs-lookup"><span data-stu-id="c41ce-141">To influence the bandwidth for failback traffic from Azure, modify the value **DownloadThreadsPerVM**.</span></span>
2. <span data-ttu-id="c41ce-142">Výchozí hodnota je 4.</span><span class="sxs-lookup"><span data-stu-id="c41ce-142">The default value is 4.</span></span> <span data-ttu-id="c41ce-143">V síti s „nadměrným zřízením“ je třeba tyto klíče registru změnit z výchozích hodnot.</span><span class="sxs-lookup"><span data-stu-id="c41ce-143">In an “overprovisioned” network, these registry keys should be changed from the default values.</span></span> <span data-ttu-id="c41ce-144">Maximum je 32.</span><span class="sxs-lookup"><span data-stu-id="c41ce-144">The maximum is 32.</span></span> <span data-ttu-id="c41ce-145">Monitorováním provozu hodnotu optimalizujte.</span><span class="sxs-lookup"><span data-stu-id="c41ce-145">Monitor traffic to optimize the value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c41ce-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c41ce-146">Next steps</span></span>

<span data-ttu-id="c41ce-147">Přejděte na [krok 4: plánování sítě](hyper-v-site-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="c41ce-147">Go to [Step 4: Plan networking](hyper-v-site-walkthrough-network.md).</span></span>
