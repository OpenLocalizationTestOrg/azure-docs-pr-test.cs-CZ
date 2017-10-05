---
title: "Omezení systému řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje doporučená velikost pro připojení a součásti řady StorSimple 8000 a omezení systému."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/28/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cc3c0ad193af7625c8c4c1c2e82b6bdc8be33310
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-are-storsimple-8000-series-system-limits"></a><span data-ttu-id="d2eb6-103">Jaká jsou omezení systému řady StorSimple 8000?</span><span class="sxs-lookup"><span data-stu-id="d2eb6-103">What are StorSimple 8000 series system limits?</span></span>

## <a name="overview"></a><span data-ttu-id="d2eb6-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d2eb6-104">Overview</span></span>

<span data-ttu-id="d2eb6-105">StorSimple poskytuje škálovatelná a flexibilní úložiště vašeho datového centra.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-105">StorSimple provides scalable and flexible storage for your datacenter.</span></span> <span data-ttu-id="d2eb6-106">Nicméně existují některá omezení, které jste měli vzít v úvahu při plánování, nasazení a provoz vašeho řešení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-106">However, there are some limits that you should keep in mind as you plan, deploy, and operate your StorSimple solution.</span></span> <span data-ttu-id="d2eb6-107">Následující tabulka popisuje tyto limity a uvádí několik doporučení, takže můžete využívat naplno vašeho řešení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-107">The following table describes these limits and provides some recommendations so that you can get the most out of your StorSimple solution.</span></span>

