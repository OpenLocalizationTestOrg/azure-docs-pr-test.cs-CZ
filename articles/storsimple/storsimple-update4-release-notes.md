---
title: "Poznámky k verzi zařízení StorSimple 8000 řady aktualizace 4 | Microsoft Docs"
description: "Popisuje nové funkce, problémy a řešení pro zařízení StorSimple 8000 řady aktualizací 4."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 23f1bbb066c5b6481988ee841ad8979d78abf084
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-4-release-notes"></a><span data-ttu-id="89076-103">Poznámky k verzi zařízení StorSimple 8000 řady aktualizací 4</span><span class="sxs-lookup"><span data-stu-id="89076-103">StorSimple 8000 Series Update 4 release notes</span></span>

## <a name="overview"></a><span data-ttu-id="89076-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="89076-104">Overview</span></span>

<span data-ttu-id="89076-105">Následující poznámky k verzi popisují nové funkce a identifikovat kritická otevřené problémy pro StorSimple 8000 řady aktualizací 4.</span><span class="sxs-lookup"><span data-stu-id="89076-105">The following release notes describe the new features and identify the critical open issues for StorSimple 8000 Series Update 4.</span></span> <span data-ttu-id="89076-106">Také obsahují seznam aktualizací softwaru zařízení StorSimple, zahrnuté v této verzi.</span><span class="sxs-lookup"><span data-stu-id="89076-106">They also contain a list of the StorSimple software updates included in this release.</span></span> 

<span data-ttu-id="89076-107">Aktualizaci 4 můžete použít pro všechna zařízení StorSimple, s verzí (GA) nebo Update 0.1 prostřednictvím aktualizace 3.1.</span><span class="sxs-lookup"><span data-stu-id="89076-107">Update 4 can be applied to any StorSimple device running Release (GA) or Update 0.1 through Update 3.1.</span></span> <span data-ttu-id="89076-108">Zařízení verzi spojenou s aktualizací 4 je 6.3.9600.17820.</span><span class="sxs-lookup"><span data-stu-id="89076-108">The device version associated with Update 4 is 6.3.9600.17820.</span></span>

<span data-ttu-id="89076-109">Přečtěte si informace obsažené v poznámkách k verzi před nasazením aktualizace v řešení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="89076-109">Please review the information contained in the release notes before you deploy the update in your StorSimple solution.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="89076-110">Aktualizace 4 obsahuje software zařízení, Seznam USM firmwaru, LSI ovladače a firmware, firmwaru disku, Storport a Spaceport, zabezpečení a jiné aktualizace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="89076-110">Update 4 has device software, USM firmware, LSI driver and firmware, disk firmware, Storport and Spaceport, security, and other OS updates.</span></span> <span data-ttu-id="89076-111">Jak dlouho trvá přibližně 4 hodiny nainstalovat tuto aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="89076-111">It takes approximately 4 hours to install this update.</span></span> <span data-ttu-id="89076-112">Aktualizace firmwaru disku je rušivý aktualizace a výsledkem výpadku pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="89076-112">Disk firmware update is a disruptive update and results in a downtime for your device.</span></span> <span data-ttu-id="89076-113">Doporučujeme, abyste použili aktualizaci 4 k zachování aktualizovaného stavu zařízení.</span><span class="sxs-lookup"><span data-stu-id="89076-113">We recommend that you apply Update 4 to keep your device up-to-date.</span></span> 
> * <span data-ttu-id="89076-114">Pro nové verze, nemusí zobrazit aktualizace okamžitě, protože provedeme postupné zavádění aktualizací.</span><span class="sxs-lookup"><span data-stu-id="89076-114">For new releases, you may not see updates immediately because we do a phased rollout of the updates.</span></span> <span data-ttu-id="89076-115">Počkejte několik dní, a pak vyhledejte aktualizace znovu jako tyto brzy bude dostupná.</span><span class="sxs-lookup"><span data-stu-id="89076-115">Wait a few days, and then scan for updates again as these will become available soon.</span></span>

