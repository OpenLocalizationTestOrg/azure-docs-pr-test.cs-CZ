---
title: "Poznámky k verzi 0,5 aktualizace pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje důležité otevřené problémy a řešení pro pole virtuální zařízení StorSimple spuštění Update 0,5."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/08/2017
ms.author: alkohli
ms.openlocfilehash: 4d020ff2b998da4cb52fe91e4d7d4b93544965a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-virtual-array-update-05-release-notes"></a><span data-ttu-id="8a9cd-103">Poznámky k verzi 0,5 aktualizace pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="8a9cd-103">StorSimple Virtual Array Update 0.5 release notes</span></span>

## <a name="overview"></a><span data-ttu-id="8a9cd-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8a9cd-104">Overview</span></span>

<span data-ttu-id="8a9cd-105">Následující poznámky k verzi identifikovat kritická otevřené problémy a vyřešit problémy pro Microsoft Azure StorSimple virtuální pole aktualizace.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-105">The following release notes identify the critical open issues and the resolved issues for Microsoft Azure StorSimple Virtual Array updates.</span></span>

<span data-ttu-id="8a9cd-106">Poznámky k verzi se průběžně aktualizuje a při zjištění zásadních problémů, které vyžadují řešení, se řešení přidá.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-106">The release notes are continuously updated, and as critical issues requiring a workaround are discovered, they are added.</span></span> <span data-ttu-id="8a9cd-107">Před nasazením pole virtuální zařízení StorSimple, pečlivě zkontrolujte informace obsažené v poznámkách k verzi.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-107">Before you deploy your StorSimple Virtual Array, carefully review the information contained in the release notes.</span></span>

<span data-ttu-id="8a9cd-108">Aktualizace 0,5 odpovídá verzi softwaru **10.0.10290.0**.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-108">Update 0.5 corresponds to the software version **10.0.10290.0**.</span></span>

> [!NOTE]
> <span data-ttu-id="8a9cd-109">Aktualizace jsou rušivý a zařízení restartovat.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-109">Updates are disruptive and restart your device.</span></span> <span data-ttu-id="8a9cd-110">Pokud vstupně-výstupní operace probíhá, zařízení způsobuje výpadek.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-110">If I/O are in progress, the device incurs downtime.</span></span> <span data-ttu-id="8a9cd-111">Podrobné pokyny o tom, jak použít aktualizaci, přejděte na [nainstalujte aktualizaci 0,5](storsimple-virtual-array-install-update-05.md).</span><span class="sxs-lookup"><span data-stu-id="8a9cd-111">For detailed instructions on how to apply the update, go to [Install Update 0.5](storsimple-virtual-array-install-update-05.md).</span></span>


## <a name="whats-new-in-the-update-05"></a><span data-ttu-id="8a9cd-112">Co je nového v aktualizaci 0,5</span><span class="sxs-lookup"><span data-stu-id="8a9cd-112">What's new in the Update 0.5</span></span>
<span data-ttu-id="8a9cd-113">Aktualizace 0,5 je primárně oprava chyby sestavení.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-113">Update 0.5 is primarily a bug-fix build.</span></span> <span data-ttu-id="8a9cd-114">Hlavní vylepšení a opravy chyb jsou následující:</span><span class="sxs-lookup"><span data-stu-id="8a9cd-114">The main enhancements and bug-fixes are as follows:</span></span>

- <span data-ttu-id="8a9cd-115">**Zálohování odolnosti vylepšení** – tato verze obsahuje opravy, které zlepšují odolnost zálohování.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-115">**Backup resiliency improvements** - This release has fixes that improve the backup resiliency.</span></span> <span data-ttu-id="8a9cd-116">V dřívějších verzích byly opakovány zálohování pouze pro určité výjimky.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-116">In the earlier releases, backups were retried only for certain exceptions.</span></span> <span data-ttu-id="8a9cd-117">Tato verze opakování zálohování výjimky a umožňuje pružnější zálohy.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-117">This release retries all the backup exceptions and makes the backups more resilient.</span></span>