| <span data-ttu-id="d2eb6-108">Identifikátor omezení</span><span class="sxs-lookup"><span data-stu-id="d2eb6-108">Limit identifier</span></span> | <span data-ttu-id="d2eb6-109">Omezení</span><span class="sxs-lookup"><span data-stu-id="d2eb6-109">Limit</span></span> | <span data-ttu-id="d2eb6-110">Komentáře</span><span class="sxs-lookup"><span data-stu-id="d2eb6-110">Comments</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d2eb6-111">Maximální počet přihlašovacích údajů účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="d2eb6-111">Maximum number of storage account credentials</span></span> |<span data-ttu-id="d2eb6-112">64</span><span class="sxs-lookup"><span data-stu-id="d2eb6-112">64</span></span> | |
| <span data-ttu-id="d2eb6-113">Maximální počet kontejnery svazků</span><span class="sxs-lookup"><span data-stu-id="d2eb6-113">Maximum number of volume containers</span></span> |<span data-ttu-id="d2eb6-114">64</span><span class="sxs-lookup"><span data-stu-id="d2eb6-114">64</span></span> | |
| <span data-ttu-id="d2eb6-115">Maximální počet svazků</span><span class="sxs-lookup"><span data-stu-id="d2eb6-115">Maximum number of volumes</span></span> |<span data-ttu-id="d2eb6-116">255</span><span class="sxs-lookup"><span data-stu-id="d2eb6-116">255</span></span> | |
| <span data-ttu-id="d2eb6-117">Maximální počet místně vázaných svazků</span><span class="sxs-lookup"><span data-stu-id="d2eb6-117">Maximum number of locally pinned volumes</span></span> |<span data-ttu-id="d2eb6-118">32</span><span class="sxs-lookup"><span data-stu-id="d2eb6-118">32</span></span> | |
| <span data-ttu-id="d2eb6-119">Maximální počet plány na šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="d2eb6-119">Maximum number of schedules per bandwidth template</span></span> |<span data-ttu-id="d2eb6-120">168</span><span class="sxs-lookup"><span data-stu-id="d2eb6-120">168</span></span> |<span data-ttu-id="d2eb6-121">Plán pro každou hodinu, každý den v týdnu (24 * 7).</span><span class="sxs-lookup"><span data-stu-id="d2eb6-121">A schedule for every hour, every day of the week (24*7).</span></span> |
| <span data-ttu-id="d2eb6-122">Maximální velikost vrstvený svazek na fyzických zařízení</span><span class="sxs-lookup"><span data-stu-id="d2eb6-122">Maximum size of a tiered volume on physical devices</span></span> |<span data-ttu-id="d2eb6-123">64 TB pro 8100 a 8600</span><span class="sxs-lookup"><span data-stu-id="d2eb6-123">64 TB for 8100 and 8600</span></span> |<span data-ttu-id="d2eb6-124">8100 a 8600 jsou fyzické zařízení.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-124">8100 and 8600 are physical devices.</span></span> |
| <span data-ttu-id="d2eb6-125">Maximální velikost vrstvený svazek na virtuálním zařízením v Azure</span><span class="sxs-lookup"><span data-stu-id="d2eb6-125">Maximum size of a tiered volume on virtual devices in Azure</span></span> |<span data-ttu-id="d2eb6-126">30 TB pro 8010</span><span class="sxs-lookup"><span data-stu-id="d2eb6-126">30 TB for 8010</span></span> <br></br> <span data-ttu-id="d2eb6-127">64 TB pro 8020</span><span class="sxs-lookup"><span data-stu-id="d2eb6-127">64 TB for 8020</span></span> |<span data-ttu-id="d2eb6-128">8010 a 8020 jsou virtuální zařízení v Azure, které používají úložiště úrovně Standard a Premium Storage v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-128">8010 and 8020 are virtual devices in Azure that use Standard Storage and Premium Storage respectively.</span></span> |
| <span data-ttu-id="d2eb6-129">Maximální velikost místně vázaný svazek na fyzických zařízení</span><span class="sxs-lookup"><span data-stu-id="d2eb6-129">Maximum size of a locally pinned volume on physical devices</span></span> |<span data-ttu-id="d2eb6-130">Šířka 8,5 TB pro 8100</span><span class="sxs-lookup"><span data-stu-id="d2eb6-130">8.5 TB for 8100</span></span> <br></br> <span data-ttu-id="d2eb6-131">22,5 TB pro 8600</span><span class="sxs-lookup"><span data-stu-id="d2eb6-131">22.5 TB for 8600</span></span> |<span data-ttu-id="d2eb6-132">8100 a 8600 jsou fyzické zařízení.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-132">8100 and 8600 are physical devices.</span></span> |
| <span data-ttu-id="d2eb6-133">Maximální počet připojení iSCSI</span><span class="sxs-lookup"><span data-stu-id="d2eb6-133">Maximum number of iSCSI connections</span></span> |<span data-ttu-id="d2eb6-134">512</span><span class="sxs-lookup"><span data-stu-id="d2eb6-134">512</span></span> | |
| <span data-ttu-id="d2eb6-135">Maximální počet připojení iniciátorů iSCSI</span><span class="sxs-lookup"><span data-stu-id="d2eb6-135">Maximum number of iSCSI connections from initiators</span></span> |<span data-ttu-id="d2eb6-136">512</span><span class="sxs-lookup"><span data-stu-id="d2eb6-136">512</span></span> | |
| <span data-ttu-id="d2eb6-137">Maximální počet záznamů řízení přístupu podle zařízení</span><span class="sxs-lookup"><span data-stu-id="d2eb6-137">Maximum number of access control records per device</span></span> |<span data-ttu-id="d2eb6-138">64</span><span class="sxs-lookup"><span data-stu-id="d2eb6-138">64</span></span> | |
| <span data-ttu-id="d2eb6-139">Maximální počet svazků na zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="d2eb6-139">Maximum number of volumes per backup policy</span></span> |<span data-ttu-id="d2eb6-140">20</span><span class="sxs-lookup"><span data-stu-id="d2eb6-140">20</span></span> | |
| <span data-ttu-id="d2eb6-141">Maximální počet záloh uchovávány v plánu (v zásadu zálohování)</span><span class="sxs-lookup"><span data-stu-id="d2eb6-141">Maximum number of backups retained per schedule (in a backup policy)</span></span> |<span data-ttu-id="d2eb6-142">64</span><span class="sxs-lookup"><span data-stu-id="d2eb6-142">64</span></span> | |
| <span data-ttu-id="d2eb6-143">Maximální počet plány na zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="d2eb6-143">Maximum number of schedules per backup policy</span></span> |<span data-ttu-id="d2eb6-144">10</span><span class="sxs-lookup"><span data-stu-id="d2eb6-144">10</span></span> | |
| <span data-ttu-id="d2eb6-145">Maximální počet snímků žádný typ, který uchovávání může být na svazku</span><span class="sxs-lookup"><span data-stu-id="d2eb6-145">Maximum number of snapshots of any type that can be retained per volume</span></span> |<span data-ttu-id="d2eb6-146">256</span><span class="sxs-lookup"><span data-stu-id="d2eb6-146">256</span></span> |<span data-ttu-id="d2eb6-147">Tato hodnota zahrnuje místních snímků a cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-147">This number includes local snapshots and cloud snapshots.</span></span> |
| <span data-ttu-id="d2eb6-148">Maximální počet snímků, které můžou být v libovolném zařízení</span><span class="sxs-lookup"><span data-stu-id="d2eb6-148">Maximum number of snapshots that can be present in any device</span></span> |<span data-ttu-id="d2eb6-149">10 000</span><span class="sxs-lookup"><span data-stu-id="d2eb6-149">10,000</span></span> | |
| <span data-ttu-id="d2eb6-150">Maximální počet svazků, které lze zpracovat paralelní zálohování, obnovení nebo klonování</span><span class="sxs-lookup"><span data-stu-id="d2eb6-150">Maximum number of volumes that can be processed in parallel for backup, restore, or clone</span></span> |<span data-ttu-id="d2eb6-151">16</span><span class="sxs-lookup"><span data-stu-id="d2eb6-151">16</span></span> |<ul><li><span data-ttu-id="d2eb6-152">Pokud existuje víc než 16 svazky, jsou zpracovávány postupně, jakmile sloty zpracování k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-152">If there are more than 16 volumes, they are processed sequentially as processing slots become available.</span></span></li><li><span data-ttu-id="d2eb6-153">Nové zálohy klonovaný nebo obnovený vrstvený svazek nemůže proběhnout, dokud je operace dokončena.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-153">New backups of a cloned or a restored tiered volume cannot occur until the operation is finished.</span></span> <span data-ttu-id="d2eb6-154">Pro místní svazek, jsou však zálohy povoleny, po svazek je online.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-154">However, for a local volume, backups are allowed after the volume is online.</span></span></li></ul> |
| <span data-ttu-id="d2eb6-155">Obnovení a klonování obnovit dobu vrstvené svazky</span><span class="sxs-lookup"><span data-stu-id="d2eb6-155">Restore and clone recover time for tiered volumes</span></span> |<span data-ttu-id="d2eb6-156">< 2 minuty</span><span class="sxs-lookup"><span data-stu-id="d2eb6-156">< 2 minutes</span></span> |<ul><li><span data-ttu-id="d2eb6-157">Svazek je k dispozici během 2 minut operaci obnovení nebo klonování, bez ohledu na velikost svazku.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-157">The volume is made available within 2 minutes of restore or clone operation, regardless of the volume size.</span></span></li><li><span data-ttu-id="d2eb6-158">Výkon svazku může zpočátku pomalejší než běžné jako většinu dat a metadat se stále nachází v cloudu.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-158">The volume performance may initially be slower than normal as most of the data and metadata still resides in the cloud.</span></span> <span data-ttu-id="d2eb6-159">Jako toky dat z cloudu do zařízení StorSimple může zvýšit výkon.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-159">Performance may increase as data flows from the cloud to the StorSimple device.</span></span></li><li><span data-ttu-id="d2eb6-160">Celkový čas ke stažení metadat, závisí na velikosti přiděleného svazku.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-160">The total time to download metadata depends on the allocated volume size.</span></span> <span data-ttu-id="d2eb6-161">Metadata se automaticky přenesou do zařízení na pozadí ve výši 5 minut za TB dat přidělené svazku.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-161">Metadata is automatically brought into the device in the background at the rate of 5 minutes per TB of allocated volume data.</span></span> <span data-ttu-id="d2eb6-162">Tato míra může mít vliv šířky pásma Internetu do cloudu.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-162">This rate may be affected by Internet bandwidth to the cloud.</span></span></li><li><span data-ttu-id="d2eb6-163">Obnovení nebo operaci klonování je dokončena po všechna metadata v zařízení.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-163">The restore or clone operation is complete when all the metadata is on the device.</span></span></li><li><span data-ttu-id="d2eb6-164">Operace zálohování nelze provést, dokud nebude obnovení nebo je plně dokončení operace klonování.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-164">Backup operations cannot be performed until the restore or clone operation is fully complete.</span></span> |
| <span data-ttu-id="d2eb6-165">Obnovení obnovit dobu místně vázaných svazků</span><span class="sxs-lookup"><span data-stu-id="d2eb6-165">Restore recover time for locally pinned volumes</span></span> |<span data-ttu-id="d2eb6-166">< 2 minuty</span><span class="sxs-lookup"><span data-stu-id="d2eb6-166">< 2 minutes</span></span> |<ul><li><span data-ttu-id="d2eb6-167">Svazek je k dispozici během 2 minut operaci obnovení, bez ohledu na velikost svazku.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-167">The volume is made available within 2 minutes of the restore operation, regardless of the volume size.</span></span></li><li><span data-ttu-id="d2eb6-168">Výkon svazku může zpočátku pomalejší než běžné jako většinu dat a metadat se stále nachází v cloudu.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-168">The volume performance may initially be slower than normal as most of the data and metadata still resides in the cloud.</span></span> <span data-ttu-id="d2eb6-169">Jako toky dat z cloudu do zařízení StorSimple může zvýšit výkon.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-169">Performance may increase as data flows from the cloud to the StorSimple device.</span></span></li><li><span data-ttu-id="d2eb6-170">Celkový čas ke stažení metadat, závisí na velikosti přiděleného svazku.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-170">The total time to download metadata depends on the allocated volume size.</span></span> <span data-ttu-id="d2eb6-171">Metadata se automaticky přenesou do zařízení na pozadí ve výši 5 minut za TB dat přidělené svazku.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-171">Metadata is automatically brought into the device in the background at the rate of 5 minutes per TB of allocated volume data.</span></span> <span data-ttu-id="d2eb6-172">Tato míra může mít vliv šířky pásma Internetu do cloudu.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-172">This rate may be affected by Internet bandwidth to the cloud.</span></span></li><li><span data-ttu-id="d2eb6-173">Na rozdíl od vrstvené svazky místně vázaných svazků data na svazku je také stažen místně na zařízení.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-173">Unlike tiered volumes, for locally pinned volumes, the volume data is also downloaded locally on the device.</span></span> <span data-ttu-id="d2eb6-174">Operace obnovení byla dokončena, pokud všechna data svazek znovu do zařízení.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-174">The restore operation is complete when all the volume data has been brought to the device.</span></span></li><li><span data-ttu-id="d2eb6-175">Operace obnovení může trvat dlouho.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-175">The restore operations may be long.</span></span> <span data-ttu-id="d2eb6-176">Celkový čas na dokončení obnovení závisí na velikosti zřízené místní svazek, pásma vašeho internetového připojení a existující data v zařízení.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-176">The total time to complete the restore depends on the size of the provisioned local volume, your Internet bandwidth, and the existing data on the device.</span></span> <span data-ttu-id="d2eb6-177">Operace zálohování na místně vázaný svazek jsou povoleny, když probíhá operace obnovení.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-177">Backup operations on the locally pinned volume are allowed while the restore operation is in progress.</span></span> |
| <span data-ttu-id="d2eb6-178">Rychlost zpracování pro cloudové snímky</span><span class="sxs-lookup"><span data-stu-id="d2eb6-178">Processing rate for cloud snapshots</span></span> |<span data-ttu-id="d2eb6-179">15 minut nebo TB</span><span class="sxs-lookup"><span data-stu-id="d2eb6-179">15 minutes/TB</span></span> |<ul><li><span data-ttu-id="d2eb6-180">Minimální hodnota času aby cloudu snímku připravené k odeslání za TB dat přidělené svazku v zálohování.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-180">Minimum time to make the cloud snapshot ready for upload, per TB of allocated volume data in backup.</span></span> </li><li> <span data-ttu-id="d2eb6-181">Celkový počet cloudových snímků čas se počítá přidáním této doby čas nahrávání snímku, která obsahuje šířky pásma Internetu do cloudu.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-181">Total cloud snapshot time is calculated by adding this time to the snapshot upload time, which is affected by the Internet bandwidth to cloud.</span></span> |
| <span data-ttu-id="d2eb6-182">Propustnost pro čtení a zápis maximálního počtu klientů (Pokud obsluhovat z vrstvy SSD) *</span><span class="sxs-lookup"><span data-stu-id="d2eb6-182">Maximum client read/write throughput (when served from the SSD tier)*</span></span> |<span data-ttu-id="d2eb6-183">920/720 MB/s jeden 10 GbE síťovým rozhraním</span><span class="sxs-lookup"><span data-stu-id="d2eb6-183">920/720 MB/s with a single 10 GbE network interface</span></span> |<span data-ttu-id="d2eb6-184">Až 2 x s funkci MPIO a dvě síťová rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-184">Up to 2x with MPIO and two network interfaces.</span></span> |
| <span data-ttu-id="d2eb6-185">Propustnost pro čtení a zápis maximálního počtu klientů (Pokud obsluhovat z vrstvy HDD) *</span><span class="sxs-lookup"><span data-stu-id="d2eb6-185">Maximum client read/write throughput (when served from the HDD tier)*</span></span> |<span data-ttu-id="d2eb6-186">120/250 MB/s</span><span class="sxs-lookup"><span data-stu-id="d2eb6-186">120/250 MB/s</span></span> | |
| <span data-ttu-id="d2eb6-187">Propustnost pro čtení a zápis maximálního počtu klientů (Pokud obsluhovat z vrstvy cloudu) * pro Update 3 a vyšší **</span><span class="sxs-lookup"><span data-stu-id="d2eb6-187">Maximum client read/write throughput (when served from the cloud tier)* for Update 3 and later**</span></span> |<span data-ttu-id="d2eb6-188">40/60 MB/s pro vrstvené svazky</span><span class="sxs-lookup"><span data-stu-id="d2eb6-188">40/60 MB/s for tiered volumes</span></span><br><br><span data-ttu-id="d2eb6-189">60/80 MB/s pro vrstvené svazky s archivace zvolené během vytvoření svazku</span><span class="sxs-lookup"><span data-stu-id="d2eb6-189">60/80 MB/s for tiered volumes with archival option selected during volume creation</span></span> |<span data-ttu-id="d2eb6-190">Propustnost čtení závisí na klientech generování a údržbu dostatečná hloubku fronty vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-190">Read throughput depends on clients generating and maintaining sufficient I/O queue depth.</span></span> <br><br><span data-ttu-id="d2eb6-191">Rychlost dosáhnout závisí na rychlosti základní účtu úložiště používat.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-191">Speed achieved depends on the speed of the underlying storage account used.</span></span> |

<span data-ttu-id="d2eb6-192">&#42; Maximální propustnost podle typu vstupně-výstupních operací se měří s 100 procent čtení a zápisu 100 procent scénáře.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-192">&#42; Maximum throughput per I/O type was measured with 100 percent read and 100 percent write scenarios.</span></span> <span data-ttu-id="d2eb6-193">Skutečná propustnost může být nižší a závisí na vstupně-výstupních operací kombinovat a síťové podmínky.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-193">Actual throughput may be lower and depends on I/O mix and network conditions.</span></span>

<span data-ttu-id="d2eb6-194">&#42; &#42; Výkon čísla před Update 3 může být nižší.</span><span class="sxs-lookup"><span data-stu-id="d2eb6-194">&#42;&#42; Performance numbers prior to Update 3 may be lower.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2eb6-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2eb6-195">Next steps</span></span>
<span data-ttu-id="d2eb6-196">Zkontrolujte [požadavky na systém StorSimple](storsimple-8000-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="d2eb6-196">Review the [StorSimple system requirements](storsimple-8000-system-requirements.md).</span></span>
