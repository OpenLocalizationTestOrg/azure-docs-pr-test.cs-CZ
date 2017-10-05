---
title: "Poznámky k verzi 0,6 aktualizace pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje důležité otevřené problémy a řešení pro pole virtuální zařízení StorSimple spuštění Update 0,6."
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
ms.date: 05/24/2017
ms.author: alkohli
ms.openlocfilehash: af202cf652300ee7897eb2dede33f38058fc2837
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-virtual-array-update-06-release-notes"></a><span data-ttu-id="956dd-103">Poznámky k verzi 0,6 aktualizace pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="956dd-103">StorSimple Virtual Array Update 0.6 release notes</span></span>

## <a name="overview"></a><span data-ttu-id="956dd-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="956dd-104">Overview</span></span>

<span data-ttu-id="956dd-105">Následující poznámky k verzi identifikovat kritická otevřené problémy a vyřešit problémy pro Microsoft Azure StorSimple virtuální pole aktualizace.</span><span class="sxs-lookup"><span data-stu-id="956dd-105">The following release notes identify the critical open issues and the resolved issues for Microsoft Azure StorSimple Virtual Array updates.</span></span>

<span data-ttu-id="956dd-106">Poznámky k verzi se průběžně aktualizuje a při zjištění zásadních problémů, které vyžadují řešení, se řešení přidá.</span><span class="sxs-lookup"><span data-stu-id="956dd-106">The release notes are continuously updated, and as critical issues requiring a workaround are discovered, they are added.</span></span> <span data-ttu-id="956dd-107">Před nasazením pole virtuální zařízení StorSimple, pečlivě zkontrolujte informace obsažené v poznámkách k verzi.</span><span class="sxs-lookup"><span data-stu-id="956dd-107">Before you deploy your StorSimple Virtual Array, carefully review the information contained in the release notes.</span></span>

<span data-ttu-id="956dd-108">Aktualizace 0,6 odpovídá verzi softwaru **10.0.10293.0**.</span><span class="sxs-lookup"><span data-stu-id="956dd-108">Update 0.6 corresponds to the software version **10.0.10293.0**.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="956dd-109">Aktualizace jsou rušivý a zařízení restartovat.</span><span class="sxs-lookup"><span data-stu-id="956dd-109">Updates are disruptive and restart your device.</span></span> <span data-ttu-id="956dd-110">Pokud vstupně-výstupní operace probíhá, zařízení způsobuje výpadek.</span><span class="sxs-lookup"><span data-stu-id="956dd-110">If I/O are in progress, the device incurs downtime.</span></span> <span data-ttu-id="956dd-111">Podrobné pokyny o tom, jak použít aktualizaci, přejděte na [nainstalujte aktualizaci 0,6](storsimple-virtual-array-install-update-06.md).</span><span class="sxs-lookup"><span data-stu-id="956dd-111">For detailed instructions on how to apply the update, go to [Install Update 0.6](storsimple-virtual-array-install-update-06.md).</span></span>
>
> - <span data-ttu-id="956dd-112">Důrazně doporučujeme nainstalovat aktualizaci 0,6 okamžitě, protože obsahuje opravy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="956dd-112">We strongly recommend that you install Update 0.6 immediately as it contains critical security fixes.</span></span>


## <a name="whats-new-in-the-update-06"></a><span data-ttu-id="956dd-113">Co je nového v aktualizaci 0,6</span><span class="sxs-lookup"><span data-stu-id="956dd-113">What's new in the Update 0.6</span></span>
<span data-ttu-id="956dd-114">Aktualizace 0,6 je důležité aktualizace a musí být nasazené, okamžitě.</span><span class="sxs-lookup"><span data-stu-id="956dd-114">Update 0.6 is a critical update and should be deployed immediately.</span></span> <span data-ttu-id="956dd-115">Tato aktualizace obsahuje následující opravy:</span><span class="sxs-lookup"><span data-stu-id="956dd-115">This update contains the following fixes:</span></span> 