## <a name="whats-new-in-update-4"></a><span data-ttu-id="89076-116">Co je nového v aktualizaci 4</span><span class="sxs-lookup"><span data-stu-id="89076-116">What's new in Update 4</span></span>

<span data-ttu-id="89076-117">V aktualizaci 4 byly provedeny následující klíčových vylepšení a opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="89076-117">The following key improvements and bug fixes have been made in Update 4.</span></span>

* <span data-ttu-id="89076-118">**Chytřejší automatizované algoritmy recyklace místa** – v aktualizaci 4, recyklaci automatizované místa algoritmy líp upravit cyklů recyklaci místa podle očekávané regenerovaný místo k dispozici v cloudu.</span><span class="sxs-lookup"><span data-stu-id="89076-118">**Smarter automated space reclamation algorithms** – In Update 4, the automated space reclamation algorithms are enhanced to adjust the space reclamation cycles based on the expected reclaimed space available in the cloud.</span></span> 

* <span data-ttu-id="89076-119">**Vylepšení výkonu pro místně vázaných svazků** – aktualizace 4 je vylepšený výkon místně vázaných svazků ve scénářích, které mají přijímat vysoké data (data srovnatelná velikosti svazku).</span><span class="sxs-lookup"><span data-stu-id="89076-119">**Performance enhancements for locally pinned volumes** – Update 4 has improved the performance of locally pinned volumes in scenarios that have high data ingestion (data comparable to volume size).</span></span>