- <span data-ttu-id="8a9cd-118">**Aktualizace pro sledování využití úložiště** -spuštění 30. června 2017, úložiště využití monitorování pro virtuální zařízení řady StorSimple vyřadí.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-118">**Updates for storage usage monitoring** - Starting 30 June 2017, the storage usage monitoring for StorSimple Virtual Device Series will be retired.</span></span> <span data-ttu-id="8a9cd-119">To platí pro monitorování grafy na všechny virtuální pole verzi Update 0,4 nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-119">This applies to the monitoring charts on all the virtual arrays running Update 0.4 or lower.</span></span> <span data-ttu-id="8a9cd-120">Tato aktualizace obsahuje změny, které potřebujete, chcete-li pokračovat v používání monitorování využití úložiště na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-120">This update contains the changes required for you to continue the use of storage usage monitoring in the Azure portal.</span></span> <span data-ttu-id="8a9cd-121">Nainstalujte tuto aktualizaci před 30. června 2017 chcete pokračovat v používání funkce monitorování.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-121">Install this critical update before June 30, 2017 to continue using the monitoring feature.</span></span>


## <a name="issues-fixed-in-the-update-05"></a><span data-ttu-id="8a9cd-122">Chyby v aktualizaci 0,5</span><span class="sxs-lookup"><span data-stu-id="8a9cd-122">Issues fixed in the Update 0.5</span></span>

<span data-ttu-id="8a9cd-123">Následující tabulka obsahuje souhrn chyby v této verzi.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-123">The following table provides a summary of issues fixed in this release.</span></span>

| <span data-ttu-id="8a9cd-124">Ne.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-124">No.</span></span> | <span data-ttu-id="8a9cd-125">Funkce</span><span class="sxs-lookup"><span data-stu-id="8a9cd-125">Feature</span></span> | <span data-ttu-id="8a9cd-126">Problém</span><span class="sxs-lookup"><span data-stu-id="8a9cd-126">Issue</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8a9cd-127">1</span><span class="sxs-lookup"><span data-stu-id="8a9cd-127">1</span></span> |<span data-ttu-id="8a9cd-128">Odolnost proti chybám zálohování</span><span class="sxs-lookup"><span data-stu-id="8a9cd-128">Backup resiliency</span></span>| <span data-ttu-id="8a9cd-129">V dřívějších verzích byly opakovány zálohování pouze pro určité výjimky.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-129">In the earlier releases, backups were retried only for certain exceptions.</span></span> <span data-ttu-id="8a9cd-130">Tato verze obsahuje opravu, aby zálohování pružnější opakováním zálohování výjimky.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-130">This release contains a fix to make backups more resilient by retrying all the backup exceptions.</span></span>|
| <span data-ttu-id="8a9cd-131">2</span><span class="sxs-lookup"><span data-stu-id="8a9cd-131">2</span></span> |<span data-ttu-id="8a9cd-132">Monitorování</span><span class="sxs-lookup"><span data-stu-id="8a9cd-132">Monitoring</span></span>| <span data-ttu-id="8a9cd-133">Monitorování využití úložiště pro virtuální zařízení řady StorSimple bude zastaralé od 30. června 2017.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-133">The storage usage monitoring for StorSimple Virtual Device Series will be deprecated starting June 30, 2017.</span></span> <span data-ttu-id="8a9cd-134">Tato akce ovlivní monitorování grafy ve službě StorSimple Manager zařízení systémem pole virtuální zařízení StorSimple (1200 modelu).</span><span class="sxs-lookup"><span data-stu-id="8a9cd-134">This action impacts the monitoring charts on the StorSimple Device Manager service running on StorSimple Virtual Arrays (1200 model).</span></span> <span data-ttu-id="8a9cd-135">Tato verze obsahuje aktualizace, které uživateli umožní pokračovat v používání monitorování využití úložiště na virtuální pole nad rámec 30. června 2017.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-135">This release has updates that allow the user to continue the use of storage usage monitoring on the virtual arrays beyond June 30, 2017.</span></span>|
| <span data-ttu-id="8a9cd-136">3</span><span class="sxs-lookup"><span data-stu-id="8a9cd-136">3</span></span> |<span data-ttu-id="8a9cd-137">Souborový server</span><span class="sxs-lookup"><span data-stu-id="8a9cd-137">File server</span></span>| <span data-ttu-id="8a9cd-138">V dřívějších verzích uživatel může omylem zkopírovat šifrované soubory do virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-138">In the earlier releases, a user could mistakenly copy encrypted files to the virtual array.</span></span> <span data-ttu-id="8a9cd-139">Tato verze obsahuje opravu, která nepovolí kopírování šifrovaných souborů na virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-139">This release contains a fix that would not allow copying of encrypted files to virtual array.</span></span> <span data-ttu-id="8a9cd-140">Pokud vaše zařízení má existující šifrované soubory před aktualizací, zálohování budou nadále fungovat, dokud nebude šifrované soubory se odstraní ze systému.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-140">If your device has existing encrypted files prior to the update, backups will continue to fail until all the encrypted files are deleted from the system.</span></span> |