- <span data-ttu-id="956dd-116">**Opravy zabezpečení systému Windows** – tato verze obsahuje **opravy zabezpečení systému Windows kritické**.</span><span class="sxs-lookup"><span data-stu-id="956dd-116">**Windows Security fixes** - This release has **Windows critical security fixes**.</span></span> <span data-ttu-id="956dd-117">Projděte si další informace o problémy zabezpečení a přidružené opravy následující aktualizace zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="956dd-117">Review the following security updates for more information about the security issues and the associated fixes:</span></span>
    - [<span data-ttu-id="956dd-118">Prosinec 2016 pouze kvality zabezpečení aktualizace pro Windows 8.1 a Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="956dd-118">December 2016 Security Only Quality Update for Windows 8.1 and Windows Server 2012 R2</span></span>](https://support.microsoft.com/help/3205400/december-2016-security-only-quality-update-for-windows-8.1-and-windows-server-2012-r2)
    - [<span data-ttu-id="956dd-119">2017 března pouze kvality zabezpečení aktualizace pro Windows 8.1 a Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="956dd-119">March 2017 Security Only Quality Update for Windows 8.1 and Windows Server 2012 R2</span></span>](https://support.microsoft.com/help/4012213/march-2017-security-only-quality-update-for-windows-8-1-and-windows-server-2012-r23)
    - [<span data-ttu-id="956dd-120">9 může 2017 – KB4019213 (jen zabezpečení aktualizace)</span><span class="sxs-lookup"><span data-stu-id="956dd-120">May 9, 2017—KB4019213 (Security-only update)</span></span>](https://support.microsoft.com/help/4019213/windows-8-update-kb4019213)

- <span data-ttu-id="956dd-121">**Obnovení opravu** -ve starších verzích byla chybu, která by zabránila v dokončení obnovení.</span><span class="sxs-lookup"><span data-stu-id="956dd-121">**Restore fix** - In earlier releases, there was a bug that would prevent the restore from completing.</span></span> <span data-ttu-id="956dd-122">Tato chyba byla opravena v této verzi.</span><span class="sxs-lookup"><span data-stu-id="956dd-122">This bug has been fixed in this release.</span></span>


## <a name="issues-fixed-in-the-update-06"></a><span data-ttu-id="956dd-123">Chyby v aktualizaci 0,6</span><span class="sxs-lookup"><span data-stu-id="956dd-123">Issues fixed in the Update 0.6</span></span>

<span data-ttu-id="956dd-124">Následující tabulka obsahuje souhrn chyby v této verzi.</span><span class="sxs-lookup"><span data-stu-id="956dd-124">The following table provides a summary of issues fixed in this release.</span></span>

| <span data-ttu-id="956dd-125">Ne.</span><span class="sxs-lookup"><span data-stu-id="956dd-125">No.</span></span> | <span data-ttu-id="956dd-126">Funkce</span><span class="sxs-lookup"><span data-stu-id="956dd-126">Feature</span></span> | <span data-ttu-id="956dd-127">Problém</span><span class="sxs-lookup"><span data-stu-id="956dd-127">Issue</span></span> |
| --- | --- | --- |
| <span data-ttu-id="956dd-128">1</span><span class="sxs-lookup"><span data-stu-id="956dd-128">1</span></span> |<span data-ttu-id="956dd-129">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="956dd-129">Security</span></span>| <span data-ttu-id="956dd-130">Tato verze obsahuje důležité aktualizace zabezpečení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="956dd-130">This release contains critical Windows Security updates.</span></span> <span data-ttu-id="956dd-131">Doporučujeme nainstalovat tuto aktualizaci okamžitě.</span><span class="sxs-lookup"><span data-stu-id="956dd-131">We suggest that you install this update immediately.</span></span>|
| <span data-ttu-id="956dd-132">2</span><span class="sxs-lookup"><span data-stu-id="956dd-132">2</span></span> |<span data-ttu-id="956dd-133">Obnovení</span><span class="sxs-lookup"><span data-stu-id="956dd-133">Restore</span></span>| <span data-ttu-id="956dd-134">Během obnovení byl spor, který by brání v dokončení úlohy obnovení.</span><span class="sxs-lookup"><span data-stu-id="956dd-134">During a restore, there was a race condition that would prevent the restore job from completing.</span></span> <span data-ttu-id="956dd-135">Oprava chyby řeší tento spor.</span><span class="sxs-lookup"><span data-stu-id="956dd-135">The bug fix addresses this race condition.</span></span>|


## <a name="known-issues-in-the-update-06"></a><span data-ttu-id="956dd-136">Známé problémy v aktualizaci 0,6</span><span class="sxs-lookup"><span data-stu-id="956dd-136">Known issues in the Update 0.6</span></span>

<span data-ttu-id="956dd-137">Následující tabulka obsahuje souhrn známé problémy pro pole virtuální zařízení StorSimple a zahrnuje problémy uvedené verze z předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="956dd-137">The following table provides a summary of known issues for the StorSimple Virtual Array and includes the issues release-noted from the previous releases.</span></span>

| <span data-ttu-id="956dd-138">Ne.</span><span class="sxs-lookup"><span data-stu-id="956dd-138">No.</span></span> | <span data-ttu-id="956dd-139">Funkce</span><span class="sxs-lookup"><span data-stu-id="956dd-139">Feature</span></span> | <span data-ttu-id="956dd-140">Problém</span><span class="sxs-lookup"><span data-stu-id="956dd-140">Issue</span></span> | <span data-ttu-id="956dd-141">Alternativní řešení a komentář</span><span class="sxs-lookup"><span data-stu-id="956dd-141">Workaround/comments</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="956dd-142">**1.**</span><span class="sxs-lookup"><span data-stu-id="956dd-142">**1.**</span></span> |<span data-ttu-id="956dd-143">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="956dd-143">Updates</span></span> |<span data-ttu-id="956dd-144">Virtuální zařízení vytvořené ve verzi preview nelze aktualizovat na podporovanou verzi obecné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="956dd-144">The virtual devices created in the preview release cannot be updated to a supported General Availability version.</span></span> |<span data-ttu-id="956dd-145">Tato virtuální zařízení musí převzetí služeb při selhání pro verzi obecné dostupnosti pomocí pracovního postupu zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="956dd-145">These virtual devices must be failed over for the General Availability release using a disaster recovery (DR) workflow.</span></span> |
| <span data-ttu-id="956dd-146">**2.**</span><span class="sxs-lookup"><span data-stu-id="956dd-146">**2.**</span></span> |<span data-ttu-id="956dd-147">Zřízené datový disk</span><span class="sxs-lookup"><span data-stu-id="956dd-147">Provisioned data disk</span></span> |<span data-ttu-id="956dd-148">Jakmile máte zřízen datový disk určité zadané velikosti a vytvořit odpovídající virtuální zařízení StorSimple, nesmí zvětšení nebo zmenšení datový disk.</span><span class="sxs-lookup"><span data-stu-id="956dd-148">Once you have provisioned a data disk of a certain specified size and created the corresponding StorSimple virtual device, you must not expand or shrink the data disk.</span></span> <span data-ttu-id="956dd-149">Probíhá pokus o provést výsledky ke ztrátě všech dat v místních vrstvách zařízení.</span><span class="sxs-lookup"><span data-stu-id="956dd-149">Attempting to do results in a loss of all the data in the local tiers of the device.</span></span> | |
| <span data-ttu-id="956dd-150">**3.**</span><span class="sxs-lookup"><span data-stu-id="956dd-150">**3.**</span></span> |<span data-ttu-id="956dd-151">Zásady skupiny</span><span class="sxs-lookup"><span data-stu-id="956dd-151">Group policy</span></span> |<span data-ttu-id="956dd-152">Když je zařízení připojené k doméně, použití zásad skupiny může nepříznivě ovlivnit činnost zařízení.</span><span class="sxs-lookup"><span data-stu-id="956dd-152">When a device is domain-joined, applying a group policy can adversely affect the device operation.</span></span> |<span data-ttu-id="956dd-153">Zkontrolujte, zda je vaše virtuální pole ve vlastní organizační jednotku (OU) pro službu Active Directory a žádné objekty zásad skupiny (GPO) se použijí k němu.</span><span class="sxs-lookup"><span data-stu-id="956dd-153">Ensure that your virtual array is in its own organizational unit (OU) for Active Directory and no group policy objects (GPO) are applied to it.</span></span> |
| <span data-ttu-id="956dd-154">**4.**</span><span class="sxs-lookup"><span data-stu-id="956dd-154">**4.**</span></span> |<span data-ttu-id="956dd-155">Místní webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="956dd-155">Local web UI</span></span> |<span data-ttu-id="956dd-156">Pokud funkce Rozšířené zabezpečení jsou povolené v aplikaci Internet Explorer (IE ESC), některé z uživatelského rozhraní místní webové stránky, jako je například údržby nebo Poradce při potížích s nemusí fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="956dd-156">If enhanced security features are enabled in Internet Explorer (IE ESC), some local web UI pages such as Troubleshooting or Maintenance may not work properly.</span></span> <span data-ttu-id="956dd-157">Tlačítka na těchto stránkách nemusí fungovat.</span><span class="sxs-lookup"><span data-stu-id="956dd-157">Buttons on these pages may also not work.</span></span> |<span data-ttu-id="956dd-158">Vypněte funkce rozšířeného zabezpečení aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="956dd-158">Turn off enhanced security features in Internet Explorer.</span></span> |
| <span data-ttu-id="956dd-159">**5.**</span><span class="sxs-lookup"><span data-stu-id="956dd-159">**5.**</span></span> |<span data-ttu-id="956dd-160">Místní webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="956dd-160">Local web UI</span></span> |<span data-ttu-id="956dd-161">Ve virtuálním počítači technologie Hyper-V rozhraní sítě v webového uživatelského rozhraní se zobrazí jako 10 GB/s rozhraní.</span><span class="sxs-lookup"><span data-stu-id="956dd-161">In a Hyper-V virtual machine, the network interfaces in the web UI are displayed as 10 Gbps interfaces.</span></span> |<span data-ttu-id="956dd-162">Toto chování je odraz technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="956dd-162">This behavior is a reflection of Hyper-V.</span></span> <span data-ttu-id="956dd-163">Technologie Hyper-V vždy zobrazuje 10 GB/s pro virtuální síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="956dd-163">Hyper-V always shows 10 Gbps for virtual network adapters.</span></span> |
| <span data-ttu-id="956dd-164">**6.**</span><span class="sxs-lookup"><span data-stu-id="956dd-164">**6.**</span></span> |<span data-ttu-id="956dd-165">Vrstvené svazky nebo sdílené složky</span><span class="sxs-lookup"><span data-stu-id="956dd-165">Tiered volumes or shares</span></span> |<span data-ttu-id="956dd-166">Rozsah bajtů uzamčení pro aplikace, které pracují s StorSimple vrstvené svazky se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="956dd-166">Byte range locking for applications that work with the StorSimple tiered volumes is not supported.</span></span> <span data-ttu-id="956dd-167">Pokud je povolený zámek rozsah bajtů, StorSimple vrstvení není funkční.</span><span class="sxs-lookup"><span data-stu-id="956dd-167">If byte range locking is enabled, StorSimple tiering does not work.</span></span> |<span data-ttu-id="956dd-168">Doporučené míry zahrnují:</span><span class="sxs-lookup"><span data-stu-id="956dd-168">Recommended measures include:</span></span> <br></br><span data-ttu-id="956dd-169">Vypněte rozsah bajtů zamykání logiky aplikace.</span><span class="sxs-lookup"><span data-stu-id="956dd-169">Turn off byte range locking in your application logic.</span></span><br></br><span data-ttu-id="956dd-170">Zvolte dávat data pro tuto aplikaci v místně vázaných svazků a vrstvené svazky.</span><span class="sxs-lookup"><span data-stu-id="956dd-170">Choose to put data for this application in locally pinned volumes as opposed to tiered volumes.</span></span><br></br><span data-ttu-id="956dd-171">*Přímý přístup paměti*: při použití místně připnutý svazky a je povolený zámek rozsah bajtů, místně vázaný svazek může být online i před dokončením obnovení.</span><span class="sxs-lookup"><span data-stu-id="956dd-171">*Caveat*: When using locally pinned volumes and byte range locking is enabled, the locally pinned volume can be online even before the restore is complete.</span></span> <span data-ttu-id="956dd-172">V takových případech Pokud obnovení probíhá, pak je nutné počkat na dokončení obnovení.</span><span class="sxs-lookup"><span data-stu-id="956dd-172">In such instances, if a restore is in progress, then you must wait for the restore to complete.</span></span> |
| <span data-ttu-id="956dd-173">**7.**</span><span class="sxs-lookup"><span data-stu-id="956dd-173">**7.**</span></span> |<span data-ttu-id="956dd-174">Vrstvený sdílené složky</span><span class="sxs-lookup"><span data-stu-id="956dd-174">Tiered shares</span></span> |<span data-ttu-id="956dd-175">Práce s velkými soubory může mít za následek pomalé vrstvy.</span><span class="sxs-lookup"><span data-stu-id="956dd-175">Working with large files could result in slow tier out.</span></span> |<span data-ttu-id="956dd-176">Při práci s velkých souborů, doporučujeme vám, že na největší soubor je menší než velikost sdílené složky % 3.</span><span class="sxs-lookup"><span data-stu-id="956dd-176">When working with large files, we recommend that the largest file is smaller than 3% of the share size.</span></span> |
| <span data-ttu-id="956dd-177">**8.**</span><span class="sxs-lookup"><span data-stu-id="956dd-177">**8.**</span></span> |<span data-ttu-id="956dd-178">Použít kapacity pro sdílené složky</span><span class="sxs-lookup"><span data-stu-id="956dd-178">Used capacity for shares</span></span> |<span data-ttu-id="956dd-179">Může se zobrazit sdílení spotřeby, pokud nejsou žádná data ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="956dd-179">You may see share consumption when there is no data on the share.</span></span> <span data-ttu-id="956dd-180">Tato spotřeba totiž využité kapacitě pro sdílené složky obsahuje metadata.</span><span class="sxs-lookup"><span data-stu-id="956dd-180">This consumption is because the used capacity for shares includes metadata.</span></span> | |
| <span data-ttu-id="956dd-181">**9.**</span><span class="sxs-lookup"><span data-stu-id="956dd-181">**9.**</span></span> |<span data-ttu-id="956dd-182">Zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="956dd-182">Disaster recovery</span></span> |<span data-ttu-id="956dd-183">Můžete provádět pouze obnovení po havárii souborového serveru ke stejné doméně jako u zdrojového zařízení.</span><span class="sxs-lookup"><span data-stu-id="956dd-183">You can only perform the disaster recovery of a file server to the same domain as that of the source device.</span></span> <span data-ttu-id="956dd-184">V této verzi nepodporuje zotavení po havárii na cílovém zařízení v jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="956dd-184">Disaster recovery to a target device in another domain is not supported in this release.</span></span> |<span data-ttu-id="956dd-185">Tato možnost je implementovaná v novější verzi.</span><span class="sxs-lookup"><span data-stu-id="956dd-185">This is implemented in a later release.</span></span> <span data-ttu-id="956dd-186">Další informace, přejděte na [převzetí služeb při selhání a zotavení po havárii pro pole virtuální zařízení StorSimple](storsimple-virtual-array-failover-dr.md)</span><span class="sxs-lookup"><span data-stu-id="956dd-186">For more information, go to [Failover and disaster recovery for your StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md)</span></span> |
| <span data-ttu-id="956dd-187">**10.**</span><span class="sxs-lookup"><span data-stu-id="956dd-187">**10.**</span></span> |<span data-ttu-id="956dd-188">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="956dd-188">Azure PowerShell</span></span> |<span data-ttu-id="956dd-189">Virtuální zařízení StorSimple nelze spravovat pomocí prostředí Azure PowerShell v této verzi.</span><span class="sxs-lookup"><span data-stu-id="956dd-189">The StorSimple virtual devices cannot be managed through the Azure PowerShell in this release.</span></span> |<span data-ttu-id="956dd-190">Všechny správu virtuálních zařízení by mělo být provedeno prostřednictvím portálu Azure a místní webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="956dd-190">All the management of the virtual devices should be done through the Azure portal and the local web UI.</span></span> |
| <span data-ttu-id="956dd-191">**11.**</span><span class="sxs-lookup"><span data-stu-id="956dd-191">**11.**</span></span> |<span data-ttu-id="956dd-192">Změna hesla</span><span class="sxs-lookup"><span data-stu-id="956dd-192">Password change</span></span> |<span data-ttu-id="956dd-193">Konzole virtuální pole zařízení přijímá pouze vstup v en-us klávesové formátu.</span><span class="sxs-lookup"><span data-stu-id="956dd-193">The virtual array device console only accepts input in en-us keyboard format.</span></span> | |
| <span data-ttu-id="956dd-194">**12.**</span><span class="sxs-lookup"><span data-stu-id="956dd-194">**12.**</span></span> |<span data-ttu-id="956dd-195">CHAP</span><span class="sxs-lookup"><span data-stu-id="956dd-195">CHAP</span></span> |<span data-ttu-id="956dd-196">Po vytvoření pověření CHAP nelze odebrat.</span><span class="sxs-lookup"><span data-stu-id="956dd-196">CHAP credentials once created cannot be removed.</span></span> <span data-ttu-id="956dd-197">Kromě toho pokud změníte přihlašovací údaje CHAP, budete muset svazky převést do režimu offline a pak přiřaďte je online změna se projeví.</span><span class="sxs-lookup"><span data-stu-id="956dd-197">Additionally, if you modify the CHAP credentials, you need to take the volumes offline and then bring them online for the change to take effect.</span></span> |<span data-ttu-id="956dd-198">Tomuto problému dochází v novější verzi.</span><span class="sxs-lookup"><span data-stu-id="956dd-198">This issue is addressed in a later release.</span></span> |
| <span data-ttu-id="956dd-199">**13.**</span><span class="sxs-lookup"><span data-stu-id="956dd-199">**13.**</span></span> |<span data-ttu-id="956dd-200">serveru se službou iSCSI</span><span class="sxs-lookup"><span data-stu-id="956dd-200">iSCSI server</span></span> |<span data-ttu-id="956dd-201">'Použít úložiště, zobrazí pro svazek iSCSI může být jiný služby StorSimple Manager zařízení a iSCSI hostitele.</span><span class="sxs-lookup"><span data-stu-id="956dd-201">The 'Used storage' displayed for an iSCSI volume may be different in the StorSimple Device Manager service and the iSCSI host.</span></span> |<span data-ttu-id="956dd-202">Zobrazení souborů má hostitel iSCSI.</span><span class="sxs-lookup"><span data-stu-id="956dd-202">The iSCSI host has the filesystem view.</span></span><br></br><span data-ttu-id="956dd-203">Zařízení se zobrazí na bloky přidělené při při maximální velikosti svazku.</span><span class="sxs-lookup"><span data-stu-id="956dd-203">The device sees the blocks allocated when the volume was at the maximum size.</span></span> |
| <span data-ttu-id="956dd-204">**14.**</span><span class="sxs-lookup"><span data-stu-id="956dd-204">**14.**</span></span> |<span data-ttu-id="956dd-205">Souborový server</span><span class="sxs-lookup"><span data-stu-id="956dd-205">File server</span></span> |<span data-ttu-id="956dd-206">Pokud má soubor ve složce alternativní Data datového proudu (reklamy) s ním spojená, není reklamy zálohovat nebo obnovit prostřednictvím obnovení po havárii, klonování a obnovení na úrovni položek.</span><span class="sxs-lookup"><span data-stu-id="956dd-206">If a file in a folder has an Alternate Data Stream (ADS) associated with it, the ADS is not backed up or restored via disaster recovery, clone, and Item Level Recovery.</span></span> | |
| <span data-ttu-id="956dd-207">**15.**</span><span class="sxs-lookup"><span data-stu-id="956dd-207">**15.**</span></span> |<span data-ttu-id="956dd-208">Souborový server</span><span class="sxs-lookup"><span data-stu-id="956dd-208">File server</span></span> |<span data-ttu-id="956dd-209">Symbolické odkazy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="956dd-209">Symbolic links are not supported.</span></span> | |
| <span data-ttu-id="956dd-210">**16.**</span><span class="sxs-lookup"><span data-stu-id="956dd-210">**16.**</span></span> |<span data-ttu-id="956dd-211">Souborový server</span><span class="sxs-lookup"><span data-stu-id="956dd-211">File server</span></span> |<span data-ttu-id="956dd-212">Soubory chráněné pomocí Windows systém souboru (Encrypting File System) po zkopírování přes nebo uložený na výsledek pole virtuální zařízení StorSimple souborového serveru v nepodporované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="956dd-212">Files protected by Windows Encrypting File System (EFS) when copied over or stored on the StorSimple Virtual Array file server result in an unsupported configuration.</span></span>  | |
| <span data-ttu-id="956dd-213">**17.**</span><span class="sxs-lookup"><span data-stu-id="956dd-213">**17.**</span></span> |<span data-ttu-id="956dd-214">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="956dd-214">Updates</span></span> |<span data-ttu-id="956dd-215">Pokud se zobrazí chyba kód: 2359302 (šestnáctkově 0x240006) při pokusu o instalaci opravy hotfix prostřednictvím místního uživatelského rozhraní, pak z toho vyplývá, že oprava hotfix je již nainstalována na zařízení.</span><span class="sxs-lookup"><span data-stu-id="956dd-215">If you see Error code: 2359302 (hex 0x240006) when trying to install a hotfix through the local UI, then this implies that the hotfix is already installed on your device.</span></span>   | |

## <a name="next-step"></a><span data-ttu-id="956dd-216">Další krok</span><span class="sxs-lookup"><span data-stu-id="956dd-216">Next step</span></span>
<span data-ttu-id="956dd-217">[Nainstalujte aktualizaci 0,6](storsimple-virtual-array-install-update-06.md) na pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="956dd-217">[Install Update 0.6](storsimple-virtual-array-install-update-06.md) on your StorSimple Virtual Array.</span></span>

## <a name="references"></a><span data-ttu-id="956dd-218">Odkazy</span><span class="sxs-lookup"><span data-stu-id="956dd-218">References</span></span>
<span data-ttu-id="956dd-219">Hledáte starší poznámku k verzi?</span><span class="sxs-lookup"><span data-stu-id="956dd-219">Looking for an older release note?</span></span> <span data-ttu-id="956dd-220">Přejít na:</span><span class="sxs-lookup"><span data-stu-id="956dd-220">Go to:</span></span>

* [<span data-ttu-id="956dd-221">K vydání 0,5 verze Update pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="956dd-221">StorSimple Virtual Array Update 0.5 Release Notes</span></span>](storsimple-virtual-array-update-05-release-notes.md)
* [<span data-ttu-id="956dd-222">K vydání 0,4 verze Update pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="956dd-222">StorSimple Virtual Array Update 0.4 Release Notes</span></span>](storsimple-virtual-array-update-04-release-notes.md)
* [<span data-ttu-id="956dd-223">K vydání 0,3 verze Update pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="956dd-223">StorSimple Virtual Array Update 0.3 Release Notes</span></span>](storsimple-ova-update-03-release-notes.md)
* [<span data-ttu-id="956dd-224">Poznámky k verzi zařízení StorSimple virtuální pole aktualizace 0,1 a 0,2</span><span class="sxs-lookup"><span data-stu-id="956dd-224">StorSimple Virtual Array Update 0.1 and 0.2 Release Notes</span></span>](storsimple-ova-update-01-release-notes.md)
* [<span data-ttu-id="956dd-225">Poznámky k verzi zařízení StorSimple virtuální pole Obecné dostupnosti</span><span class="sxs-lookup"><span data-stu-id="956dd-225">StorSimple Virtual Array General Availability Release Notes</span></span>](storsimple-ova-pp-release-notes.md)
