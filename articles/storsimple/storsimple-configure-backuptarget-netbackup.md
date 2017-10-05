---
title: "Řady StorSimple 8000 jako cíl zálohování s NetBackup | Microsoft Docs"
description: "Popisuje konfiguraci cíl zálohy zařízení StorSimple s NetBackup této společnosti."
services: storsimple
documentationcenter: 
author: harshakirank
manager: matd
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: hkanna
ms.openlocfilehash: c9b3a259f9bc3e0561c7ba94e91edf7c8e0deabb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-as-a-backup-target-with-netbackup"></a><span data-ttu-id="60438-103">StorSimple jako cíl zálohování s NetBackup</span><span class="sxs-lookup"><span data-stu-id="60438-103">StorSimple as a backup target with NetBackup</span></span>

## <a name="overview"></a><span data-ttu-id="60438-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="60438-104">Overview</span></span>

<span data-ttu-id="60438-105">Azure StorSimple je řešením hybridní cloudové úložiště od společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="60438-105">Azure StorSimple is a hybrid cloud storage solution from Microsoft.</span></span> <span data-ttu-id="60438-106">StorSimple řeší složitosti nárůst exponenciální dat pomocí účtu úložiště Azure jako rozšíření nástroje v případě místních řešení a automaticky vrstvení dat mezi místní úložiště a úložiště v cloudu.</span><span class="sxs-lookup"><span data-stu-id="60438-106">StorSimple addresses the complexities of exponential data growth by using an Azure storage account as an extension of the on-premises solution, and automatically tiering data across on-premises storage and cloud storage.</span></span>

<span data-ttu-id="60438-107">V tomto článku probereme StorSimple integrace s NetBackup této společnosti a osvědčené postupy pro integraci obě řešení.</span><span class="sxs-lookup"><span data-stu-id="60438-107">In this article, we discuss StorSimple integration with Veritas NetBackup, and best practices for integrating both solutions.</span></span> <span data-ttu-id="60438-108">Také jsme zkontrolujte doporučení o tom, jak nastavit této společnosti NetBackup nejlépe integraci s StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-108">We also make recommendations on how to set up Veritas NetBackup to best integrate with StorSimple.</span></span> <span data-ttu-id="60438-109">Odložení jsme do této společnosti osvědčené postupy, zálohování architekty a správce pro nejlepší způsob, jak nastavit této společnosti NetBackup splňovat požadavky na jednotlivé zálohování a smlouvy o úrovni služeb (SLA).</span><span class="sxs-lookup"><span data-stu-id="60438-109">We defer to Veritas best practices, backup architects, and administrators for the best way to set up Veritas NetBackup to meet individual backup requirements and service-level agreements (SLAs).</span></span>

<span data-ttu-id="60438-110">I když jsme ilustrují kroky konfigurace a klíčové koncepty, tento článek je rozhodně není to podrobný Průvodce konfigurace nebo instalace.</span><span class="sxs-lookup"><span data-stu-id="60438-110">Although we illustrate configuration steps and key concepts, this article is by no means a step-by-step configuration or installation guide.</span></span> <span data-ttu-id="60438-111">Předpokládáme, že základní součásti a infrastrukturu správně funguje a připravené k podpoře koncepty, které jsme popisují.</span><span class="sxs-lookup"><span data-stu-id="60438-111">We assume that the basic components and infrastructure are in working order and ready to support the concepts that we describe.</span></span>

### <a name="who-should-read-this"></a><span data-ttu-id="60438-112">Komu je určen to?</span><span class="sxs-lookup"><span data-stu-id="60438-112">Who should read this?</span></span>

<span data-ttu-id="60438-113">Informace v tomto článku bude nejužitečnější správců zálohování, správců úložiště a úložiště architektům, kteří mají znalosti o úložiště, Windows Server 2012 R2, Ethernet, cloudové služby a NetBackup této společnosti.</span><span class="sxs-lookup"><span data-stu-id="60438-113">The information in this article will be most helpful to backup administrators, storage administrators, and storage architects who have knowledge of storage, Windows Server 2012 R2, Ethernet, cloud services, and Veritas NetBackup.</span></span>

### <a name="supported-versions"></a><span data-ttu-id="60438-114">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="60438-114">Supported versions</span></span>