## <a name="known-issues-in-the-update-05"></a><span data-ttu-id="8a9cd-141">Známé problémy v aktualizaci 0,5</span><span class="sxs-lookup"><span data-stu-id="8a9cd-141">Known issues in the Update 0.5</span></span>

<span data-ttu-id="8a9cd-142">Následující tabulka obsahuje souhrn známé problémy pro pole virtuální zařízení StorSimple a zahrnuje problémy uvedené verze z předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-142">The following table provides a summary of known issues for the StorSimple Virtual Array and includes the issues release-noted from the previous releases.</span></span>

| <span data-ttu-id="8a9cd-143">Ne.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-143">No.</span></span> | <span data-ttu-id="8a9cd-144">Funkce</span><span class="sxs-lookup"><span data-stu-id="8a9cd-144">Feature</span></span> | <span data-ttu-id="8a9cd-145">Problém</span><span class="sxs-lookup"><span data-stu-id="8a9cd-145">Issue</span></span> | <span data-ttu-id="8a9cd-146">Alternativní řešení a komentář</span><span class="sxs-lookup"><span data-stu-id="8a9cd-146">Workaround/comments</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8a9cd-147">**1.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-147">**1.**</span></span> |<span data-ttu-id="8a9cd-148">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="8a9cd-148">Updates</span></span> |<span data-ttu-id="8a9cd-149">Virtuální zařízení vytvořené ve verzi preview nelze aktualizovat na podporovanou verzi obecné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-149">The virtual devices created in the preview release cannot be updated to a supported General Availability version.</span></span> |<span data-ttu-id="8a9cd-150">Tato virtuální zařízení musí převzetí služeb při selhání pro verzi obecné dostupnosti pomocí pracovního postupu zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-150">These virtual devices must be failed over for the General Availability release using a disaster recovery (DR) workflow.</span></span> |
| <span data-ttu-id="8a9cd-151">**2.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-151">**2.**</span></span> |<span data-ttu-id="8a9cd-152">Zřízené datový disk</span><span class="sxs-lookup"><span data-stu-id="8a9cd-152">Provisioned data disk</span></span> |<span data-ttu-id="8a9cd-153">Jakmile máte zřízen datový disk určité zadané velikosti a vytvořit odpovídající virtuální zařízení StorSimple, nesmí zvětšení nebo zmenšení datový disk.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-153">Once you have provisioned a data disk of a certain specified size and created the corresponding StorSimple virtual device, you must not expand or shrink the data disk.</span></span> <span data-ttu-id="8a9cd-154">Probíhá pokus o provést výsledky ke ztrátě všech dat v místních vrstvách zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-154">Attempting to do results in a loss of all the data in the local tiers of the device.</span></span> | |
| <span data-ttu-id="8a9cd-155">**3.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-155">**3.**</span></span> |<span data-ttu-id="8a9cd-156">Zásady skupiny</span><span class="sxs-lookup"><span data-stu-id="8a9cd-156">Group policy</span></span> |<span data-ttu-id="8a9cd-157">Když je zařízení připojené k doméně, použití zásad skupiny může nepříznivě ovlivnit činnost zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-157">When a device is domain-joined, applying a group policy can adversely affect the device operation.</span></span> |<span data-ttu-id="8a9cd-158">Zkontrolujte, zda je vaše virtuální pole ve vlastní organizační jednotku (OU) pro službu Active Directory a žádné objekty zásad skupiny (GPO) se použijí k němu.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-158">Ensure that your virtual array is in its own organizational unit (OU) for Active Directory and no group policy objects (GPO) are applied to it.</span></span> |
| <span data-ttu-id="8a9cd-159">**4.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-159">**4.**</span></span> |<span data-ttu-id="8a9cd-160">Místní webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="8a9cd-160">Local web UI</span></span> |<span data-ttu-id="8a9cd-161">Pokud funkce Rozšířené zabezpečení jsou povolené v aplikaci Internet Explorer (IE ESC), některé z uživatelského rozhraní místní webové stránky, jako je například údržby nebo Poradce při potížích s nemusí fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-161">If enhanced security features are enabled in Internet Explorer (IE ESC), some local web UI pages such as Troubleshooting or Maintenance may not work properly.</span></span> <span data-ttu-id="8a9cd-162">Tlačítka na těchto stránkách nemusí fungovat.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-162">Buttons on these pages may also not work.</span></span> |<span data-ttu-id="8a9cd-163">Vypněte funkce rozšířeného zabezpečení aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-163">Turn off enhanced security features in Internet Explorer.</span></span> |
| <span data-ttu-id="8a9cd-164">**5.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-164">**5.**</span></span> |<span data-ttu-id="8a9cd-165">Místní webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="8a9cd-165">Local web UI</span></span> |<span data-ttu-id="8a9cd-166">Ve virtuálním počítači technologie Hyper-V rozhraní sítě v webového uživatelského rozhraní se zobrazí jako 10 GB/s rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-166">In a Hyper-V virtual machine, the network interfaces in the web UI are displayed as 10 Gbps interfaces.</span></span> |<span data-ttu-id="8a9cd-167">Toto chování je odraz technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-167">This behavior is a reflection of Hyper-V.</span></span> <span data-ttu-id="8a9cd-168">Technologie Hyper-V vždy zobrazuje 10 GB/s pro virtuální síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-168">Hyper-V always shows 10 Gbps for virtual network adapters.</span></span> |
| <span data-ttu-id="8a9cd-169">**6.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-169">**6.**</span></span> |<span data-ttu-id="8a9cd-170">Vrstvené svazky nebo sdílené složky</span><span class="sxs-lookup"><span data-stu-id="8a9cd-170">Tiered volumes or shares</span></span> |<span data-ttu-id="8a9cd-171">Rozsah bajtů uzamčení pro aplikace, které pracují s StorSimple vrstvené svazky se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-171">Byte range locking for applications that work with the StorSimple tiered volumes is not supported.</span></span> <span data-ttu-id="8a9cd-172">Pokud je povolený zámek rozsah bajtů, StorSimple vrstvení není funkční.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-172">If byte range locking is enabled, StorSimple tiering does not work.</span></span> |<span data-ttu-id="8a9cd-173">Doporučené míry zahrnují:</span><span class="sxs-lookup"><span data-stu-id="8a9cd-173">Recommended measures include:</span></span> <br></br><span data-ttu-id="8a9cd-174">Vypněte rozsah bajtů zamykání logiky aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-174">Turn off byte range locking in your application logic.</span></span><br></br><span data-ttu-id="8a9cd-175">Zvolte dávat data pro tuto aplikaci v místně vázaných svazků a vrstvené svazky.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-175">Choose to put data for this application in locally pinned volumes as opposed to tiered volumes.</span></span><br></br><span data-ttu-id="8a9cd-176">*Přímý přístup paměti*: při použití místně připnutý svazky a je povolený zámek rozsah bajtů, místně vázaný svazek může být online i před dokončením obnovení.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-176">*Caveat*: When using locally pinned volumes and byte range locking is enabled, the locally pinned volume can be online even before the restore is complete.</span></span> <span data-ttu-id="8a9cd-177">V takových případech Pokud obnovení probíhá, pak je nutné počkat na dokončení obnovení.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-177">In such instances, if a restore is in progress, then you must wait for the restore to complete.</span></span> |
| <span data-ttu-id="8a9cd-178">**7.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-178">**7.**</span></span> |<span data-ttu-id="8a9cd-179">Vrstvený sdílené složky</span><span class="sxs-lookup"><span data-stu-id="8a9cd-179">Tiered shares</span></span> |<span data-ttu-id="8a9cd-180">Práce s velkými soubory může mít za následek pomalé vrstvy.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-180">Working with large files could result in slow tier out.</span></span> |<span data-ttu-id="8a9cd-181">Při práci s velkých souborů, doporučujeme vám, že na největší soubor je menší než velikost sdílené složky % 3.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-181">When working with large files, we recommend that the largest file is smaller than 3% of the share size.</span></span> |
| <span data-ttu-id="8a9cd-182">**8.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-182">**8.**</span></span> |<span data-ttu-id="8a9cd-183">Použít kapacity pro sdílené složky</span><span class="sxs-lookup"><span data-stu-id="8a9cd-183">Used capacity for shares</span></span> |<span data-ttu-id="8a9cd-184">Může se zobrazit sdílení spotřeby, pokud nejsou žádná data ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-184">You may see share consumption when there is no data on the share.</span></span> <span data-ttu-id="8a9cd-185">Tato spotřeba totiž využité kapacitě pro sdílené složky obsahuje metadata.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-185">This consumption is because the used capacity for shares includes metadata.</span></span> | |
| <span data-ttu-id="8a9cd-186">**9.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-186">**9.**</span></span> |<span data-ttu-id="8a9cd-187">Zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="8a9cd-187">Disaster recovery</span></span> |<span data-ttu-id="8a9cd-188">Můžete provádět pouze obnovení po havárii souborového serveru ke stejné doméně jako u zdrojového zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-188">You can only perform the disaster recovery of a file server to the same domain as that of the source device.</span></span> <span data-ttu-id="8a9cd-189">V této verzi nepodporuje zotavení po havárii na cílovém zařízení v jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-189">Disaster recovery to a target device in another domain is not supported in this release.</span></span> |<span data-ttu-id="8a9cd-190">Tato možnost je implementovaná v novější verzi.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-190">This is implemented in a later release.</span></span> <span data-ttu-id="8a9cd-191">Další informace, přejděte na [převzetí služeb při selhání a zotavení po havárii pro pole virtuální zařízení StorSimple](storsimple-virtual-array-failover-dr.md)</span><span class="sxs-lookup"><span data-stu-id="8a9cd-191">For more information, go to [Failover and disaster recovery for your StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md)</span></span> |
| <span data-ttu-id="8a9cd-192">**10.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-192">**10.**</span></span> |<span data-ttu-id="8a9cd-193">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8a9cd-193">Azure PowerShell</span></span> |<span data-ttu-id="8a9cd-194">Virtuální zařízení StorSimple nelze spravovat pomocí prostředí Azure PowerShell v této verzi.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-194">The StorSimple virtual devices cannot be managed through the Azure PowerShell in this release.</span></span> |<span data-ttu-id="8a9cd-195">Všechny správu virtuálních zařízení by mělo být provedeno prostřednictvím portálu Azure a místní webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-195">All the management of the virtual devices should be done through the Azure portal and the local web UI.</span></span> |
| <span data-ttu-id="8a9cd-196">**11.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-196">**11.**</span></span> |<span data-ttu-id="8a9cd-197">Změna hesla</span><span class="sxs-lookup"><span data-stu-id="8a9cd-197">Password change</span></span> |<span data-ttu-id="8a9cd-198">Konzole virtuální pole zařízení přijímá pouze vstup v en-us klávesové formátu.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-198">The virtual array device console only accepts input in en-us keyboard format.</span></span> | |
| <span data-ttu-id="8a9cd-199">**12.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-199">**12.**</span></span> |<span data-ttu-id="8a9cd-200">CHAP</span><span class="sxs-lookup"><span data-stu-id="8a9cd-200">CHAP</span></span> |<span data-ttu-id="8a9cd-201">Po vytvoření pověření CHAP nelze odebrat.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-201">CHAP credentials once created cannot be removed.</span></span> <span data-ttu-id="8a9cd-202">Kromě toho pokud změníte přihlašovací údaje CHAP, budete muset svazky převést do režimu offline a pak přiřaďte je online změna se projeví.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-202">Additionally, if you modify the CHAP credentials, you need to take the volumes offline and then bring them online for the change to take effect.</span></span> |<span data-ttu-id="8a9cd-203">Tomuto problému dochází v novější verzi.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-203">This issue is addressed in a later release.</span></span> |
| <span data-ttu-id="8a9cd-204">**13.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-204">**13.**</span></span> |<span data-ttu-id="8a9cd-205">serveru se službou iSCSI</span><span class="sxs-lookup"><span data-stu-id="8a9cd-205">iSCSI server</span></span> |<span data-ttu-id="8a9cd-206">'Použít úložiště, zobrazí pro svazek iSCSI může být jiný služby StorSimple Manager zařízení a iSCSI hostitele.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-206">The 'Used storage' displayed for an iSCSI volume may be different in the StorSimple Device Manager service and the iSCSI host.</span></span> |<span data-ttu-id="8a9cd-207">Zobrazení souborů má hostitel iSCSI.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-207">The iSCSI host has the filesystem view.</span></span><br></br><span data-ttu-id="8a9cd-208">Zařízení se zobrazí na bloky přidělené při při maximální velikosti svazku.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-208">The device sees the blocks allocated when the volume was at the maximum size.</span></span> |
| <span data-ttu-id="8a9cd-209">**14.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-209">**14.**</span></span> |<span data-ttu-id="8a9cd-210">Souborový server</span><span class="sxs-lookup"><span data-stu-id="8a9cd-210">File server</span></span> |<span data-ttu-id="8a9cd-211">Pokud má soubor ve složce alternativní Data datového proudu (reklamy) s ním spojená, není reklamy zálohovat nebo obnovit prostřednictvím obnovení po havárii, klonování a obnovení na úrovni položek.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-211">If a file in a folder has an Alternate Data Stream (ADS) associated with it, the ADS is not backed up or restored via disaster recovery, clone, and Item Level Recovery.</span></span> | |
| <span data-ttu-id="8a9cd-212">**15.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-212">**15.**</span></span> |<span data-ttu-id="8a9cd-213">Souborový server</span><span class="sxs-lookup"><span data-stu-id="8a9cd-213">File server</span></span> |<span data-ttu-id="8a9cd-214">Symbolické odkazy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-214">Symbolic links are not supported.</span></span> | |
| <span data-ttu-id="8a9cd-215">**16.**</span><span class="sxs-lookup"><span data-stu-id="8a9cd-215">**16.**</span></span> |<span data-ttu-id="8a9cd-216">Souborový server</span><span class="sxs-lookup"><span data-stu-id="8a9cd-216">File server</span></span> |<span data-ttu-id="8a9cd-217">Soubory chráněné pomocí Windows systém souboru (Encrypting File System) po zkopírování přes nebo uložený na výsledek pole virtuální zařízení StorSimple souborového serveru v nepodporované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-217">Files protected by Windows Encrypting File System (EFS) when copied over or stored on the StorSimple Virtual Array file server result in an unsupported configuration.</span></span>  | |