* <span data-ttu-id="89076-120">**Obnovení na základě Heatmap** -ve starších verzích, následující zotavení po havárii (DR), data byla obnovena z cloudu podle vzorů přístupu, výsledkem je nízký výkon.</span><span class="sxs-lookup"><span data-stu-id="89076-120">**Heatmap-based restore** - In the earlier releases, following a disaster recovery (DR), the data was restored from the cloud based on the access patterns resulting in a slow performance.</span></span> 

    <span data-ttu-id="89076-121">Nová funkce je implementována v aktualizaci 4 sleduje často používaná data a vytvořit heatmap, když je zařízení používá před zotavení po Havárii (nejčastěji používaných bloky dat mají vysokou heat, zatímco menší bloky dat používá mít nízkou heat).</span><span class="sxs-lookup"><span data-stu-id="89076-121">A new feature is implemented in Update 4 that tracks frequently accessed data to create a heatmap when the device is in use prior to DR (Most used data chunks have high heat whereas less used chunks have low heat).</span></span> <span data-ttu-id="89076-122">Po zotavení po Havárii používá StorSimple heatmap pro automatické obnovení a rehydrataci při spotřebě data z cloudu.</span><span class="sxs-lookup"><span data-stu-id="89076-122">After DR, StorSimple uses the heatmap to automatically restore and rehydrate the data from the cloud.</span></span> 

    <span data-ttu-id="89076-123">Všechny obnovení jsou nyní heatmap na základě obnovení.</span><span class="sxs-lookup"><span data-stu-id="89076-123">All the restores are now heatmap based restores.</span></span> <span data-ttu-id="89076-124">Další informace o tom, jak dotaz a zrušení úloh obnovení a rehydrataci heatmap na základě, přejděte na [prostředí Windows PowerShell pro referenční informace o rutinách StorSimple](https://technet.microsoft.com/library/dn688168.aspx).</span><span class="sxs-lookup"><span data-stu-id="89076-124">For more information on how to query and cancel heatmap based restore and rehydration jobs, go to [Windows PowerShell for StorSimple cmdlet reference](https://technet.microsoft.com/library/dn688168.aspx).</span></span>

* <span data-ttu-id="89076-125">**Nástroj diagnostiky StorSimple** – v aktualizaci 4, nástroj pro diagnostiku StorSimple je vydán umožňující snadno diagnostice a řešení potíží s problémy související se stav součásti systému, sítě, výkonu a hardwaru.</span><span class="sxs-lookup"><span data-stu-id="89076-125">**StorSimple Diagnostics tool** – In Update 4, a StorSimple Diagnostics tool is being released to allow for easy diagnosing and troubleshooting of issues related to system, network, performance, and hardware component health.</span></span> <span data-ttu-id="89076-126">Tento nástroj je spuštěn prostřednictvím Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="89076-126">This tool is run via the Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="89076-127">Další informace, přejděte na [řešení problémů pomocí diagnostiky StorSimple nástroj](storsimple-8000-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="89076-127">For more information, go to [troubleshoot using StorSimple Diagnostics tool](storsimple-8000-diagnostics.md).</span></span>

* <span data-ttu-id="89076-128">**Na základě uživatelského rozhraní nástroj pro migraci StorSimple** -před touto verzí migraci dat z řady 5000 7000 požadované uživatelům provést část pracovní postup migrace pomocí rozhraní Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="89076-128">**UI-based StorSimple Migration tool** - Prior to this release, migration of data from 5000-7000 series required the users to execute a part of the migration workflow using the Azure PowerShell interface.</span></span> <span data-ttu-id="89076-129">V této verzi je k dispozici pro podporu, aby se usnadnilo stejný pracovní postup migrace nástroj Migrace StorSimple na základě uživatelského rozhraní na snadno použitelné.</span><span class="sxs-lookup"><span data-stu-id="89076-129">In this release, an easy-to-use UI-based StorSimple Migration tool is made available for Support to facilitate the same migration workflow.</span></span> <span data-ttu-id="89076-130">Tento nástroj by také umožňují konsolidace intervalů obnovení.</span><span class="sxs-lookup"><span data-stu-id="89076-130">This tool would also allow for the consolidation of recovery buckets.</span></span> 

* <span data-ttu-id="89076-131">**Změny související s FIPS** – toto vydání a vyšší, FIPS ve výchozím na všechna zařízení řady StorSimple 8000 je povolený pro Microsoft Azure Government i Azure veřejných cloudů, účty.</span><span class="sxs-lookup"><span data-stu-id="89076-131">**FIPS-related changes** - This release onwards, FIPS is enabled by default on all the StorSimple 8000 series devices for both the Microsoft Azure Government and Azure public cloud accounts.</span></span>

* <span data-ttu-id="89076-132">**Aktualizovat změny** – v této verzi bylo opraveno chyby související s aktualizovat selhání.</span><span class="sxs-lookup"><span data-stu-id="89076-132">**Update changes** - In this release, bugs related to update failures have been fixed.</span></span>

* <span data-ttu-id="89076-133">**Výstrahy pro selhání disku** – v této verzi se přidá nová výstraha s upozorněním, uživatel brzké selhání disku.</span><span class="sxs-lookup"><span data-stu-id="89076-133">**Alert for disk failures** - A new alert that warns the user of impending disk failures is added in this release.</span></span> <span data-ttu-id="89076-134">Pokud narazíte na tuto výstrahu, obraťte se na Microsoft Support pro odeslání náhradní disk.</span><span class="sxs-lookup"><span data-stu-id="89076-134">If you encounter this alert, contact Microsoft Support to ship a replacement disk.</span></span> <span data-ttu-id="89076-135">Další informace, přejděte na [hardwaru výstrahy na zařízení StorSimple](storsimple-manage-alerts.md#hardware-alerts).</span><span class="sxs-lookup"><span data-stu-id="89076-135">For more information, go to [hardware alerts on your StorSimple device](storsimple-manage-alerts.md#hardware-alerts).</span></span>

* <span data-ttu-id="89076-136">**Změny nahrazení řadiče** – přidání rutinu, která umožňuje uživatelům zjistit stav procesu nahrazení řadiče v této verzi.</span><span class="sxs-lookup"><span data-stu-id="89076-136">**Controller replacement changes** - A cmdlet that allows the user to query the status of the controller replacement process is added in this release.</span></span> <span data-ttu-id="89076-137">Další informace naleznete [rutiny zjistit stav nahrazení řadiče](https://technet.microsoft.com/library/dn688168.aspx).</span><span class="sxs-lookup"><span data-stu-id="89076-137">For more information, go to the [cmdlet to query controller replacement status](https://technet.microsoft.com/library/dn688168.aspx).</span></span>


## <a name="issues-fixed-in-update-4"></a><span data-ttu-id="89076-138">Chyby v aktualizaci 4</span><span class="sxs-lookup"><span data-stu-id="89076-138">Issues fixed in Update 4</span></span>

<span data-ttu-id="89076-139">Následující tabulka obsahuje souhrn problémy, které jsme vyřešili v aktualizaci 4.</span><span class="sxs-lookup"><span data-stu-id="89076-139">The following table provides a summary of issues that were fixed in Update 4.</span></span>    

| <span data-ttu-id="89076-140">Ne</span><span class="sxs-lookup"><span data-stu-id="89076-140">No</span></span> | <span data-ttu-id="89076-141">Funkce</span><span class="sxs-lookup"><span data-stu-id="89076-141">Feature</span></span> | <span data-ttu-id="89076-142">Problém</span><span class="sxs-lookup"><span data-stu-id="89076-142">Issue</span></span> | <span data-ttu-id="89076-143">Platí pro fyzické zařízení</span><span class="sxs-lookup"><span data-stu-id="89076-143">Applies to physical device</span></span> | <span data-ttu-id="89076-144">Platí pro virtuální zařízení</span><span class="sxs-lookup"><span data-stu-id="89076-144">Applies to virtual device</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="89076-145">1</span><span class="sxs-lookup"><span data-stu-id="89076-145">1</span></span> |<span data-ttu-id="89076-146">Převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="89076-146">Failover</span></span> |<span data-ttu-id="89076-147">Ve starší verzi po převzetí služeb při selhání, se problém související s čištění zjištěnými na straně zákazníka.</span><span class="sxs-lookup"><span data-stu-id="89076-147">In the earlier release, after the failover, there was an issue related to cleanup observed at the customer site.</span></span> <span data-ttu-id="89076-148">Tento problém vyřešen v této verzi.</span><span class="sxs-lookup"><span data-stu-id="89076-148">This issue is fixed in this release.</span></span> |<span data-ttu-id="89076-149">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-149">Yes</span></span> |<span data-ttu-id="89076-150">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-150">Yes</span></span> |
| <span data-ttu-id="89076-151">2</span><span class="sxs-lookup"><span data-stu-id="89076-151">2</span></span> |<span data-ttu-id="89076-152">Místně vázaných svazků</span><span class="sxs-lookup"><span data-stu-id="89076-152">Locally pinned volumes</span></span> |<span data-ttu-id="89076-153">V předchozí verzi došlo problém související svazku vytváření místně vázaných svazků, které by způsobilo chyby při vytváření svazku.</span><span class="sxs-lookup"><span data-stu-id="89076-153">In the previous release, there was an issue to related volume creation for locally pinned volumes that would result in volume creation failures.</span></span> <span data-ttu-id="89076-154">Tento problém byl způsobeno kořenové a pevné v této verzi.</span><span class="sxs-lookup"><span data-stu-id="89076-154">This issue was root-caused and fixed in this release.</span></span> |<span data-ttu-id="89076-155">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-155">Yes</span></span> |<span data-ttu-id="89076-156">Ne</span><span class="sxs-lookup"><span data-stu-id="89076-156">No</span></span> |
| <span data-ttu-id="89076-157">3</span><span class="sxs-lookup"><span data-stu-id="89076-157">3</span></span> |<span data-ttu-id="89076-158">Balíček pro podporu</span><span class="sxs-lookup"><span data-stu-id="89076-158">Support package</span></span> |<span data-ttu-id="89076-159">V předchozí verzi byly problémy související s podporu balíček, který by způsobilo System.OutOfMemory výjimky nebo jiné chyby, které vedly k nedodržení vytvoření balíčku podpory.</span><span class="sxs-lookup"><span data-stu-id="89076-159">In previous release, there were issues related to Support package that would result in a System.OutOfMemory exception or other errors resulting in a Support package creation failure.</span></span> <span data-ttu-id="89076-160">Tyto chyby budou opraveny v této verzi.</span><span class="sxs-lookup"><span data-stu-id="89076-160">These bugs are fixed in this release.</span></span> |<span data-ttu-id="89076-161">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-161">Yes</span></span> |<span data-ttu-id="89076-162">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-162">Yes</span></span> |
| <span data-ttu-id="89076-163">4</span><span class="sxs-lookup"><span data-stu-id="89076-163">4</span></span> |<span data-ttu-id="89076-164">Monitorování</span><span class="sxs-lookup"><span data-stu-id="89076-164">Monitoring</span></span> |<span data-ttu-id="89076-165">V předchozí verzi existuje problém související s monitorování grafy pro místně připnuli svazky kde byl spotřeby ukazuje EB.</span><span class="sxs-lookup"><span data-stu-id="89076-165">In previous release, there an issue related to monitoring charts for locally pinned volumes where consumption was shown in EB.</span></span> <span data-ttu-id="89076-166">Tato chyba se vyřeší v této verzi.</span><span class="sxs-lookup"><span data-stu-id="89076-166">This bug is resolved in this release.</span></span> |<span data-ttu-id="89076-167">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-167">Yes</span></span> |<span data-ttu-id="89076-168">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-168">Yes</span></span> |
| <span data-ttu-id="89076-169">5</span><span class="sxs-lookup"><span data-stu-id="89076-169">5</span></span> |<span data-ttu-id="89076-170">Migrace</span><span class="sxs-lookup"><span data-stu-id="89076-170">Migration</span></span> |<span data-ttu-id="89076-171">V předchozí verzi nebyly k dispozici několik problémy související s spolehlivost migrace z řady 5000 7000 zařízení řady 8000.</span><span class="sxs-lookup"><span data-stu-id="89076-171">In previous release, there were several issues related to the reliability of migration from 5000-7000 series to 8000 series devices.</span></span> <span data-ttu-id="89076-172">Mít byly tyto problémy řešeny v této verzi.</span><span class="sxs-lookup"><span data-stu-id="89076-172">These issues have been addressed in this release.</span></span> |<span data-ttu-id="89076-173">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-173">Yes</span></span> |<span data-ttu-id="89076-174">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-174">Yes</span></span> |
| <span data-ttu-id="89076-175">6</span><span class="sxs-lookup"><span data-stu-id="89076-175">6</span></span> |<span data-ttu-id="89076-176">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="89076-176">Update</span></span> |<span data-ttu-id="89076-177">V předchozích verzích, pokud došlo k chybě aktualizace, pak se řadiče přejde do režimu obnovení a proto uživatele nelze pokračovat v aktualizaci a by bylo potřeba kontaktovat Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="89076-177">In previous releases, if there was an update failure, the controllers would go into recovery mode and hence the user could not proceed with the update and would need to contact Microsoft Support.</span></span> <br> <span data-ttu-id="89076-178">Toto chování se změnilo v této verzi.</span><span class="sxs-lookup"><span data-stu-id="89076-178">This behavior was changed in this release.</span></span> <span data-ttu-id="89076-179">Pokud má uživatel k selhání aktualizace po oběma řadičům běží stejná verze (aktualizace 4), pak se řadiče nepřecházejí do režimu obnovení.</span><span class="sxs-lookup"><span data-stu-id="89076-179">If the user has an update failure after both the controllers are running the same version (Update 4), the controllers do not go into recovery mode.</span></span> <span data-ttu-id="89076-180">Pokud uživatel zaznamená této chybě, doporučujeme, abyste chvilku počkejte a pak zkuste znovu aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="89076-180">If the user encounters this failure, we recommend that they wait for a bit and then retry the update.</span></span> <span data-ttu-id="89076-181">Opakovaném může být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="89076-181">The retry could succeed.</span></span> <span data-ttu-id="89076-182">V případě selhání opakovaném se měli kontaktovat Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="89076-182">If the retry fails, then they should contact Microsoft Support.</span></span> |<span data-ttu-id="89076-183">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-183">Yes</span></span> |<span data-ttu-id="89076-184">Ano</span><span class="sxs-lookup"><span data-stu-id="89076-184">Yes</span></span> |


## <a name="known-issues-in-update-4-from-previous-releases"></a><span data-ttu-id="89076-185">Známé problémy v aktualizaci 4 z předchozích verzí</span><span class="sxs-lookup"><span data-stu-id="89076-185">Known issues in Update 4 from previous releases</span></span>

<span data-ttu-id="89076-186">Nejsou žádné nové známé problémy v aktualizaci 4.</span><span class="sxs-lookup"><span data-stu-id="89076-186">There are no new known issues in Update 4.</span></span> <span data-ttu-id="89076-187">Pro seznam problémů, které přenášejí do aktualizace 4 z předchozích verzí, přejděte na [poznámky k verzi Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).</span><span class="sxs-lookup"><span data-stu-id="89076-187">For a list of issues carried over to Update 4 from previous releases, go to [Update 3 release notes](storsimple-update3-release-notes.md#known-issues-in-update-3).</span></span>

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-4"></a><span data-ttu-id="89076-188">Serial-attached SCSI (SAS) řadiče a aktualizace firmwaru v aktualizaci 4</span><span class="sxs-lookup"><span data-stu-id="89076-188">Serial-attached SCSI (SAS) controller and firmware updates in Update 4</span></span>

<span data-ttu-id="89076-189">Tato verze obsahuje řadič SAS a LSI ovladače a firmware aktualizace.</span><span class="sxs-lookup"><span data-stu-id="89076-189">This release has SAS controller and LSI driver and firmware updates.</span></span> <span data-ttu-id="89076-190">Další informace o tom, jak tyto aktualizace nainstalujete najdete v tématu [nainstalovat aktualizace 4](storsimple-install-update-4.md) zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="89076-190">For more information on how to install these updates, see [install Update 4](storsimple-install-update-4.md) on your StorSimple device.</span></span>

## <a name="virtual-device-updates-in-update-4"></a><span data-ttu-id="89076-191">Aktualizace virtuální zařízení v aktualizaci 4</span><span class="sxs-lookup"><span data-stu-id="89076-191">Virtual device updates in Update 4</span></span>

<span data-ttu-id="89076-192">Tuto aktualizaci nelze nainstalovat do cloudu zařízení StorSimple (také označované jako virtuální zařízení).</span><span class="sxs-lookup"><span data-stu-id="89076-192">This update cannot be applied to the StorSimple Cloud Appliance (also known as the virtual device).</span></span> <span data-ttu-id="89076-193">Nové virtuální zařízení bude potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="89076-193">New virtual devices will need to be created.</span></span> 

## <a name="next-step"></a><span data-ttu-id="89076-194">Další krok</span><span class="sxs-lookup"><span data-stu-id="89076-194">Next step</span></span>

<span data-ttu-id="89076-195">Zjistěte, jak [nainstalovat aktualizace 4](storsimple-install-update-4.md) zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="89076-195">Learn how to [install Update 4](storsimple-install-update-4.md) on your StorSimple device.</span></span>