-   <span data-ttu-id="60438-115">NetBackup 7.7.x a novější verze</span><span class="sxs-lookup"><span data-stu-id="60438-115">NetBackup 7.7.x and later versions</span></span>
-   [<span data-ttu-id="60438-116">StorSimple Update 3 a novějších verzích</span><span class="sxs-lookup"><span data-stu-id="60438-116">StorSimple Update 3 and later versions</span></span>](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a><span data-ttu-id="60438-117">Proč StorSimple jako cíl zálohování?</span><span class="sxs-lookup"><span data-stu-id="60438-117">Why StorSimple as a backup target?</span></span>

<span data-ttu-id="60438-118">StorSimple je vhodná pro cíl zálohy, protože:</span><span class="sxs-lookup"><span data-stu-id="60438-118">StorSimple is a good choice for a backup target because:</span></span>

-   <span data-ttu-id="60438-119">Poskytuje standardní, místní úložiště pro zálohování aplikace, které chcete použít jako cíl pro rychlé zálohování, beze změn.</span><span class="sxs-lookup"><span data-stu-id="60438-119">It provides standard, local storage for backup applications to use as a fast backup destination, without any changes.</span></span> <span data-ttu-id="60438-120">Můžete taky použít StorSimple pro rychlé obnovení poslední zálohy.</span><span class="sxs-lookup"><span data-stu-id="60438-120">You also can use StorSimple for a quick restore of recent backups.</span></span>
-   <span data-ttu-id="60438-121">Jeho cloudu vrstvení je bezproblémově integrovaná s účtem úložiště Azure cloud používání nákladově efektivní úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="60438-121">Its cloud tiering is seamlessly integrated with an Azure cloud storage account to use cost-effective Azure Storage.</span></span>
-   <span data-ttu-id="60438-122">Automaticky poskytuje úložiště mimo pracoviště pro zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="60438-122">It automatically provides offsite storage for disaster recovery.</span></span>

## <a name="key-concepts"></a><span data-ttu-id="60438-123">Klíčové koncepty</span><span class="sxs-lookup"><span data-stu-id="60438-123">Key concepts</span></span>

<span data-ttu-id="60438-124">Stejně jako u jakékoli řešení úložiště, postupujte opatrně hodnocení výkonu úložiště na řešení, SLA, je důležité na úspěšné provedení míra změn a požadavků na kapacitu růstu.</span><span class="sxs-lookup"><span data-stu-id="60438-124">As with any storage solution, a careful assessment of the solution’s storage performance, SLAs, rate of change, and capacity growth needs is critical to success.</span></span> <span data-ttu-id="60438-125">Main nápad je, že díky zavedení cloudovou vrstvu, čas přístupu a propustnosti na cloudu play roli základní možnosti StorSimple svou úlohu.</span><span class="sxs-lookup"><span data-stu-id="60438-125">The main idea is that by introducing a cloud tier, your access times and throughputs to the cloud play a fundamental role in the ability of StorSimple to do its job.</span></span>

<span data-ttu-id="60438-126">StorSimple slouží k poskytování úložiště pro aplikace, které fungují na dobře definovaný pracovní sady dat (aktivní data).</span><span class="sxs-lookup"><span data-stu-id="60438-126">StorSimple is designed to provide storage to applications that operate on a well-defined working set of data (hot data).</span></span> <span data-ttu-id="60438-127">V tomto modelu pracovní sady dat je uložena v místních vrstvách a vrstveny zbývající nepracovní, uloží málo používaná data nebo archivovat sadu dat do cloudu.</span><span class="sxs-lookup"><span data-stu-id="60438-127">In this model, the working set of data is stored on the local tiers, and the remaining nonworking/cold/archived set of data is tiered to the cloud.</span></span> <span data-ttu-id="60438-128">Tento model je znázorněná na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="60438-128">This model is represented in the following figure.</span></span> <span data-ttu-id="60438-129">Téměř nestrukturované zelená řádek představuje data uložená v místních vrstvách zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-129">The nearly flat green line represents the data stored on the local tiers of the StorSimple device.</span></span> <span data-ttu-id="60438-130">Červená čára představuje celkovou velikost dat uložených v řešení StorSimple napříč všechny úrovně.</span><span class="sxs-lookup"><span data-stu-id="60438-130">The red line represents the total amount of data stored on the StorSimple solution across all tiers.</span></span> <span data-ttu-id="60438-131">Mezera mezi nestrukturované zelená řádku a exponenciální red křivky představuje celkové množství dat uložených v cloudu.</span><span class="sxs-lookup"><span data-stu-id="60438-131">The space between the flat green line and the exponential red curve represents the total amount of data stored in the cloud.</span></span>

<span data-ttu-id="60438-132">**Vrstvení StorSimple**
![vrstvení diagram StorSimple](./media/storsimple-configure-backup-target-using-netbackup/image1.jpg)</span><span class="sxs-lookup"><span data-stu-id="60438-132">**StorSimple tiering**
![StorSimple tiering diagram](./media/storsimple-configure-backup-target-using-netbackup/image1.jpg)</span></span>

<span data-ttu-id="60438-133">Tato architektura pamatovat zjistíte, že zařízení StorSimple je výborně fungují jako cíl zálohy.</span><span class="sxs-lookup"><span data-stu-id="60438-133">With this architecture in mind, you will find that StorSimple is ideally suited to operate as a backup target.</span></span> <span data-ttu-id="60438-134">Můžete použít StorSimple na:</span><span class="sxs-lookup"><span data-stu-id="60438-134">You can use StorSimple to:</span></span>
-   <span data-ttu-id="60438-135">Vaše nejčastěji se vyskytující obnovení proveďte z místní pracovní sadu dat.</span><span class="sxs-lookup"><span data-stu-id="60438-135">Perform your most frequent restores from the local working set of data.</span></span>
-   <span data-ttu-id="60438-136">Použijte cloudu pro obnovení po havárii mimo pracoviště a starší data, kde jsou méně časté obnovení.</span><span class="sxs-lookup"><span data-stu-id="60438-136">Use the cloud for offsite disaster recovery and older data, where restores are less frequent.</span></span>

## <a name="storsimple-benefits"></a><span data-ttu-id="60438-137">Výhody StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-137">StorSimple benefits</span></span>

<span data-ttu-id="60438-138">V případě místních řešení, která je bezproblémově integrovaná s Microsoft Azure a využívají k bezproblémový přístup k místnímu poskytuje StorSimple a cloudového úložiště.</span><span class="sxs-lookup"><span data-stu-id="60438-138">StorSimple provides an on-premises solution that is seamlessly integrated with Microsoft Azure, by taking advantage of seamless access to on-premises and cloud storage.</span></span>

<span data-ttu-id="60438-139">StorSimple používá automatické vrstvení mezi místní zařízení, která má SSD zařízení (SSD) a serial-attached SCSI (SAS), úložiště a Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="60438-139">StorSimple uses automatic tiering between the on-premises device, which has solid-state device (SSD) and serial-attached SCSI (SAS) storage, and Azure Storage.</span></span> <span data-ttu-id="60438-140">Automatické vrstvení udržuje často používaná data místní počítač, na vrstvy SSD a SAS.</span><span class="sxs-lookup"><span data-stu-id="60438-140">Automatic tiering keeps frequently accessed data local, on the SSD and SAS tiers.</span></span> <span data-ttu-id="60438-141">Přesune málokdy načítaná data do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="60438-141">It moves infrequently accessed data to Azure Storage.</span></span>

<span data-ttu-id="60438-142">StorSimple nabízí tyto výhody:</span><span class="sxs-lookup"><span data-stu-id="60438-142">StorSimple offers these benefits:</span></span>

-   <span data-ttu-id="60438-143">Jedinečný odstraňování duplicitních dat a komprese algoritmy, které použít cloudu k dosažení úrovně nebývalá odstranění duplicitních dat</span><span class="sxs-lookup"><span data-stu-id="60438-143">Unique deduplication and compression algorithms that use the cloud to achieve unprecedented deduplication levels</span></span>
-   <span data-ttu-id="60438-144">Vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="60438-144">High availability</span></span>
-   <span data-ttu-id="60438-145">Geografická replikace pomocí Azure geografická replikace</span><span class="sxs-lookup"><span data-stu-id="60438-145">Geo-replication by using Azure geo-replication</span></span>
-   <span data-ttu-id="60438-146">Integrace se službou Azure</span><span class="sxs-lookup"><span data-stu-id="60438-146">Azure integration</span></span>
-   <span data-ttu-id="60438-147">Šifrování dat v cloudu</span><span class="sxs-lookup"><span data-stu-id="60438-147">Data encryption in the cloud</span></span>
-   <span data-ttu-id="60438-148">Zotavení po havárii vylepšené a dodržování předpisů</span><span class="sxs-lookup"><span data-stu-id="60438-148">Improved disaster recovery and compliance</span></span>

<span data-ttu-id="60438-149">I když StorSimple uvede dva scénáře hlavní nasazení (primární cíl zálohy a sekundární cíl zálohy), od základu, je prostý, zařízení s blokovým úložištěm.</span><span class="sxs-lookup"><span data-stu-id="60438-149">Although StorSimple presents two main deployment scenarios (primary backup target and secondary backup target), fundamentally, it's a plain, block storage device.</span></span> <span data-ttu-id="60438-150">StorSimple nemá všechny komprese a odstranění duplicit.</span><span class="sxs-lookup"><span data-stu-id="60438-150">StorSimple does all the compression and deduplication.</span></span> <span data-ttu-id="60438-151">Bezproblémově odešle a načte data mezi cloudu a aplikace a systému souborů.</span><span class="sxs-lookup"><span data-stu-id="60438-151">It seamlessly sends and retrieves data between the cloud and the application and file system.</span></span>

<span data-ttu-id="60438-152">Další informace o zařízení StorSimple, najdete v části [řady StorSimple 8000: hybridní cloudové úložiště řešení](storsimple-overview.md).</span><span class="sxs-lookup"><span data-stu-id="60438-152">For more information about StorSimple, see [StorSimple 8000 series: Hybrid cloud storage solution](storsimple-overview.md).</span></span> <span data-ttu-id="60438-153">Navíc můžete zkontrolovat [technických specifikací řady StorSimple 8000](storsimple-technical-specifications-and-compliance.md).</span><span class="sxs-lookup"><span data-stu-id="60438-153">Also, you can review the [technical StorSimple 8000 series specifications](storsimple-technical-specifications-and-compliance.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60438-154">Pomocí StorSimple zařízení jako cíl zálohování je podporováno pouze pro zařízení StorSimple 8000 Update 3 a novějších verzích.</span><span class="sxs-lookup"><span data-stu-id="60438-154">Using a StorSimple device as a backup target is supported only for StorSimple 8000 Update 3 and later versions.</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="60438-155">Přehled architektury</span><span class="sxs-lookup"><span data-stu-id="60438-155">Architecture overview</span></span>

<span data-ttu-id="60438-156">Následující tabulky popisují počáteční pokyny modelu architektura zařízení.</span><span class="sxs-lookup"><span data-stu-id="60438-156">The following tables show the device model-to-architecture initial guidance.</span></span>

<span data-ttu-id="60438-157">**StorSimple kapacity pro místní a cloudové úložiště**</span><span class="sxs-lookup"><span data-stu-id="60438-157">**StorSimple capacities for local and cloud storage**</span></span>

| <span data-ttu-id="60438-158">Kapacita úložiště</span><span class="sxs-lookup"><span data-stu-id="60438-158">Storage capacity</span></span>       | <span data-ttu-id="60438-159">8100</span><span class="sxs-lookup"><span data-stu-id="60438-159">8100</span></span>          | <span data-ttu-id="60438-160">8600</span><span class="sxs-lookup"><span data-stu-id="60438-160">8600</span></span>            |
|------------------------|---------------|-----------------|
| <span data-ttu-id="60438-161">Místní úložiště kapacity</span><span class="sxs-lookup"><span data-stu-id="60438-161">Local storage capacity</span></span> | <span data-ttu-id="60438-162">&lt;10 TiB\*</span><span class="sxs-lookup"><span data-stu-id="60438-162">&lt; 10 TiB\*</span></span>  | <span data-ttu-id="60438-163">&lt;20 TiB\*</span><span class="sxs-lookup"><span data-stu-id="60438-163">&lt; 20 TiB\*</span></span>  |
| <span data-ttu-id="60438-164">Kapacita cloudového úložiště</span><span class="sxs-lookup"><span data-stu-id="60438-164">Cloud storage capacity</span></span> | <span data-ttu-id="60438-165">&gt;200 TiB\*</span><span class="sxs-lookup"><span data-stu-id="60438-165">&gt; 200 TiB\*</span></span> | <span data-ttu-id="60438-166">&gt;500 TiB\*</span><span class="sxs-lookup"><span data-stu-id="60438-166">&gt; 500 TiB\*</span></span> |
<span data-ttu-id="60438-167">\*Velikost úložiště předpokládá bez odstranění duplicitních dat nebo kompresi.</span><span class="sxs-lookup"><span data-stu-id="60438-167">\* Storage size assumes no deduplication or compression.</span></span>

<span data-ttu-id="60438-168">**StorSimple kapacity pro primární a sekundární zálohování**</span><span class="sxs-lookup"><span data-stu-id="60438-168">**StorSimple capacities for primary and secondary backups**</span></span>

| <span data-ttu-id="60438-169">Zálohování scénář</span><span class="sxs-lookup"><span data-stu-id="60438-169">Backup scenario</span></span>  | <span data-ttu-id="60438-170">Místní úložiště kapacity</span><span class="sxs-lookup"><span data-stu-id="60438-170">Local storage capacity</span></span>  | <span data-ttu-id="60438-171">Kapacita cloudového úložiště</span><span class="sxs-lookup"><span data-stu-id="60438-171">Cloud storage capacity</span></span>  |
|---|---|---|
| <span data-ttu-id="60438-172">Primární zálohu</span><span class="sxs-lookup"><span data-stu-id="60438-172">Primary backup</span></span>  | <span data-ttu-id="60438-173">Nedávné zálohy uložený v místním úložišti pro rychlé obnovení ke splnění plánovaného bodu obnovení (RPO)</span><span class="sxs-lookup"><span data-stu-id="60438-173">Recent backups stored on local storage for fast recovery to meet recovery point objective (RPO)</span></span> | <span data-ttu-id="60438-174">Historie zálohování (RPO) se vejde kapacity cloudu</span><span class="sxs-lookup"><span data-stu-id="60438-174">Backup history (RPO) fits in cloud capacity</span></span> |
| <span data-ttu-id="60438-175">Sekundární zálohování</span><span class="sxs-lookup"><span data-stu-id="60438-175">Secondary backup</span></span> | <span data-ttu-id="60438-176">Sekundární kopii zálohovaných dat mohou být uloženy ve kapacity cloudu</span><span class="sxs-lookup"><span data-stu-id="60438-176">Secondary copy of backup data can be stored in cloud capacity</span></span>  | <span data-ttu-id="60438-177">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="60438-177">N/A</span></span>  |

## <a name="storsimple-as-a-primary-backup-target"></a><span data-ttu-id="60438-178">StorSimple jako primární cíl zálohování</span><span class="sxs-lookup"><span data-stu-id="60438-178">StorSimple as a primary backup target</span></span>

<span data-ttu-id="60438-179">V tomto scénáři jsou pro aplikaci Zálohování, která jako jedinou úložiště pro zálohy zobrazí svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-179">In this scenario, StorSimple volumes are presented to the backup application as the sole repository for backups.</span></span> <span data-ttu-id="60438-180">Následující obrázek znázorňuje architekturu řešení, ve kterém použijte všechny zálohy zařízení StorSimple zřízeny vrstvené svazky pro zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="60438-180">The following figure shows a solution architecture in which all backups use StorSimple tiered volumes for backups and restores.</span></span>

![StorSimple jako primární cíl zálohy logického diagramu](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a><span data-ttu-id="60438-182">Primární cíl zálohování logických kroků.</span><span class="sxs-lookup"><span data-stu-id="60438-182">Primary target backup logical steps</span></span>

1.  <span data-ttu-id="60438-183">Zálohování serveru kontaktovat agenta cíl zálohování a zálohování agent odesílá data na zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="60438-183">The backup server contacts the target backup agent, and the backup agent transmits data to the backup server.</span></span>
2.  <span data-ttu-id="60438-184">Zálohování serveru zapisuje data do StorSimple zřízeny vrstvené svazky.</span><span class="sxs-lookup"><span data-stu-id="60438-184">The backup server writes data to the StorSimple tiered volumes.</span></span>
3.  <span data-ttu-id="60438-185">Zálohování serveru aktualizuje databázi katalogu a pak dokončí úlohu zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-185">The backup server updates the catalog database, and then finishes the backup job.</span></span>
4.  <span data-ttu-id="60438-186">Skript snímku aktivuje snapshot manager zařízení StorSimple (spuštění nebo odstranění).</span><span class="sxs-lookup"><span data-stu-id="60438-186">A snapshot script triggers the StorSimple snapshot manager (start or delete).</span></span>
5.  <span data-ttu-id="60438-187">Zálohování serveru odstraní vypršela platnost záloh na základě zásad uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="60438-187">The backup server deletes expired backups based on a retention policy.</span></span>

### <a name="primary-target-restore-logical-steps"></a><span data-ttu-id="60438-188">Primární cíl obnovení logických kroků</span><span class="sxs-lookup"><span data-stu-id="60438-188">Primary target restore logical steps</span></span>

1.  <span data-ttu-id="60438-189">Zálohování serveru spustí obnovit příslušná data z úložiště úložiště.</span><span class="sxs-lookup"><span data-stu-id="60438-189">The backup server starts restoring the appropriate data from the storage repository.</span></span>
2.  <span data-ttu-id="60438-190">Agenta zálohování přijímá data ze zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="60438-190">The backup agent receives the data from the backup server.</span></span>
3.  <span data-ttu-id="60438-191">Záložní server dokončí úlohu obnovení.</span><span class="sxs-lookup"><span data-stu-id="60438-191">The backup server finishes the restore job.</span></span>

## <a name="storsimple-as-a-secondary-backup-target"></a><span data-ttu-id="60438-192">StorSimple jako sekundární cíl zálohování</span><span class="sxs-lookup"><span data-stu-id="60438-192">StorSimple as a secondary backup target</span></span>

<span data-ttu-id="60438-193">V tomto scénáři se svazky zařízení StorSimple primárně používají pro dlouhodobé uchovávání nebo archivovat.</span><span class="sxs-lookup"><span data-stu-id="60438-193">In this scenario, StorSimple volumes primarily are used for long-term retention or archiving.</span></span>

<span data-ttu-id="60438-194">Následující obrázek ukazuje architekturu, ve které prvotní zálohy a obnoví vysoce výkonné cílový svazek.</span><span class="sxs-lookup"><span data-stu-id="60438-194">The following figure shows an architecture in which initial backups and restores target a high-performance volume.</span></span> <span data-ttu-id="60438-195">Tyto zálohy jsou zkopírovány a jeho archivaci pro StorSimple vrstvené svazku podle nastaveného plánu.</span><span class="sxs-lookup"><span data-stu-id="60438-195">These backups are copied and archived to a StorSimple tiered volume on a set schedule.</span></span>

<span data-ttu-id="60438-196">Je důležité velikost svazku vysoce výkonné, tak, aby ji může zpracovat požadavky vaší uchování zásad kapacitu a výkon.</span><span class="sxs-lookup"><span data-stu-id="60438-196">It's important to size your high-performance volume so that it can handle your retention policy capacity and performance requirements.</span></span>

![StorSimple jako sekundární cíl zálohy logického diagramu](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a><span data-ttu-id="60438-198">Sekundární cíl zálohování logických kroků.</span><span class="sxs-lookup"><span data-stu-id="60438-198">Secondary target backup logical steps</span></span>

1.  <span data-ttu-id="60438-199">Zálohování serveru kontaktovat agenta cíl zálohování a zálohování agent odesílá data na zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="60438-199">The backup server contacts the target backup agent, and the backup agent transmits data to the backup server.</span></span>
2.  <span data-ttu-id="60438-200">Zálohování serveru zapisuje data do úložiště s vysokým výkonem.</span><span class="sxs-lookup"><span data-stu-id="60438-200">The backup server writes data to high-performance storage.</span></span>
3.  <span data-ttu-id="60438-201">Zálohování serveru aktualizuje databázi katalogu a pak dokončí úlohu zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-201">The backup server updates the catalog database, and then finishes the backup job.</span></span>
4.  <span data-ttu-id="60438-202">Zálohování serveru zkopíruje zálohy na StorSimple na základě zásad uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="60438-202">The backup server copies backups to StorSimple based on a retention policy.</span></span>
5.  <span data-ttu-id="60438-203">Skript snímku aktivuje snapshot manager zařízení StorSimple (spuštění nebo odstranění).</span><span class="sxs-lookup"><span data-stu-id="60438-203">A snapshot script triggers the StorSimple snapshot manager (start or delete).</span></span>
6.  <span data-ttu-id="60438-204">Zálohování serveru odstraní vypršela platnost záloh na základě zásad uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="60438-204">The backup server deletes the expired backups based on a retention policy.</span></span>

### <a name="secondary-target-restore-logical-steps"></a><span data-ttu-id="60438-205">Sekundární cíl obnovení logických kroků</span><span class="sxs-lookup"><span data-stu-id="60438-205">Secondary target restore logical steps</span></span>

1.  <span data-ttu-id="60438-206">Zálohování serveru spustí obnovit příslušná data z úložiště úložiště.</span><span class="sxs-lookup"><span data-stu-id="60438-206">The backup server starts restoring the appropriate data from the storage repository.</span></span>
2.  <span data-ttu-id="60438-207">Agenta zálohování přijímá data ze zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="60438-207">The backup agent receives the data from the backup server.</span></span>
3.  <span data-ttu-id="60438-208">Záložní server dokončí úlohu obnovení.</span><span class="sxs-lookup"><span data-stu-id="60438-208">The backup server finishes the restore job.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="60438-209">Nasazení řešení.</span><span class="sxs-lookup"><span data-stu-id="60438-209">Deploy the solution</span></span>

<span data-ttu-id="60438-210">Nasazení tohoto řešení vyžaduje tři kroky:</span><span class="sxs-lookup"><span data-stu-id="60438-210">Deploying this solution requires three steps:</span></span>
1. <span data-ttu-id="60438-211">Připravte síťové infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="60438-211">Prepare the network infrastructure.</span></span>
2. <span data-ttu-id="60438-212">Nasazení zařízení StorSimple jako cíl zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-212">Deploy your StorSimple device as a backup target.</span></span>
3. <span data-ttu-id="60438-213">Nasaďte NetBackup této společnosti.</span><span class="sxs-lookup"><span data-stu-id="60438-213">Deploy Veritas NetBackup.</span></span>

<span data-ttu-id="60438-214">Každý krok je podrobněji v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="60438-214">Each step is discussed in detail in the following sections.</span></span>

### <a name="set-up-the-network"></a><span data-ttu-id="60438-215">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="60438-215">Set up the network</span></span>

<span data-ttu-id="60438-216">Protože StorSimple je řešení, které jsou integrovány v cloudu Azure, vyžaduje StorSimple aktivní a připojení pracovní do cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="60438-216">Because StorSimple is a solution that's integrated with the Azure cloud, StorSimple requires an active and working connection to the Azure cloud.</span></span> <span data-ttu-id="60438-217">Toto připojení se používá pro operace, jako je cloudových snímků, Správa dat a přenos metadata, a do vrstvy starší, menší používaná data do cloudového úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="60438-217">This connection is used for operations like cloud snapshots, data management, and metadata transfer, and to tier older, less accessed data to Azure cloud storage.</span></span>

<span data-ttu-id="60438-218">Pro řešení pro optimální výkon doporučujeme, postupujte podle těchto sítě osvědčené postupy:</span><span class="sxs-lookup"><span data-stu-id="60438-218">For the solution to perform optimally, we recommend that you follow these networking best practices:</span></span>

-   <span data-ttu-id="60438-219">Odkaz, který se připojuje k Azure StorSimple vrstvení musí splňovat požadavky na šířku pásma.</span><span class="sxs-lookup"><span data-stu-id="60438-219">The link that connects the StorSimple tiering to Azure must meet your bandwidth requirements.</span></span> <span data-ttu-id="60438-220">Jak toho docílit, použít zajistí správnou úroveň služby (QoS Quality Service) k infrastruktuře přepínače tak, aby odpovídaly plánovaný bod obnovení a obnovení čas SLA cíl (RTO).</span><span class="sxs-lookup"><span data-stu-id="60438-220">To achieve this, apply the proper Quality of Service (QoS) level to your infrastructure switches to match your RPO and recovery time objective (RTO) SLAs.</span></span>

-   <span data-ttu-id="60438-221">Maximální latence přístupu k úložišti objektů Blob v Azure by měl být přibližně 80 ms.</span><span class="sxs-lookup"><span data-stu-id="60438-221">Maximum Azure Blob storage access latencies should be around 80 ms.</span></span>

### <a name="deploy-storsimple"></a><span data-ttu-id="60438-222">Nasazení zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-222">Deploy StorSimple</span></span>

<span data-ttu-id="60438-223">Pokyny krok za krokem StorSimple nasazení najdete v tématu [nasazení místního zařízení StorSimple](storsimple-deployment-walkthrough-u2.md).</span><span class="sxs-lookup"><span data-stu-id="60438-223">For step-by-step StorSimple deployment guidance, see [Deploy your on-premises StorSimple device](storsimple-deployment-walkthrough-u2.md).</span></span>

### <a name="deploy-netbackup"></a><span data-ttu-id="60438-224">Nasazení NetBackup</span><span class="sxs-lookup"><span data-stu-id="60438-224">Deploy NetBackup</span></span>

<span data-ttu-id="60438-225">Pokyny krok za krokem NetBackup 7.7.x nasazení najdete v tématu [NetBackup 7.7.x dokumentaci](http://www.veritas.com/docs/000094423).</span><span class="sxs-lookup"><span data-stu-id="60438-225">For step-by-step NetBackup 7.7.x deployment guidance, see the [NetBackup 7.7.x documentation](http://www.veritas.com/docs/000094423).</span></span>

## <a name="set-up-the-solution"></a><span data-ttu-id="60438-226">Nastavit řešení</span><span class="sxs-lookup"><span data-stu-id="60438-226">Set up the solution</span></span>

<span data-ttu-id="60438-227">V této části ukážeme některé příklady konfigurace.</span><span class="sxs-lookup"><span data-stu-id="60438-227">In this section, we demonstrate some configuration examples.</span></span> <span data-ttu-id="60438-228">Následující příklady a doporučení znázorňují nejvíce základní a základní implementaci.</span><span class="sxs-lookup"><span data-stu-id="60438-228">The following examples and recommendations illustrate the most basic and fundamental implementation.</span></span> <span data-ttu-id="60438-229">Tato implementace nemusí vztahovat přímo na konkrétní požadavky zálohy.</span><span class="sxs-lookup"><span data-stu-id="60438-229">This implementation might not apply directly to your specific backup requirements.</span></span>

### <a name="set-up-storsimple"></a><span data-ttu-id="60438-230">Nastavení pro zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-230">Set up StorSimple</span></span>

| <span data-ttu-id="60438-231">Úlohy nasazení zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-231">StorSimple deployment tasks</span></span>  | <span data-ttu-id="60438-232">Další komentáře</span><span class="sxs-lookup"><span data-stu-id="60438-232">Additional comments</span></span> |
|---|---|
| <span data-ttu-id="60438-233">Nasazení místního zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-233">Deploy your on-premises StorSimple device.</span></span> | <span data-ttu-id="60438-234">Podporované verze: aktualizace 3 a vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="60438-234">Supported versions: Update 3 and later versions.</span></span> |
| <span data-ttu-id="60438-235">Zapněte cíle zálohy.</span><span class="sxs-lookup"><span data-stu-id="60438-235">Turn on the backup target.</span></span> | <span data-ttu-id="60438-236">Pomocí těchto příkazů k zapnutí nebo vypnutí režimu cíl zálohy a získat stav.</span><span class="sxs-lookup"><span data-stu-id="60438-236">Use these commands to turn on or turn off backup target mode, and to get status.</span></span> <span data-ttu-id="60438-237">Další informace najdete v tématu [připojit vzdáleně k zařízení StorSimple](storsimple-remote-connect.md).</span><span class="sxs-lookup"><span data-stu-id="60438-237">For more information, see [Connect remotely to a StorSimple device](storsimple-remote-connect.md).</span></span></br> <span data-ttu-id="60438-238">Chcete zapnout režim zálohování: `Set-HCSBackupApplianceMode -enable`.</span><span class="sxs-lookup"><span data-stu-id="60438-238">To turn on backup mode: `Set-HCSBackupApplianceMode -enable`.</span></span> </br> <span data-ttu-id="60438-239">Chcete-li vypnout režim zálohování: `Set-HCSBackupApplianceMode -disable`.</span><span class="sxs-lookup"><span data-stu-id="60438-239">To turn off backup mode: `Set-HCSBackupApplianceMode -disable`.</span></span> </br> <span data-ttu-id="60438-240">Chcete-li získat aktuální stav nastavení režim zálohování: `Get-HCSBackupApplianceMode`.</span><span class="sxs-lookup"><span data-stu-id="60438-240">To get the current state of backup mode settings: `Get-HCSBackupApplianceMode`.</span></span> |
| <span data-ttu-id="60438-241">Vytvořte kontejner svazků společné pro uložení zálohy dat svazku.</span><span class="sxs-lookup"><span data-stu-id="60438-241">Create a common volume container for your volume that stores the backup data.</span></span> <span data-ttu-id="60438-242">Všechna data v kontejneru svazku se odstraňují.</span><span class="sxs-lookup"><span data-stu-id="60438-242">All data in a volume container is deduplicated.</span></span> | <span data-ttu-id="60438-243">Kontejnery svazků zařízení StorSimple definování domén odstranění duplicit.</span><span class="sxs-lookup"><span data-stu-id="60438-243">StorSimple volume containers define deduplication domains.</span></span>  |
| <span data-ttu-id="60438-244">Vytvořte svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-244">Create StorSimple volumes.</span></span> | <span data-ttu-id="60438-245">Vytvořte svazky s velikostí jako blízko předpokládaného využití nejdříve, protože velikost svazku ovlivňují cloud snímku doba trvání.</span><span class="sxs-lookup"><span data-stu-id="60438-245">Create volumes with sizes as close to the anticipated usage as possible, because volume size affects cloud snapshot duration time.</span></span> <span data-ttu-id="60438-246">Informace o tom, jak velikost svazku, přečtěte si informace o [zásady uchovávání informací](#retention-policies).</span><span class="sxs-lookup"><span data-stu-id="60438-246">For information about how to size a volume, read about [retention policies](#retention-policies).</span></span></br> </br> <span data-ttu-id="60438-247">Použití StorSimple zřízeny vrstvené svazky a vyberte **použít tento svazek pro archivní data s méně často používaná** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="60438-247">Use StorSimple tiered volumes, and select the **Use this volume for less frequently accessed archival data** check box.</span></span> </br> <span data-ttu-id="60438-248">Použití pouze místně vázaný svazky není podporováno.</span><span class="sxs-lookup"><span data-stu-id="60438-248">Using only locally pinned volumes is not supported.</span></span> |
| <span data-ttu-id="60438-249">Vytvoření jedinečné zásady StorSimple zálohování pro všechny svazky, cíl zálohy.</span><span class="sxs-lookup"><span data-stu-id="60438-249">Create a unique StorSimple backup policy for all the backup target volumes.</span></span> | <span data-ttu-id="60438-250">Zásady zálohování StorSimple definuje skupinu konzistence svazku.</span><span class="sxs-lookup"><span data-stu-id="60438-250">A StorSimple backup policy defines the volume consistency group.</span></span> |
| <span data-ttu-id="60438-251">Zakažte plán, protože platnost vyprší snímky.</span><span class="sxs-lookup"><span data-stu-id="60438-251">Disable the schedule as the snapshots expire.</span></span> | <span data-ttu-id="60438-252">Snímky jsou aktivovány jako následného zpracování operace.</span><span class="sxs-lookup"><span data-stu-id="60438-252">Snapshots are triggered as a post-processing operation.</span></span> |

### <a name="set-up-the-host-backup-server-storage"></a><span data-ttu-id="60438-253">Nastavení úložiště zálohování serveru hostitele</span><span class="sxs-lookup"><span data-stu-id="60438-253">Set up the host backup server storage</span></span>

<span data-ttu-id="60438-254">Nastavení úložiště zálohování serveru hostitele podle těchto pokynů:</span><span class="sxs-lookup"><span data-stu-id="60438-254">Set up the host backup server storage according to these guidelines:</span></span>  

- <span data-ttu-id="60438-255">Nepoužívejte rozložené svazky (vytvořený Správa disků systému Windows); rozložené svazky nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="60438-255">Don't use spanned volumes (created by Windows Disk Management); spanned volumes are not supported.</span></span>
- <span data-ttu-id="60438-256">Formátovat pomocí systému souborů NTFS o velikosti 64 KB alokační svazků.</span><span class="sxs-lookup"><span data-stu-id="60438-256">Format your volumes using NTFS with 64-KB allocation size.</span></span>
- <span data-ttu-id="60438-257">Mapovat přímo na NetBackup server svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-257">Map the StorSimple volumes directly to the NetBackup server.</span></span>
    - <span data-ttu-id="60438-258">Použijte iSCSI pro fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="60438-258">Use iSCSI for physical servers.</span></span>
    - <span data-ttu-id="60438-259">Použijte průchozí disky pro virtuální servery.</span><span class="sxs-lookup"><span data-stu-id="60438-259">Use pass-through disks for virtual servers.</span></span>


## <a name="best-practices-for-storsimple-and-netbackup"></a><span data-ttu-id="60438-260">Osvědčené postupy pro StorSimple a NetBackup</span><span class="sxs-lookup"><span data-stu-id="60438-260">Best practices for StorSimple and NetBackup</span></span>

<span data-ttu-id="60438-261">Nastavit řešení podle pokynů v následujícím několik oddílů.</span><span class="sxs-lookup"><span data-stu-id="60438-261">Set up your solution according to the guidelines in the following few sections.</span></span>

### <a name="operating-system-best-practices"></a><span data-ttu-id="60438-262">Osvědčené postupy operačního systému</span><span class="sxs-lookup"><span data-stu-id="60438-262">Operating system best practices</span></span>

-   <span data-ttu-id="60438-263">Zakažte šifrování na Windows serveru a odstranění duplicit pro systém souborů NTFS.</span><span class="sxs-lookup"><span data-stu-id="60438-263">Disable Windows Server encryption and deduplication for the NTFS file system.</span></span>
-   <span data-ttu-id="60438-264">Zakážete defragmentace systému Windows Server na svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-264">Disable Windows Server defragmentation on the StorSimple volumes.</span></span>
-   <span data-ttu-id="60438-265">Zakažte indexování systému Windows Server na svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-265">Disable Windows Server indexing on the StorSimple volumes.</span></span>
-   <span data-ttu-id="60438-266">Spusťte antivirovým na zdrojovém hostiteli (ne proti svazky zařízení StorSimple).</span><span class="sxs-lookup"><span data-stu-id="60438-266">Run an antivirus scan at the source host (not against the StorSimple volumes).</span></span>
-   <span data-ttu-id="60438-267">Vypnout výchozí [údržby systému Windows Server](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) ve Správci úloh.</span><span class="sxs-lookup"><span data-stu-id="60438-267">Turn off the default [Windows Server maintenance](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) in Task Manager.</span></span> <span data-ttu-id="60438-268">To udělat v jednom z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="60438-268">Do this in one of the following ways:</span></span>
    - <span data-ttu-id="60438-269">Vypněte Konfigurátor údržby v Plánovači úloh systému Windows.</span><span class="sxs-lookup"><span data-stu-id="60438-269">Turn off the Maintenance configurator in Windows Task Scheduler.</span></span>
    - <span data-ttu-id="60438-270">Stáhněte si [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) z webu Windows Sysinternals.</span><span class="sxs-lookup"><span data-stu-id="60438-270">Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) from Windows Sysinternals.</span></span> <span data-ttu-id="60438-271">Po stažení nástroje PsExec, spusťte prostředí Windows PowerShell jako správce a zadejte:</span><span class="sxs-lookup"><span data-stu-id="60438-271">After you download PsExec, run Windows PowerShell as an administrator, and type:</span></span>
      ```powershell
      psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
      ```

### <a name="storsimple-best-practices"></a><span data-ttu-id="60438-272">Osvědčené postupy StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-272">StorSimple best practices</span></span>

-   <span data-ttu-id="60438-273">Ujistěte se, že zařízení StorSimple se aktualizuje na [Update 3 nebo novější](storsimple-install-update-3.md).</span><span class="sxs-lookup"><span data-stu-id="60438-273">Be sure that the StorSimple device is updated to [Update 3 or later](storsimple-install-update-3.md).</span></span>
-   <span data-ttu-id="60438-274">Izolovat přenosy iSCSI a cloudu.</span><span class="sxs-lookup"><span data-stu-id="60438-274">Isolate iSCSI and cloud traffic.</span></span> <span data-ttu-id="60438-275">Použijte vyhrazené iSCSI připojení pro přenos dat mezi StorSimple a zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="60438-275">Use dedicated iSCSI connections for traffic between StorSimple and the backup server.</span></span>
-   <span data-ttu-id="60438-276">Ujistěte se, že zařízení StorSimple je vyhrazené cíl zálohy.</span><span class="sxs-lookup"><span data-stu-id="60438-276">Be sure that your StorSimple device is a dedicated backup target.</span></span> <span data-ttu-id="60438-277">Smíšené úlohy nejsou podporované, protože mají vliv na RTO a plánovaný bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="60438-277">Mixed workloads are not supported because they affect your RTO and RPO.</span></span>

### <a name="netbackup-best-practices"></a><span data-ttu-id="60438-278">Osvědčené postupy NetBackup</span><span class="sxs-lookup"><span data-stu-id="60438-278">NetBackup best practices</span></span>

-   <span data-ttu-id="60438-279">Databázi NetBackup by měl být místní pro server a není umístěn v svazek StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-279">The NetBackup database should be local to the server and not reside on a StorSimple volume.</span></span>
-   <span data-ttu-id="60438-280">Pro zotavení po havárii zálohujte databázi NetBackup na svazek StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-280">For disaster recovery, back up the NetBackup database on a StorSimple volume.</span></span>
-   <span data-ttu-id="60438-281">Podporujeme NetBackup úplné a přírůstkové zálohování (také označované jako rozdílové přírůstkových záloh v NetBackup) pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="60438-281">We support NetBackup full and incremental backups (also referred to as differential incremental backups in NetBackup) for this solution.</span></span> <span data-ttu-id="60438-282">Doporučujeme vám, nepoužívejte syntetické a kumulativní přírůstkové zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-282">We recommend that you do not use synthetic and cumulative incremental backups.</span></span>
-   <span data-ttu-id="60438-283">Zálohování datových souborů by měl obsahovat pouze data pro konkrétní úlohu.</span><span class="sxs-lookup"><span data-stu-id="60438-283">Backup data files should contain only the data for a specific job.</span></span> <span data-ttu-id="60438-284">Například žádné médium připojí přes různé úlohy jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="60438-284">For example, no media appends across different jobs are allowed.</span></span>

<span data-ttu-id="60438-285">Nejnovější nastavení NetBackup a osvědčené postupy při implementaci těchto požadavků naleznete v dokumentaci NetBackup v [www.veritas.com](https://www.veritas.com).</span><span class="sxs-lookup"><span data-stu-id="60438-285">For the latest NetBackup settings and best practices for implementing these requirements, see the NetBackup documentation at [www.veritas.com](https://www.veritas.com).</span></span>


## <a name="retention-policies"></a><span data-ttu-id="60438-286">Zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="60438-286">Retention policies</span></span>

<span data-ttu-id="60438-287">Jeden z nejběžnějších typů zásad uchovávání záloh je zásada Dědečka otec a SYN (GFS).</span><span class="sxs-lookup"><span data-stu-id="60438-287">One of the most common backup retention policy types is a Grandfather, Father, and Son (GFS) policy.</span></span> <span data-ttu-id="60438-288">V zásadách GFS přírůstkové zálohy se provádí denně a úplné zálohy se provádějí týdenní a měsíční.</span><span class="sxs-lookup"><span data-stu-id="60438-288">In a GFS policy, an incremental backup is performed daily and full backups are done weekly and monthly.</span></span> <span data-ttu-id="60438-289">Výsledkem zásad šesti StorSimple zřízeny vrstvené svazky: jeden svazek obsahuje úplných záloh týdenní, měsíční nebo roční; pět svazky úložiště každodenní přírůstkové zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-289">This policy results in six StorSimple tiered volumes: one volume contains the weekly, monthly, and yearly full backups; the other five volumes store daily incremental backups.</span></span>

<span data-ttu-id="60438-290">V následujícím příkladu použijeme rotaci GFS.</span><span class="sxs-lookup"><span data-stu-id="60438-290">In the following example, we use a GFS rotation.</span></span> <span data-ttu-id="60438-291">Příklad předpokládá následující:</span><span class="sxs-lookup"><span data-stu-id="60438-291">The example assumes the following:</span></span>

-   <span data-ttu-id="60438-292">Bez zajištěná nebo komprimovaných dat se používá.</span><span class="sxs-lookup"><span data-stu-id="60438-292">Non-deduped or compressed data is used.</span></span>
-   <span data-ttu-id="60438-293">Úplné zálohy představují 1 TiB.</span><span class="sxs-lookup"><span data-stu-id="60438-293">Full backups are 1 TiB each.</span></span>
-   <span data-ttu-id="60438-294">Každodenní přírůstkové zálohování jsou 500 GiB.</span><span class="sxs-lookup"><span data-stu-id="60438-294">Daily incremental backups are 500 GiB each.</span></span>
-   <span data-ttu-id="60438-295">Čtyři týdenní zálohy jsou v měsíci.</span><span class="sxs-lookup"><span data-stu-id="60438-295">Four weekly backups are kept for a month.</span></span>
-   <span data-ttu-id="60438-296">Dvanáct měsíční zálohy jsou v roce.</span><span class="sxs-lookup"><span data-stu-id="60438-296">Twelve monthly backups are kept for a year.</span></span>
-   <span data-ttu-id="60438-297">Jeden rok zálohování je udržováno 10 let.</span><span class="sxs-lookup"><span data-stu-id="60438-297">One yearly backup is kept for 10 years.</span></span>

<span data-ttu-id="60438-298">Podle předchozích předpokladů, vytvořit 26-TiB StorSimple vrstvené svazek pro měsíční a roční úplné zálohy.</span><span class="sxs-lookup"><span data-stu-id="60438-298">Based on the preceding assumptions, create a 26-TiB StorSimple tiered volume for the monthly and yearly full backups.</span></span> <span data-ttu-id="60438-299">Vytvoření 5-TiB StorSimple vrstvené svazku pro každou přírůstkovou denní zálohy.</span><span class="sxs-lookup"><span data-stu-id="60438-299">Create a 5-TiB StorSimple tiered volume for each of the incremental daily backups.</span></span>

| <span data-ttu-id="60438-300">Uchování typ zálohy</span><span class="sxs-lookup"><span data-stu-id="60438-300">Backup type retention</span></span> | <span data-ttu-id="60438-301">Velikost (TiB)</span><span class="sxs-lookup"><span data-stu-id="60438-301">Size (TiB)</span></span> | <span data-ttu-id="60438-302">Násobitel GFS\*</span><span class="sxs-lookup"><span data-stu-id="60438-302">GFS multiplier\*</span></span> | <span data-ttu-id="60438-303">Celková kapacita (TiB)</span><span class="sxs-lookup"><span data-stu-id="60438-303">Total capacity (TiB)</span></span>  |
|---|---|---|---|
| <span data-ttu-id="60438-304">Týdenní úplné</span><span class="sxs-lookup"><span data-stu-id="60438-304">Weekly full</span></span> | <span data-ttu-id="60438-305">1</span><span class="sxs-lookup"><span data-stu-id="60438-305">1</span></span> | <span data-ttu-id="60438-306">4</span><span class="sxs-lookup"><span data-stu-id="60438-306">4</span></span>  | <span data-ttu-id="60438-307">4</span><span class="sxs-lookup"><span data-stu-id="60438-307">4</span></span> |
| <span data-ttu-id="60438-308">Každodenní přírůstkové</span><span class="sxs-lookup"><span data-stu-id="60438-308">Daily incremental</span></span> | <span data-ttu-id="60438-309">0.5</span><span class="sxs-lookup"><span data-stu-id="60438-309">0.5</span></span> | <span data-ttu-id="60438-310">20 (cykly stejný počet týdnů v měsíci)</span><span class="sxs-lookup"><span data-stu-id="60438-310">20 (cycles equal number of weeks per month)</span></span> | <span data-ttu-id="60438-311">12 (2 pro další kvóty)</span><span class="sxs-lookup"><span data-stu-id="60438-311">12 (2 for additional quota)</span></span> |
| <span data-ttu-id="60438-312">Měsíční úplné</span><span class="sxs-lookup"><span data-stu-id="60438-312">Monthly full</span></span> | <span data-ttu-id="60438-313">1</span><span class="sxs-lookup"><span data-stu-id="60438-313">1</span></span> | <span data-ttu-id="60438-314">12</span><span class="sxs-lookup"><span data-stu-id="60438-314">12</span></span> | <span data-ttu-id="60438-315">12</span><span class="sxs-lookup"><span data-stu-id="60438-315">12</span></span> |
| <span data-ttu-id="60438-316">Roční úplné</span><span class="sxs-lookup"><span data-stu-id="60438-316">Yearly full</span></span> | <span data-ttu-id="60438-317">1</span><span class="sxs-lookup"><span data-stu-id="60438-317">1</span></span>  | <span data-ttu-id="60438-318">10</span><span class="sxs-lookup"><span data-stu-id="60438-318">10</span></span> | <span data-ttu-id="60438-319">10</span><span class="sxs-lookup"><span data-stu-id="60438-319">10</span></span> |
| <span data-ttu-id="60438-320">Požadavek GFS</span><span class="sxs-lookup"><span data-stu-id="60438-320">GFS requirement</span></span> |   | <span data-ttu-id="60438-321">38</span><span class="sxs-lookup"><span data-stu-id="60438-321">38</span></span> |   |
| <span data-ttu-id="60438-322">Další kvóty</span><span class="sxs-lookup"><span data-stu-id="60438-322">Additional quota</span></span>  | <span data-ttu-id="60438-323">4</span><span class="sxs-lookup"><span data-stu-id="60438-323">4</span></span>  |   | <span data-ttu-id="60438-324">42 celkový požadavek GFS</span><span class="sxs-lookup"><span data-stu-id="60438-324">42 total GFS requirement</span></span>  |
<span data-ttu-id="60438-325">\*Násobitel GFS je počet kopií, které potřebujete k ochraně a zachovat podle požadavků vaší zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-325">\* The GFS multiplier is the number of copies you need to protect and retain to meet your backup policy requirements.</span></span>

## <a name="set-up-netbackup-storage"></a><span data-ttu-id="60438-326">Nastavení úložiště NetBackup</span><span class="sxs-lookup"><span data-stu-id="60438-326">Set up NetBackup storage</span></span>

### <a name="to-set-up-netbackup-storage"></a><span data-ttu-id="60438-327">Instalace služby NetBackup úložiště</span><span class="sxs-lookup"><span data-stu-id="60438-327">To set up NetBackup storage</span></span>

1.  <span data-ttu-id="60438-328">V konzole pro správu NetBackup vyberte **média a správa zařízení** > **zařízení** > **fondy disků**.</span><span class="sxs-lookup"><span data-stu-id="60438-328">In the NetBackup Administration Console, select **Media and Device Management** > **Devices** > **Disk Pools**.</span></span> <span data-ttu-id="60438-329">V Průvodci konfigurací disku fondu, vyberte typ serveru úložiště **AdvancedDisk**a potom vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="60438-329">In the Disk Pool Configuration Wizard, select the storage server type **AdvancedDisk**, and then select **Next**.</span></span>

    ![Konzole pro správu NetBackup, Průvodce konfigurací disku fondu](./media/storsimple-configure-backup-target-using-netbackup/nbimage1.png)

2.  <span data-ttu-id="60438-331">Vyberte svůj server a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="60438-331">Select your server, and then select **Next**.</span></span>

    ![NetBackup konzole pro správu, vyberte server](./media/storsimple-configure-backup-target-using-netbackup/nbimage2.png)

3.  <span data-ttu-id="60438-333">Vyberte svazek StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-333">Select your StorSimple volume.</span></span>

    ![NetBackup konzole pro správu, vyberte disk, který svazek StorSimple](./media/storsimple-configure-backup-target-using-netbackup/nbimage3.png)

4.  <span data-ttu-id="60438-335">Zadejte název cíle zálohy a potom vyberte **Další** > **Další** ukončíte průvodce.</span><span class="sxs-lookup"><span data-stu-id="60438-335">Enter a name for the backup target, and then select **Next** > **Next** to finish the wizard.</span></span>

5.  <span data-ttu-id="60438-336">Zkontrolujte nastavení a potom vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="60438-336">Review the settings, and then select **Finish**.</span></span>

6.  <span data-ttu-id="60438-337">Na konci každé přiřazení svazku, změňte nastavení zařízení úložiště tak, aby odpovídaly tyto doporučené v [osvědčené postupy pro StorSimple a NetBackup](#best-practices-for-storsimple-and-netbackup).</span><span class="sxs-lookup"><span data-stu-id="60438-337">At the end of each volume assignment, change the storage device settings to match those recommended in [Best practices for StorSimple and NetBackup](#best-practices-for-storsimple-and-netbackup).</span></span>

7. <span data-ttu-id="60438-338">Opakujte kroky 1 až 6, dokud přiřazení formátujte svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-338">Repeat steps 1-6 until you are finished assigning your StorSimple volumes.</span></span>

    ![Konzole pro správu NetBackup, konfigurace disku](./media/storsimple-configure-backup-target-using-netbackup/nbimage5.png)

## <a name="set-up-storsimple-as-a-primary-backup-target"></a><span data-ttu-id="60438-340">Nastavit StorSimple jako primární cíl zálohy</span><span class="sxs-lookup"><span data-stu-id="60438-340">Set up StorSimple as a primary backup target</span></span>

> [!NOTE]
> <span data-ttu-id="60438-341">Rychlostmi cloudu dojít k obnovení dat ze zálohy, které byly zřízeny vrstvené do cloudu.</span><span class="sxs-lookup"><span data-stu-id="60438-341">Data restores from a backup that has been tiered to the cloud occur at cloud speeds.</span></span>

<span data-ttu-id="60438-342">Následující obrázek znázorňuje mapování svazek typické pro úlohu zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-342">The following figure shows the mapping of a typical volume to a backup job.</span></span> <span data-ttu-id="60438-343">V takovém případě všechny týdenní zálohy mapování na sobotu celého disku a mapování přírůstkové zálohování na disky přírůstkové pondělí – pátek.</span><span class="sxs-lookup"><span data-stu-id="60438-343">In this case, all the weekly backups map to the Saturday full disk, and the incremental backups map to Monday-Friday incremental disks.</span></span> <span data-ttu-id="60438-344">Všechny vytvořené zálohy a obnovení jsou z StorSimple vrstvené svazku.</span><span class="sxs-lookup"><span data-stu-id="60438-344">All the backups and restores are from a StorSimple tiered volume.</span></span>

![<span data-ttu-id="60438-345">Logický diagram konfigurace primární cíl zálohy</span><span class="sxs-lookup"><span data-stu-id="60438-345">Primary backup target configuration logical diagram</span></span> ](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a><span data-ttu-id="60438-346">StorSimple jako primární cíl zálohování GFS naplánovat příklad</span><span class="sxs-lookup"><span data-stu-id="60438-346">StorSimple as a primary backup target GFS schedule example</span></span>

<span data-ttu-id="60438-347">Tady je příklad plánu otočení GFS čtyři týdny, měsíční nebo roční:</span><span class="sxs-lookup"><span data-stu-id="60438-347">Here's an example of a GFS rotation schedule for four weeks, monthly, and yearly:</span></span>

| <span data-ttu-id="60438-348">Typ frekvence nebo zálohování</span><span class="sxs-lookup"><span data-stu-id="60438-348">Frequency/backup type</span></span> | <span data-ttu-id="60438-349">Úplná</span><span class="sxs-lookup"><span data-stu-id="60438-349">Full</span></span> | <span data-ttu-id="60438-350">Přírůstkové (1-5 dnů)</span><span class="sxs-lookup"><span data-stu-id="60438-350">Incremental (days 1-5)</span></span>  |   
|---|---|---|
| <span data-ttu-id="60438-351">Každý týden (1 – 4 týdny)</span><span class="sxs-lookup"><span data-stu-id="60438-351">Weekly (weeks 1-4)</span></span> | <span data-ttu-id="60438-352">Sobota</span><span class="sxs-lookup"><span data-stu-id="60438-352">Saturday</span></span> | <span data-ttu-id="60438-353">Pondělí – pátek</span><span class="sxs-lookup"><span data-stu-id="60438-353">Monday-Friday</span></span> |
| <span data-ttu-id="60438-354">Měsíční</span><span class="sxs-lookup"><span data-stu-id="60438-354">Monthly</span></span>  | <span data-ttu-id="60438-355">Sobota</span><span class="sxs-lookup"><span data-stu-id="60438-355">Saturday</span></span>  |   |
| <span data-ttu-id="60438-356">Ročně</span><span class="sxs-lookup"><span data-stu-id="60438-356">Yearly</span></span> | <span data-ttu-id="60438-357">Sobota</span><span class="sxs-lookup"><span data-stu-id="60438-357">Saturday</span></span>  |   |   |

## <a name="assigning-storsimple-volumes-to-a-netbackup-backup-job"></a><span data-ttu-id="60438-358">Přiřazení k úlohu zálohování NetBackup svazky zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-358">Assigning StorSimple volumes to a NetBackup backup job</span></span>

<span data-ttu-id="60438-359">Následující pořadí předpokládá, že jsou v souladu s pokyny agenta NetBackup nakonfigurované NetBackup a cílový hostitel.</span><span class="sxs-lookup"><span data-stu-id="60438-359">The following sequence assumes that NetBackup and the target host are configured in accordance with the NetBackup agent guidelines.</span></span>

### <a name="to-assign-storsimple-volumes-to-a-netbackup-backup-job"></a><span data-ttu-id="60438-360">Přiřadit úlohu zálohování NetBackup svazky zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-360">To assign StorSimple volumes to a NetBackup backup job</span></span>

1.  <span data-ttu-id="60438-361">V konzole pro správu NetBackup vyberte **NetBackup správu**, klikněte pravým tlačítkem na **zásady**a potom vyberte **nové zásady**.</span><span class="sxs-lookup"><span data-stu-id="60438-361">In the NetBackup Administration Console, select **NetBackup Management**, right-click **Policies**, and then select **New Policy**.</span></span>

    ![Konzole pro správu NetBackup, vytvořte novou zásadu](./media/storsimple-configure-backup-target-using-netbackup/nbimage6.png)

2.  <span data-ttu-id="60438-363">V **přidání nové zásady** dialogové okno, zadejte název zásady a pak vyberte **použití Průvodce konfigurací zásad** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="60438-363">In the **Add a New Policy** dialog box, enter a name for the policy, and then select the **Use Policy Configuration Wizard** check box.</span></span> <span data-ttu-id="60438-364">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="60438-364">Select **OK**.</span></span>

    ![Konzole pro správu NetBackup, přidat dialogové okno nové zásady](./media/storsimple-configure-backup-target-using-netbackup/nbimage7.png)

3.  <span data-ttu-id="60438-366">V Průvodci konfigurací zálohování zásady zvolit typ zálohy a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="60438-366">In the Backup Policy Configuration Wizard, elect the backup type you want, and then select **Next**.</span></span>

    ![Konzole pro správu NetBackup, vyberte typ zálohy](./media/storsimple-configure-backup-target-using-netbackup/nbimage8.png)

4.  <span data-ttu-id="60438-368">Pokud chcete nastavit typ zásad, vyberte **standardní**a potom vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="60438-368">To set the policy type, select **Standard**, and then select **Next**.</span></span>

    ![Konzole pro správu NetBackup, typ vyberte zásad](./media/storsimple-configure-backup-target-using-netbackup/nbimage9.png)

5.  <span data-ttu-id="60438-370">Vyberte svého hostitele, vyberte **zjistit klientský operační systém** zaškrtněte políčko a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="60438-370">Select your host, select the **Detect client operating system** check box, and then select **Add**.</span></span> <span data-ttu-id="60438-371">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="60438-371">Select **Next**.</span></span>

    ![Konzole pro správu NetBackup, seznamu klienty v nové zásady](./media/storsimple-configure-backup-target-using-netbackup/nbimage10.png)

6.  <span data-ttu-id="60438-373">Vyberte jednotky, které chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="60438-373">Select the drives you want to back up.</span></span>

    ![Konzole pro správu NetBackup, výběr zálohování pro novou zásadu](./media/storsimple-configure-backup-target-using-netbackup/nbimage11.png)

7.  <span data-ttu-id="60438-375">Vyberte četnost a uchování hodnoty, které splňují požadavky rotace záloh.</span><span class="sxs-lookup"><span data-stu-id="60438-375">Select the frequency and retention values that meet your backup rotation requirements.</span></span>

    ![Konzole pro správu NetBackup, četnost zálohování a oběh pro nové zásady](./media/storsimple-configure-backup-target-using-netbackup/nbimage12.png)

8.  <span data-ttu-id="60438-377">Vyberte **Další** > **Další** > **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="60438-377">Select **Next** > **Next** > **Finish**.</span></span>  <span data-ttu-id="60438-378">Po vytvoření zásady, můžete upravit plán.</span><span class="sxs-lookup"><span data-stu-id="60438-378">You can modify the schedule after the policy is created.</span></span>

9.  <span data-ttu-id="60438-379">Vyberte rozbalte zásady, které jste právě vytvořili, a potom vyberte **plány**.</span><span class="sxs-lookup"><span data-stu-id="60438-379">Select to expand the policy you just created, and then select **Schedules**.</span></span>

    ![Konzole pro správu NetBackup, plány pro nové zásady](./media/storsimple-configure-backup-target-using-netbackup/nbimage13.png)

10.  <span data-ttu-id="60438-381">Klikněte pravým tlačítkem na **rozdíl Inc**, vyberte **zkopírujte do nové**a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="60438-381">Right-click **Differential-Inc**, select **Copy to new**, and then select **OK**.</span></span>

    ![Konzole pro správu NetBackup, plán kopírování na nové zásady](./media/storsimple-configure-backup-target-using-netbackup/nbimage14.png)

11.  <span data-ttu-id="60438-383">Klikněte pravým tlačítkem na nově vytvořený plán a potom vyberte **změnu**.</span><span class="sxs-lookup"><span data-stu-id="60438-383">Right-click the newly created schedule, and then select **Change**.</span></span>

12.  <span data-ttu-id="60438-384">Na **atributy** vyberte **přepsat výběr úložiště zásad** zaškrtněte políčko a potom vyberte svazek, kde pondělí přírůstkové zálohování přejděte.</span><span class="sxs-lookup"><span data-stu-id="60438-384">On the **Attributes** tab, select the **Override policy storage selection** check box, and then select the volume where Monday incremental backups go.</span></span>

    ![Konzole pro správu NetBackup, změnit plán](./media/storsimple-configure-backup-target-using-netbackup/nbimage15.png)

13.  <span data-ttu-id="60438-386">Na **spustit okno** vyberte časový interval pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-386">On the **Start Window** tab, select the time window for your backups.</span></span>

    ![Konzole pro správu NetBackup, časový interval pro změnu start](./media/storsimple-configure-backup-target-using-netbackup/nbimage16.png)

14.  <span data-ttu-id="60438-388">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="60438-388">Select **OK**.</span></span>

15.  <span data-ttu-id="60438-389">Opakujte kroky 10-14 pro každé přírůstkové zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-389">Repeat steps 10-14 for each incremental backup.</span></span> <span data-ttu-id="60438-390">Vyberte vhodný svazek a plán pro každé zálohování, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="60438-390">Select the appropriate volume and schedule for each backup you create.</span></span>

16.  <span data-ttu-id="60438-391">Klikněte pravým tlačítkem myši **rozdíl Inc** naplánovat a poté jej odstraňte.</span><span class="sxs-lookup"><span data-stu-id="60438-391">Right-click the **Differential-Inc** schedule, and then delete it.</span></span>

17.  <span data-ttu-id="60438-392">Upravte plán úplné zálohování potřebám.</span><span class="sxs-lookup"><span data-stu-id="60438-392">Modify your Full schedule to meet your backup needs.</span></span>

    ![Konzole pro správu NetBackup, změna plánu úplných záloh](./media/storsimple-configure-backup-target-using-netbackup/nbimage17.png)

18.  <span data-ttu-id="60438-394">Změňte počáteční okna.</span><span class="sxs-lookup"><span data-stu-id="60438-394">Change the start window.</span></span>

    ![Konzole pro správu NetBackup, změnit okna spustit](./media/storsimple-configure-backup-target-using-netbackup/nbimage18.png)

19.  <span data-ttu-id="60438-396">Poslední plán vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="60438-396">The final schedule looks like this:</span></span>

    ![Konzole pro správu NetBackup, posledním plánu](./media/storsimple-configure-backup-target-using-netbackup/nbimage19.png)

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a><span data-ttu-id="60438-398">Nastavit StorSimple jako sekundární cíl zálohy</span><span class="sxs-lookup"><span data-stu-id="60438-398">Set up StorSimple as a secondary backup target</span></span>

> [!NOTE]
><span data-ttu-id="60438-399">Rychlostmi cloudu dojít k obnovení dat ze zálohy, které byly zřízeny vrstvené do cloudu.</span><span class="sxs-lookup"><span data-stu-id="60438-399">Data restores from a backup that has been tiered to the cloud occur at cloud speeds.</span></span>

<span data-ttu-id="60438-400">V tomto modelu musí mít na úložná média (jiné než StorSimple) sloužit jako dočasnou vyrovnávací paměť.</span><span class="sxs-lookup"><span data-stu-id="60438-400">In this model, you must have a storage media (other than StorSimple) to serve as a temporary cache.</span></span> <span data-ttu-id="60438-401">Například můžete použít redundantní pole nezávislých disků (RAID) svazku pro uchování místa, vstupní a výstupní (I/O) a šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="60438-401">For example, you can use a redundant array of independent disks (RAID) volume to accommodate space, input/output (I/O), and bandwidth.</span></span> <span data-ttu-id="60438-402">Doporučujeme používat RAID 5, 50 a 10.</span><span class="sxs-lookup"><span data-stu-id="60438-402">We recommend using RAID 5, 50, and 10.</span></span>

<span data-ttu-id="60438-403">Následující obrázek znázorňuje Typická krátkodobý uchování místní (pro server) svazky a dlouhodobé uchovávání archivy svazky.</span><span class="sxs-lookup"><span data-stu-id="60438-403">The following figure shows typical short-term retention local (to the server) volumes and long-term retention archives volumes.</span></span> <span data-ttu-id="60438-404">V tomto scénáři spustit všechny zálohy na místní (pro server) svazek RAID.</span><span class="sxs-lookup"><span data-stu-id="60438-404">In this scenario, all backups run on the local (to the server) RAID volume.</span></span> <span data-ttu-id="60438-405">Tyto zálohy jsou pravidelně duplicitní a archivovat na svazek archivy.</span><span class="sxs-lookup"><span data-stu-id="60438-405">These backups are periodically duplicated and archived to an archives volume.</span></span> <span data-ttu-id="60438-406">Je důležité velikost svazku RAID místní (pro server) tak, aby ji může zpracovat krátkodobé uchování požadavky kapacitu a výkon.</span><span class="sxs-lookup"><span data-stu-id="60438-406">It is important to size your local (to the server) RAID volume so that it can handle your short-term retention capacity and performance requirements.</span></span>

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a><span data-ttu-id="60438-407">StorSimple jako v příkladu GFS sekundární cíl zálohy</span><span class="sxs-lookup"><span data-stu-id="60438-407">StorSimple as a secondary backup target GFS example</span></span>

![StorSimple jako sekundární cíl zálohy logického diagramu](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetdiagram.png)

<span data-ttu-id="60438-409">Následující tabulka ukazuje, jak nastavení zálohování ke spuštění na místní a StorSimple disky.</span><span class="sxs-lookup"><span data-stu-id="60438-409">The following table shows how to set up backups to run on the local and StorSimple disks.</span></span> <span data-ttu-id="60438-410">Její součástí jsou požadavky na jednotlivé a celkové kapacity.</span><span class="sxs-lookup"><span data-stu-id="60438-410">It includes individual and total capacity requirements.</span></span>

### <a name="backup-configuration-and-capacity-requirements"></a><span data-ttu-id="60438-411">Konfigurace zálohování a požadavky na kapacitu</span><span class="sxs-lookup"><span data-stu-id="60438-411">Backup configuration and capacity requirements</span></span>

| <span data-ttu-id="60438-412">Typ zálohování a uchovávání</span><span class="sxs-lookup"><span data-stu-id="60438-412">Backup type and retention</span></span> | <span data-ttu-id="60438-413">Úložiště</span><span class="sxs-lookup"><span data-stu-id="60438-413">Configured storage</span></span> | <span data-ttu-id="60438-414">Velikost (TiB)</span><span class="sxs-lookup"><span data-stu-id="60438-414">Size (TiB)</span></span> | <span data-ttu-id="60438-415">Násobitel GFS</span><span class="sxs-lookup"><span data-stu-id="60438-415">GFS multiplier</span></span> | <span data-ttu-id="60438-416">Celková kapacita\* (TiB)</span><span class="sxs-lookup"><span data-stu-id="60438-416">Total capacity\* (TiB)</span></span> |
|---|---|---|---|---|
| <span data-ttu-id="60438-417">Týden 1 (úplné a přírůstkové)</span><span class="sxs-lookup"><span data-stu-id="60438-417">Week 1 (full and incremental)</span></span> |<span data-ttu-id="60438-418">Místní disk (krátkodobé)</span><span class="sxs-lookup"><span data-stu-id="60438-418">Local disk (short-term)</span></span>| <span data-ttu-id="60438-419">1</span><span class="sxs-lookup"><span data-stu-id="60438-419">1</span></span> | <span data-ttu-id="60438-420">1</span><span class="sxs-lookup"><span data-stu-id="60438-420">1</span></span> | <span data-ttu-id="60438-421">1</span><span class="sxs-lookup"><span data-stu-id="60438-421">1</span></span> |
| <span data-ttu-id="60438-422">Týdny StorSimple 2 – 4</span><span class="sxs-lookup"><span data-stu-id="60438-422">StorSimple weeks 2-4</span></span> |<span data-ttu-id="60438-423">StorSimple disku (dlouhodobé)</span><span class="sxs-lookup"><span data-stu-id="60438-423">StorSimple disk (long-term)</span></span> | <span data-ttu-id="60438-424">1</span><span class="sxs-lookup"><span data-stu-id="60438-424">1</span></span> | <span data-ttu-id="60438-425">4</span><span class="sxs-lookup"><span data-stu-id="60438-425">4</span></span> | <span data-ttu-id="60438-426">4</span><span class="sxs-lookup"><span data-stu-id="60438-426">4</span></span> |
| <span data-ttu-id="60438-427">Měsíční úplné</span><span class="sxs-lookup"><span data-stu-id="60438-427">Monthly full</span></span> |<span data-ttu-id="60438-428">StorSimple disku (dlouhodobé)</span><span class="sxs-lookup"><span data-stu-id="60438-428">StorSimple disk (long-term)</span></span> | <span data-ttu-id="60438-429">1</span><span class="sxs-lookup"><span data-stu-id="60438-429">1</span></span> | <span data-ttu-id="60438-430">12</span><span class="sxs-lookup"><span data-stu-id="60438-430">12</span></span> | <span data-ttu-id="60438-431">12</span><span class="sxs-lookup"><span data-stu-id="60438-431">12</span></span> |
| <span data-ttu-id="60438-432">Roční úplné</span><span class="sxs-lookup"><span data-stu-id="60438-432">Yearly full</span></span> |<span data-ttu-id="60438-433">StorSimple disku (dlouhodobé)</span><span class="sxs-lookup"><span data-stu-id="60438-433">StorSimple disk (long-term)</span></span> | <span data-ttu-id="60438-434">1</span><span class="sxs-lookup"><span data-stu-id="60438-434">1</span></span> | <span data-ttu-id="60438-435">1</span><span class="sxs-lookup"><span data-stu-id="60438-435">1</span></span> | <span data-ttu-id="60438-436">1</span><span class="sxs-lookup"><span data-stu-id="60438-436">1</span></span> |
|<span data-ttu-id="60438-437">Požadavek na velikost svazků GFS</span><span class="sxs-lookup"><span data-stu-id="60438-437">GFS volumes size requirement</span></span> |  |  |  | <span data-ttu-id="60438-438">18*</span><span class="sxs-lookup"><span data-stu-id="60438-438">18*</span></span>|
<span data-ttu-id="60438-439">\*Celkové kapacity zahrnuje 17 TiB StorSimple disky a 1 TiB místní svazek RAID.</span><span class="sxs-lookup"><span data-stu-id="60438-439">\* Total capacity includes 17 TiB of StorSimple disks and 1 TiB of local RAID volume.</span></span>


### <a name="gfs-example-schedule-gfs-rotation-weekly-monthly-and-yearly-schedule"></a><span data-ttu-id="60438-440">Plán GFS příklad: GFS otočení týdenní, měsíční a roční plán</span><span class="sxs-lookup"><span data-stu-id="60438-440">GFS example schedule: GFS rotation weekly, monthly, and yearly schedule</span></span>

| <span data-ttu-id="60438-441">Týden</span><span class="sxs-lookup"><span data-stu-id="60438-441">Week</span></span> | <span data-ttu-id="60438-442">Úplná</span><span class="sxs-lookup"><span data-stu-id="60438-442">Full</span></span> | <span data-ttu-id="60438-443">Přírůstkové den 1</span><span class="sxs-lookup"><span data-stu-id="60438-443">Incremental day 1</span></span> | <span data-ttu-id="60438-444">Přírůstkové den 2</span><span class="sxs-lookup"><span data-stu-id="60438-444">Incremental day 2</span></span> | <span data-ttu-id="60438-445">Přírůstkové den 3</span><span class="sxs-lookup"><span data-stu-id="60438-445">Incremental day 3</span></span> | <span data-ttu-id="60438-446">Přírůstkové den 4</span><span class="sxs-lookup"><span data-stu-id="60438-446">Incremental day 4</span></span> | <span data-ttu-id="60438-447">Přírůstkové den 5</span><span class="sxs-lookup"><span data-stu-id="60438-447">Incremental day 5</span></span> |
|---|---|---|---|---|---|---|
| <span data-ttu-id="60438-448">1 týden</span><span class="sxs-lookup"><span data-stu-id="60438-448">Week 1</span></span> | <span data-ttu-id="60438-449">Místní svazek RAID</span><span class="sxs-lookup"><span data-stu-id="60438-449">Local RAID volume</span></span>  | <span data-ttu-id="60438-450">Místní svazek RAID</span><span class="sxs-lookup"><span data-stu-id="60438-450">Local RAID volume</span></span> | <span data-ttu-id="60438-451">Místní svazek RAID</span><span class="sxs-lookup"><span data-stu-id="60438-451">Local RAID volume</span></span> | <span data-ttu-id="60438-452">Místní svazek RAID</span><span class="sxs-lookup"><span data-stu-id="60438-452">Local RAID volume</span></span> | <span data-ttu-id="60438-453">Místní svazek RAID</span><span class="sxs-lookup"><span data-stu-id="60438-453">Local RAID volume</span></span> | <span data-ttu-id="60438-454">Místní svazek RAID</span><span class="sxs-lookup"><span data-stu-id="60438-454">Local RAID volume</span></span> |
| <span data-ttu-id="60438-455">Týden 2</span><span class="sxs-lookup"><span data-stu-id="60438-455">Week 2</span></span> | <span data-ttu-id="60438-456">Týdny StorSimple 2 – 4</span><span class="sxs-lookup"><span data-stu-id="60438-456">StorSimple weeks 2-4</span></span> |   |   |   |   |   |
| <span data-ttu-id="60438-457">Týden 3</span><span class="sxs-lookup"><span data-stu-id="60438-457">Week 3</span></span> | <span data-ttu-id="60438-458">Týdny StorSimple 2 – 4</span><span class="sxs-lookup"><span data-stu-id="60438-458">StorSimple weeks 2-4</span></span> |   |   |   |   |   |
| <span data-ttu-id="60438-459">Týden 4</span><span class="sxs-lookup"><span data-stu-id="60438-459">Week 4</span></span> | <span data-ttu-id="60438-460">Týdny StorSimple 2 – 4</span><span class="sxs-lookup"><span data-stu-id="60438-460">StorSimple weeks 2-4</span></span> |   |   |   |   |   |
| <span data-ttu-id="60438-461">Měsíční</span><span class="sxs-lookup"><span data-stu-id="60438-461">Monthly</span></span> | <span data-ttu-id="60438-462">Měsíčně StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-462">StorSimple monthly</span></span> |   |   |   |   |   |
| <span data-ttu-id="60438-463">Ročně</span><span class="sxs-lookup"><span data-stu-id="60438-463">Yearly</span></span> | <span data-ttu-id="60438-464">Ročně StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-464">StorSimple yearly</span></span>  |   |   |   |   |   |   |


## <a name="assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a><span data-ttu-id="60438-465">Úlohy archivu a duplikování NetBackup přiřadit svazky zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-465">Assign StorSimple volumes to a NetBackup archive and duplication job</span></span>

<span data-ttu-id="60438-466">Protože NetBackup nabízí širokou škálu možností pro správu úložiště a média, doporučujeme obrátit se této společnosti nebo vaše NetBackup architekti správně vyhodnotit požadavky na úložiště životního cyklu zásad (SLP).</span><span class="sxs-lookup"><span data-stu-id="60438-466">Because NetBackup offers a wide range of options for storage and media management, we recommend that you consult with Veritas or your NetBackup architect to properly assess your storage lifecycle policy (SLP) requirements.</span></span>

<span data-ttu-id="60438-467">Po počáteční fondy, které jste definovali, je třeba zadat tři další úložiště zásad životního cyklu, celkem čtyři zásady:</span><span class="sxs-lookup"><span data-stu-id="60438-467">After you've defined the initial disk pools, you need to define three additional storage lifecycle policies, for a total of four policies:</span></span>
* <span data-ttu-id="60438-468">LocalRAIDVolume</span><span class="sxs-lookup"><span data-stu-id="60438-468">LocalRAIDVolume</span></span>
* <span data-ttu-id="60438-469">StorSimpleWeek2 4</span><span class="sxs-lookup"><span data-stu-id="60438-469">StorSimpleWeek2-4</span></span>
* <span data-ttu-id="60438-470">StorSimpleMonthlyFulls</span><span class="sxs-lookup"><span data-stu-id="60438-470">StorSimpleMonthlyFulls</span></span>
* <span data-ttu-id="60438-471">StorSimpleYearlyFulls</span><span class="sxs-lookup"><span data-stu-id="60438-471">StorSimpleYearlyFulls</span></span>

### <a name="to-assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a><span data-ttu-id="60438-472">Úlohy archivu a duplikování NetBackup přiřadit svazky zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-472">To assign StorSimple volumes to a NetBackup archive and duplication job</span></span>

1.  <span data-ttu-id="60438-473">V konzole pro správu NetBackup vyberte **úložiště** > **úložiště zásad životního cyklu** > **novou zásadu úložiště životního cyklu**.</span><span class="sxs-lookup"><span data-stu-id="60438-473">In the NetBackup Administration Console, select **Storage** > **Storage Lifecycle Policies** > **New Storage Lifecycle Policy**.</span></span>

    ![Konzole pro správu NetBackup, novou zásadu úložiště životního cyklu](./media/storsimple-configure-backup-target-using-netbackup/nbimage20.png)

2.  <span data-ttu-id="60438-475">Zadejte název pro snímek a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="60438-475">Enter a name for the snapshot, and then select **Add**.</span></span>

3.  <span data-ttu-id="60438-476">V **operaci nového** v dialogovém **vlastnosti** kartě pro **operace**, vyberte **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="60438-476">In the **New Operation** dialog box, on the **Properties** tab, for **Operation**, select **Backup**.</span></span> <span data-ttu-id="60438-477">Vyberte hodnoty, které chcete použít pro **cílového úložiště**, **uchování typ**, a **dobu uchování**.</span><span class="sxs-lookup"><span data-stu-id="60438-477">Select the values you want for **Destination storage**, **Retention type**, and **Retention period**.</span></span> <span data-ttu-id="60438-478">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="60438-478">Select **OK**.</span></span>

    ![Konzole pro správu NetBackup, dialogové okno Nový operace](./media/storsimple-configure-backup-target-using-netbackup/nbimage22.png)

    <span data-ttu-id="60438-480">Definuje první operace zálohování a úložiště.</span><span class="sxs-lookup"><span data-stu-id="60438-480">This defines the first backup operation and repository.</span></span>

4.  <span data-ttu-id="60438-481">Vyberte, které chcete zvýraznit předchozí operace a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="60438-481">Select to highlight the previous operation, and then select **Add**.</span></span> <span data-ttu-id="60438-482">V **operaci změnit úložiště** dialogovém okně vyberte hodnoty, které chcete použít pro **cílového úložiště**, **uchování typ**, a **dobu uchování**.</span><span class="sxs-lookup"><span data-stu-id="60438-482">In the **Change Storage Operation** dialog box, select the values you want for **Destination storage**, **Retention type**, and **Retention period**.</span></span>

    ![Konzole pro správu NetBackup, dialogové okno Změnit operace úložiště](./media/storsimple-configure-backup-target-using-netbackup/nbimage23.png)

5.  <span data-ttu-id="60438-484">Vyberte, které chcete zvýraznit předchozí operace a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="60438-484">Select to highlight the previous operation, and then select **Add**.</span></span> <span data-ttu-id="60438-485">V **novou zásadu úložiště životního cyklu** dialogové okno pole, přidejte měsíční zálohy na jeden rok.</span><span class="sxs-lookup"><span data-stu-id="60438-485">In the **New Storage Lifecycle Policy** dialog box, add monthly backups for a year.</span></span>

    ![Konzole pro správu NetBackup, dialogové okno nové zásady životního cyklu úložiště](./media/storsimple-configure-backup-target-using-netbackup/nbimage24.png)

6.  <span data-ttu-id="60438-487">Opakujte kroky 4 až 5, dokud jste vytvořili komplexní zásady uchovávání SLP, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="60438-487">Repeat steps 4-5 until you've created the comprehensive SLP retention policy that you need.</span></span>

    ![Konzole pro správu NetBackup, přidat zásady v dialogovém okně novou zásadu úložiště životního cyklu](./media/storsimple-configure-backup-target-using-netbackup/nbimage25.png)

7.  <span data-ttu-id="60438-489">Po dokončení definování zásad uchovávání informací vaší SLP, v části **zásad**, definování zásad zálohování pomocí kroky popsané v následujících [svazky přiřazení zařízení StorSimple pro úlohu zálohování NetBackup](#assigning-storsimple-volumes-to-a-netbackup-backup-job).</span><span class="sxs-lookup"><span data-stu-id="60438-489">When you are finished defining your SLP retention policy, under **Policy**, define a backup policy by following the steps detailed in [Assigning StorSimple volumes to a NetBackup backup job](#assigning-storsimple-volumes-to-a-netbackup-backup-job).</span></span>

8.  <span data-ttu-id="60438-490">V části **plány**v **změnit plán** dialogové okno, klikněte pravým tlačítkem na **úplné**a potom vyberte **změnu**.</span><span class="sxs-lookup"><span data-stu-id="60438-490">Under **Schedules**, in the **Change Schedule** dialog box, right-click **Full**, and then select **Change**.</span></span>

    ![Konzole pro správu NetBackup, dialogové okno Změnit plán](./media/storsimple-configure-backup-target-using-netbackup/nbimage26.png)

9.  <span data-ttu-id="60438-492">Vyberte **přepsat výběr úložiště zásad** zaškrtněte políčko a potom vyberte zásady uchovávání informací SLP, kterou jste vytvořili v krocích 1 až 6.</span><span class="sxs-lookup"><span data-stu-id="60438-492">Select the **Override policy storage selection** check box, and then select the SLP retention policy that you created in steps 1-6.</span></span>

    ![Konzole pro správu NetBackup, výběr úložiště zásad přepsání](./media/storsimple-configure-backup-target-using-netbackup/nbimage27.png)

10.  <span data-ttu-id="60438-494">Vyberte **OK**a potom opakujte pro plán přírůstkové zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-494">Select **OK**, and then repeat for the incremental backup schedule.</span></span>

    ![Konzole pro správu NetBackup, dialogové okno Změnit plán pro přírůstkové zálohování](./media/storsimple-configure-backup-target-using-netbackup/nbimage28.png)


| <span data-ttu-id="60438-496">Uchování typ zálohy</span><span class="sxs-lookup"><span data-stu-id="60438-496">Backup type retention</span></span> | <span data-ttu-id="60438-497">Velikost (TiB)</span><span class="sxs-lookup"><span data-stu-id="60438-497">Size (TiB)</span></span> | <span data-ttu-id="60438-498">Násobitel GFS\*</span><span class="sxs-lookup"><span data-stu-id="60438-498">GFS multiplier\*</span></span> | <span data-ttu-id="60438-499">Celková kapacita (TiB)</span><span class="sxs-lookup"><span data-stu-id="60438-499">Total capacity (TiB)</span></span>  |
|---|---|---|---|
| <span data-ttu-id="60438-500">Týdenní úplné</span><span class="sxs-lookup"><span data-stu-id="60438-500">Weekly full</span></span> |  <span data-ttu-id="60438-501">1</span><span class="sxs-lookup"><span data-stu-id="60438-501">1</span></span>  |  <span data-ttu-id="60438-502">4</span><span class="sxs-lookup"><span data-stu-id="60438-502">4</span></span> | <span data-ttu-id="60438-503">4</span><span class="sxs-lookup"><span data-stu-id="60438-503">4</span></span>  |
| <span data-ttu-id="60438-504">Každodenní přírůstkové</span><span class="sxs-lookup"><span data-stu-id="60438-504">Daily incremental</span></span>  | <span data-ttu-id="60438-505">0.5</span><span class="sxs-lookup"><span data-stu-id="60438-505">0.5</span></span>  | <span data-ttu-id="60438-506">20 (cykly jsou rovná počet týdnů v měsíci)</span><span class="sxs-lookup"><span data-stu-id="60438-506">20 (cycles are equal to the number of weeks per month)</span></span> | <span data-ttu-id="60438-507">12 (2 pro další kvóty)</span><span class="sxs-lookup"><span data-stu-id="60438-507">12 (2 for additional quota)</span></span> |
| <span data-ttu-id="60438-508">Měsíční úplné</span><span class="sxs-lookup"><span data-stu-id="60438-508">Monthly full</span></span>  | <span data-ttu-id="60438-509">1</span><span class="sxs-lookup"><span data-stu-id="60438-509">1</span></span> | <span data-ttu-id="60438-510">12</span><span class="sxs-lookup"><span data-stu-id="60438-510">12</span></span> | <span data-ttu-id="60438-511">12</span><span class="sxs-lookup"><span data-stu-id="60438-511">12</span></span> |
| <span data-ttu-id="60438-512">Roční úplné</span><span class="sxs-lookup"><span data-stu-id="60438-512">Yearly full</span></span> | <span data-ttu-id="60438-513">1</span><span class="sxs-lookup"><span data-stu-id="60438-513">1</span></span>  | <span data-ttu-id="60438-514">10</span><span class="sxs-lookup"><span data-stu-id="60438-514">10</span></span> | <span data-ttu-id="60438-515">10</span><span class="sxs-lookup"><span data-stu-id="60438-515">10</span></span> |
| <span data-ttu-id="60438-516">Požadavek GFS</span><span class="sxs-lookup"><span data-stu-id="60438-516">GFS requirement</span></span>  |     |     | <span data-ttu-id="60438-517">38</span><span class="sxs-lookup"><span data-stu-id="60438-517">38</span></span> |
| <span data-ttu-id="60438-518">Další kvóty</span><span class="sxs-lookup"><span data-stu-id="60438-518">Additional quota</span></span>  | <span data-ttu-id="60438-519">4</span><span class="sxs-lookup"><span data-stu-id="60438-519">4</span></span>  |    | <span data-ttu-id="60438-520">42 celkový požadavek GFS</span><span class="sxs-lookup"><span data-stu-id="60438-520">42 total GFS requirement</span></span> |
<span data-ttu-id="60438-521">\*Násobitel GFS je počet kopií, které potřebujete k ochraně a zachovat podle požadavků vaší zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-521">\* The GFS multiplier is the number of copies you need to protect and retain to meet your backup policy requirements.</span></span>

## <a name="storsimple-cloud-snapshots"></a><span data-ttu-id="60438-522">Cloudové snímky StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-522">StorSimple cloud snapshots</span></span>

<span data-ttu-id="60438-523">Cloudové snímky StorSimple chránit data, která se nachází v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-523">StorSimple cloud snapshots protect the data that resides in your StorSimple device.</span></span> <span data-ttu-id="60438-524">Vytvoření snímku cloudu je ekvivalentní k přesouvání místní záložní pásky do zařízení mimo lokalitu.</span><span class="sxs-lookup"><span data-stu-id="60438-524">Creating a cloud snapshot is equivalent to shipping local backup tapes to an offsite facility.</span></span> <span data-ttu-id="60438-525">Pokud používáte Azure geograficky redundantní úložiště, vytvoření cloudový snímek je ekvivalentní k přesouvání záložní pásky do více lokalit.</span><span class="sxs-lookup"><span data-stu-id="60438-525">If you use Azure geo-redundant storage, creating a cloud snapshot is equivalent to shipping backup tapes to multiple sites.</span></span> <span data-ttu-id="60438-526">Pokud budete potřebovat obnovení po havárii zařízení, můžete převést online jiná zařízení StorSimple a selhání.</span><span class="sxs-lookup"><span data-stu-id="60438-526">If you need to restore a device after a disaster, you might bring another StorSimple device online and do a failover.</span></span> <span data-ttu-id="60438-527">Po převzetí služeb při selhání budou moct pro přístup k datům (cloudu rychlostmi) z nejnovější cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="60438-527">After the failover, you would be able to access the data (at cloud speeds) from the most recent cloud snapshot.</span></span>

<span data-ttu-id="60438-528">Následující část popisuje, jak vytvořit krátký skript pro spuštění a odstranění zařízení StorSimple cloudových snímků během zálohování po zpracování.</span><span class="sxs-lookup"><span data-stu-id="60438-528">The following section describes how to create a short script to start and delete StorSimple cloud snapshots during backup post-processing.</span></span>

> [!NOTE]
> <span data-ttu-id="60438-529">Snímky vytvořené ručně nebo z programu nepostupujte podle zásad vypršení platnosti StorSimple snímku.</span><span class="sxs-lookup"><span data-stu-id="60438-529">Snapshots that are manually or programmatically created do not follow the StorSimple snapshot expiration policy.</span></span> <span data-ttu-id="60438-530">Tyto snímky musíte ručně nebo z programu smazat.</span><span class="sxs-lookup"><span data-stu-id="60438-530">These snapshots must be manually or programmatically deleted.</span></span>

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a><span data-ttu-id="60438-531">Spuštění a odstraňte snímky cloudu pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="60438-531">Start and delete cloud snapshots by using a script</span></span>

> [!NOTE]
> <span data-ttu-id="60438-532">Pečlivě vyhodnoťte dodržování předpisů a dopady uchování dat před odstraněním snímku StorSimple.</span><span class="sxs-lookup"><span data-stu-id="60438-532">Carefully assess the compliance and data retention repercussions before you delete a StorSimple snapshot.</span></span> <span data-ttu-id="60438-533">Další informace o tom, jak spustit pozálohovací skript najdete v tématu [NetBackup dokumentaci](http://www.veritas.com/docs/000094423).</span><span class="sxs-lookup"><span data-stu-id="60438-533">For more information about how to run a post-backup script, see the [NetBackup documentation](http://www.veritas.com/docs/000094423).</span></span>

### <a name="backup-lifecycle"></a><span data-ttu-id="60438-534">Zálohování životního cyklu</span><span class="sxs-lookup"><span data-stu-id="60438-534">Backup lifecycle</span></span>

![Zálohování diagram životního cyklu](./media/storsimple-configure-backup-target-using-netbackup/backuplifecycle.png)

### <a name="requirements"></a><span data-ttu-id="60438-536">Požadavky</span><span class="sxs-lookup"><span data-stu-id="60438-536">Requirements</span></span>

-   <span data-ttu-id="60438-537">Server, který spouští skript musí mít přístup k prostředkům cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="60438-537">The server that runs the script must have access to Azure cloud resources.</span></span>
-   <span data-ttu-id="60438-538">Uživatelský účet musí mít potřebná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="60438-538">The user account must have the necessary permissions.</span></span>
-   <span data-ttu-id="60438-539">Zásady zálohování StorSimple s přidružené svazky zařízení StorSimple musí nastavit ale není zapnutá.</span><span class="sxs-lookup"><span data-stu-id="60438-539">A StorSimple backup policy with the associated StorSimple volumes must be set up but not turned on.</span></span>
-   <span data-ttu-id="60438-540">Budete potřebovat název prostředku StorSimple, registrační klíč ID zařízení zásad název a zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-540">You'll need the StorSimple resource name, registration key, device name, and backup policy ID.</span></span>

### <a name="to-start-or-delete-a-cloud-snapshot"></a><span data-ttu-id="60438-541">Ke spuštění nebo odstranění snímku cloudu</span><span class="sxs-lookup"><span data-stu-id="60438-541">To start or delete a cloud snapshot</span></span>

1.  <span data-ttu-id="60438-542">[Nainstalujte prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="60438-542">[Install Azure PowerShell](/powershell/azure/overview).</span></span>
2.  <span data-ttu-id="60438-543">[Stahování a import nastavení a informace o předplatném publikovat](https://msdn.microsoft.com/library/dn385850.aspx).</span><span class="sxs-lookup"><span data-stu-id="60438-543">[Download and import publish settings and subscription information](https://msdn.microsoft.com/library/dn385850.aspx).</span></span>
3.  <span data-ttu-id="60438-544">Na portálu Azure classic, získat název prostředku a [registračního klíče služby StorSimple Manager](storsimple-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="60438-544">In the Azure classic portal, get the resource name and [registration key for your StorSimple Manager service](storsimple-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).</span></span>
4.  <span data-ttu-id="60438-545">Na serveru, který spouští skript spusťte prostředí PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="60438-545">On the server that runs the script, run PowerShell as an administrator.</span></span> <span data-ttu-id="60438-546">Zadejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="60438-546">Type this command:</span></span>

    `Get-AzureStorSimpleDeviceBackupPolicy –DeviceName <device name>`

    <span data-ttu-id="60438-547">Poznamenejte si ID zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-547">Note the backup policy ID.</span></span>
5.  <span data-ttu-id="60438-548">V poznámkovém bloku vytvořte nový skript prostředí PowerShell pomocí následující kód.</span><span class="sxs-lookup"><span data-stu-id="60438-548">In Notepad, create a new PowerShell script by using the following code.</span></span>

    <span data-ttu-id="60438-549">Zkopírujte a vložte tento fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="60438-549">Copy and paste this code snippet:</span></span>
    ```powershell
    Import-AzurePublishSettingsFile "c:\\CloudSnapshot Snapshot\\myAzureSettings.publishsettings"
    Disable-AzureDataCollection
    $ApplianceName = <myStorSimpleApplianceName>
    $RetentionInDays = 20
    $RetentionInDays = -$RetentionInDays
    $Today = Get-Date
    $ExpirationDate = $Today.AddDays($RetentionInDays)
    Select-AzureStorSimpleResource -ResourceName "myResource" –RegistrationKey
    Start-AzureStorSimpleDeviceBackupJob –DeviceName $ApplianceName -BackupType CloudSnapshot -BackupPolicyId <BackupId> -Verbose
    $CompletedSnapshots =@()
    $CompletedSnapshots = Get-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName
    Write-Host "The Expiration date is " $ExpirationDate
    Write-Host

    ForEach ($SnapShot in $CompletedSnapshots)
    {
        $SnapshotStartTimeStamp = $Snapshot.CreatedOn
        if ($SnapshotStartTimeStamp -lt $ExpirationDate)

        {
            $SnapShotInstanceID = $SnapShot.InstanceId
            Write-Host "This snpashotdate was created on " $SnapshotStartTimeStamp.Date.ToShortDateString()
            Write-Host "Instance ID " $SnapShotInstanceID
            Write-Host "This snpashotdate is older and needs to be deleted"
            Write-host "\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#"
            Remove-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName -BackupId $SnapShotInstanceID -Force -Verbose
        }
    }
    ```
      <span data-ttu-id="60438-550">Uložení skriptu prostředí PowerShell do stejného umístění, kam jste uložili vašeho Azure nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="60438-550">Save the PowerShell script to the same location where you saved your Azure publish settings.</span></span> <span data-ttu-id="60438-551">Například uložte jako C:\CloudSnapshot\StorSimpleCloudSnapshot.ps1.</span><span class="sxs-lookup"><span data-stu-id="60438-551">For example, save as C:\CloudSnapshot\StorSimpleCloudSnapshot.ps1.</span></span>
6.  <span data-ttu-id="60438-552">Přidáte skript do úlohy zálohování v NetBackup.</span><span class="sxs-lookup"><span data-stu-id="60438-552">Add the script to your backup job in NetBackup.</span></span> <span data-ttu-id="60438-553">K tomu upravte možnosti se úloha NetBackup předběžné zpracování a po zpracování příkazů.</span><span class="sxs-lookup"><span data-stu-id="60438-553">To do this, edit your NetBackup job options' pre-processing and post-processing commands.</span></span>

> [!NOTE]
> <span data-ttu-id="60438-554">Doporučujeme vám spustit zásady zálohování StorSimple cloudových snímků jako skript následného zpracování na konci každodenní úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="60438-554">We recommend that you run your StorSimple cloud snapshot backup policy as a post-processing script at the end of your daily backup job.</span></span> <span data-ttu-id="60438-555">Další informace o tom, jak zálohovat a obnovovat prostředí aplikace pro zálohování, aby vyhovovaly plánovaný bod obnovení a RTO, obraťte se na vaše zálohy architekti.</span><span class="sxs-lookup"><span data-stu-id="60438-555">For more information about how to back up and restore your backup application environment to help you meet your RPO and RTO, please consult with your backup architect.</span></span>

## <a name="storsimple-as-a-restore-source"></a><span data-ttu-id="60438-556">StorSimple jako zdroj obnovení</span><span class="sxs-lookup"><span data-stu-id="60438-556">StorSimple as a restore source</span></span>

<span data-ttu-id="60438-557">Obnoví z pracovní zařízení StorSimple jako obnovení ze všech zařízení s blokovým úložištěm.</span><span class="sxs-lookup"><span data-stu-id="60438-557">Restores from a StorSimple device work like restores from any block storage device.</span></span> <span data-ttu-id="60438-558">Rychlostmi cloudu dojde k obnovení dat, která je vrstvené do cloudu.</span><span class="sxs-lookup"><span data-stu-id="60438-558">Restores of data that is tiered to the cloud occurs at cloud speeds.</span></span> <span data-ttu-id="60438-559">Pro místní data dojde k obnovení rychlostí místní disk zařízení.</span><span class="sxs-lookup"><span data-stu-id="60438-559">For local data, restores occur at the local disk speed of the device.</span></span> <span data-ttu-id="60438-560">Informace o tom, jak provést obnovení najdete v tématu [NetBackup dokumentaci](http://www.veritas.com/docs/000094423).</span><span class="sxs-lookup"><span data-stu-id="60438-560">For information about how to perform a restore, see the [NetBackup documentation](http://www.veritas.com/docs/000094423).</span></span> <span data-ttu-id="60438-561">Doporučujeme, abyste odpovídat NetBackup obnovení osvědčené postupy.</span><span class="sxs-lookup"><span data-stu-id="60438-561">We recommend that you conform to NetBackup restore best practices.</span></span>

## <a name="storsimple-failover-and-disaster-recovery"></a><span data-ttu-id="60438-562">StorSimple převzetí služeb při selhání a zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="60438-562">StorSimple failover and disaster recovery</span></span>

> [!NOTE]
> <span data-ttu-id="60438-563">Pro scénáře cíl zálohy zařízení StorSimple cloudu nepodporuje jako cíl obnovení.</span><span class="sxs-lookup"><span data-stu-id="60438-563">For backup target scenarios, StorSimple Cloud Appliance is not supported as a restore target.</span></span>

<span data-ttu-id="60438-564">Havárie může být způsobeno různých faktorech.</span><span class="sxs-lookup"><span data-stu-id="60438-564">A disaster can be caused by a variety of factors.</span></span> <span data-ttu-id="60438-565">V následující tabulce jsou uvedeny běžné scénáře zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="60438-565">The following table lists common disaster recovery scenarios.</span></span>

| <span data-ttu-id="60438-566">Scénář</span><span class="sxs-lookup"><span data-stu-id="60438-566">Scenario</span></span> | <span data-ttu-id="60438-567">Dopad</span><span class="sxs-lookup"><span data-stu-id="60438-567">Impact</span></span> | <span data-ttu-id="60438-568">Postup obnovení</span><span class="sxs-lookup"><span data-stu-id="60438-568">How to recover</span></span> | <span data-ttu-id="60438-569">Poznámky</span><span class="sxs-lookup"><span data-stu-id="60438-569">Notes</span></span> |
|---|---|---|---|
| <span data-ttu-id="60438-570">Selhání zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-570">StorSimple device failure</span></span> | <span data-ttu-id="60438-571">Operace zálohování a obnovení jsou přerušena.</span><span class="sxs-lookup"><span data-stu-id="60438-571">Backup and restore operations are interrupted.</span></span> | <span data-ttu-id="60438-572">Nahraďte zařízení se nezdařilo a provádět [StorSimple převzetí služeb při selhání a zotavení po havárii](storsimple-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="60438-572">Replace the failed device and perform [StorSimple failover and disaster recovery](storsimple-device-failover-disaster-recovery.md).</span></span> | <span data-ttu-id="60438-573">Pokud potřebujete provést obnovení po obnovení zařízení, úplné datové sady pracovní se načítají z cloudu do nového zařízení.</span><span class="sxs-lookup"><span data-stu-id="60438-573">If you need to perform a restore after device recovery, full data working sets are retrieved from the cloud to the new device.</span></span> <span data-ttu-id="60438-574">Všechny operace jsou rychlostmi cloudu.</span><span class="sxs-lookup"><span data-stu-id="60438-574">All operations are at cloud speeds.</span></span> <span data-ttu-id="60438-575">Index a katalog opětovná kontrola procesu může dojít k všechny zálohovací sklady na kontroloval a vyžádat z vrstvy cloudu k vrstvě místní zařízení, což může být zdlouhavý proces.</span><span class="sxs-lookup"><span data-stu-id="60438-575">The index and catalog rescanning process might cause all backup sets to be scanned and pulled from the cloud tier to the local device tier, which might be a time-consuming process.</span></span> |
| <span data-ttu-id="60438-576">Selhání serveru NetBackup</span><span class="sxs-lookup"><span data-stu-id="60438-576">NetBackup server failure</span></span> | <span data-ttu-id="60438-577">Operace zálohování a obnovení jsou přerušena.</span><span class="sxs-lookup"><span data-stu-id="60438-577">Backup and restore operations are interrupted.</span></span> | <span data-ttu-id="60438-578">Znovu sestavte zálohování serveru a proveďte obnovení databáze.</span><span class="sxs-lookup"><span data-stu-id="60438-578">Rebuild the backup server and perform database restore.</span></span> | <span data-ttu-id="60438-579">Musíte znovu sestavit nebo obnovit NetBackup server na lokalitě pro obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="60438-579">You must rebuild or restore the NetBackup server at the disaster recovery site.</span></span> <span data-ttu-id="60438-580">Obnovte databázi do posledního bodu.</span><span class="sxs-lookup"><span data-stu-id="60438-580">Restore the database to the most recent point.</span></span> <span data-ttu-id="60438-581">Pokud obnovenou databázi NetBackup není synchronizované s vaší nejnovější úlohy zálohování, je třeba indexování a do katalogu.</span><span class="sxs-lookup"><span data-stu-id="60438-581">If the restored NetBackup database is not in sync with your latest backup jobs, indexing and cataloging is required.</span></span> <span data-ttu-id="60438-582">Tento index a katalog opětovná kontrola procesu může dojít k všechny zálohovací sklady na kontroloval a vyžádat z vrstvy cloudu k vrstvě místní zařízení.</span><span class="sxs-lookup"><span data-stu-id="60438-582">This index and catalog rescanning process might cause all backup sets to be scanned and pulled from the cloud tier to the local device tier.</span></span> <span data-ttu-id="60438-583">Díky tomu další časově náročný.</span><span class="sxs-lookup"><span data-stu-id="60438-583">This makes it further time-intensive.</span></span> |
| <span data-ttu-id="60438-584">Selhání lokality, který vede ke ztrátě zálohování serveru a StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-584">Site failure that results in the loss of both the backup server and StorSimple</span></span> | <span data-ttu-id="60438-585">Operace zálohování a obnovení jsou přerušena.</span><span class="sxs-lookup"><span data-stu-id="60438-585">Backup and restore operations are interrupted.</span></span> | <span data-ttu-id="60438-586">Nejprve obnovit StorSimple a potom obnovte NetBackup.</span><span class="sxs-lookup"><span data-stu-id="60438-586">Restore StorSimple first, and then restore NetBackup.</span></span> | <span data-ttu-id="60438-587">Nejprve obnovit StorSimple a potom obnovte NetBackup.</span><span class="sxs-lookup"><span data-stu-id="60438-587">Restore StorSimple first, and then restore NetBackup.</span></span> <span data-ttu-id="60438-588">Pokud potřebujete provést obnovení po obnovení zařízení, pracovní úplné datových sad se načítají z cloudu do nového zařízení.</span><span class="sxs-lookup"><span data-stu-id="60438-588">If you need to perform a restore after device recovery, the full data working sets are retrieved from the cloud to the new device.</span></span> <span data-ttu-id="60438-589">Všechny operace jsou rychlostmi cloudu.</span><span class="sxs-lookup"><span data-stu-id="60438-589">All operations are at cloud speeds.</span></span> |

## <a name="references"></a><span data-ttu-id="60438-590">Odkazy</span><span class="sxs-lookup"><span data-stu-id="60438-590">References</span></span>

<span data-ttu-id="60438-591">Pro tento článek se odkazuje v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="60438-591">The following documents were referenced for this article:</span></span>

- [<span data-ttu-id="60438-592">Funkce multipath vstupně-výstupní operace instalace zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="60438-592">StorSimple multipath I/O setup</span></span>](storsimple-configure-mpio-windows-server.md)
- [<span data-ttu-id="60438-593">Scénáře úložiště: dynamické zajišťování</span><span class="sxs-lookup"><span data-stu-id="60438-593">Storage scenarios: Thin provisioning</span></span>](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [<span data-ttu-id="60438-594">Pomocí GPT jednotky</span><span class="sxs-lookup"><span data-stu-id="60438-594">Using GPT drives</span></span>](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [<span data-ttu-id="60438-595">Nastavení stínových kopií pro sdílené složky</span><span class="sxs-lookup"><span data-stu-id="60438-595">Set up shadow copies for shared folders</span></span>](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a><span data-ttu-id="60438-596">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60438-596">Next steps</span></span>

- <span data-ttu-id="60438-597">Další informace o tom, jak [obnovení ze zálohovacího skladu](storsimple-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="60438-597">Learn more about how to [restore from a backup set](storsimple-restore-from-backup-set-u2.md).</span></span>
- <span data-ttu-id="60438-598">Další informace o tom, jak provést [zařízení převzetí služeb při selhání a zotavení po havárii](storsimple-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="60438-598">Learn more about how to perform [device failover and disaster recovery](storsimple-device-failover-disaster-recovery.md).</span></span>