## <a name="next-step"></a><span data-ttu-id="8a9cd-218">Další krok</span><span class="sxs-lookup"><span data-stu-id="8a9cd-218">Next step</span></span>
<span data-ttu-id="8a9cd-219">[Nainstalujte aktualizaci 0,5](storsimple-virtual-array-install-update-05.md) na pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="8a9cd-219">[Install Update 0.5](storsimple-virtual-array-install-update-05.md) on your StorSimple Virtual Array.</span></span>

## <a name="references"></a><span data-ttu-id="8a9cd-220">Odkazy</span><span class="sxs-lookup"><span data-stu-id="8a9cd-220">References</span></span>
<span data-ttu-id="8a9cd-221">Hledáte starší poznámku k verzi?</span><span class="sxs-lookup"><span data-stu-id="8a9cd-221">Looking for an older release note?</span></span> <span data-ttu-id="8a9cd-222">Přejít na:</span><span class="sxs-lookup"><span data-stu-id="8a9cd-222">Go to:</span></span>

* [<span data-ttu-id="8a9cd-223">K vydání 0,4 verze Update pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="8a9cd-223">StorSimple Virtual Array Update 0.4 Release Notes</span></span>](storsimple-virtual-array-update-04-release-notes.md)
* [<span data-ttu-id="8a9cd-224">K vydání 0,3 verze Update pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="8a9cd-224">StorSimple Virtual Array Update 0.3 Release Notes</span></span>](storsimple-ova-update-03-release-notes.md)
* [<span data-ttu-id="8a9cd-225">Poznámky k verzi zařízení StorSimple virtuální pole aktualizace 0,1 a 0,2</span><span class="sxs-lookup"><span data-stu-id="8a9cd-225">StorSimple Virtual Array Update 0.1 and 0.2 Release Notes</span></span>](storsimple-ova-update-01-release-notes.md)
* [<span data-ttu-id="8a9cd-226">Poznámky k verzi zařízení StorSimple virtuální pole Obecné dostupnosti</span><span class="sxs-lookup"><span data-stu-id="8a9cd-226">StorSimple Virtual Array General Availability Release Notes</span></span>](storsimple-ova-pp-release-notes.md)
