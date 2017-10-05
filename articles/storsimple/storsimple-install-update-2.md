---
title: "Nainstalujte na zařízení StorSimple Update 2 | Microsoft Docs"
description: "Popisuje postup instalace zařízení StorSimple 8000 řady Update 2 na zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 8c8981df-75d9-4d19-b137-d6c6ba39dcfb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: e788439608b7122f2bca6b99b832baa5258e472d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-2-on-your-storsimple-device"></a><span data-ttu-id="1abc4-103">Nainstalujte na zařízení StorSimple Update 2</span><span class="sxs-lookup"><span data-stu-id="1abc4-103">Install Update 2 on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="1abc4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1abc4-104">Overview</span></span>
<span data-ttu-id="1abc4-105">Tento kurz vysvětluje, jak nainstalovat aktualizace 2 na zařízení StorSimple se starší verzí softwaru prostřednictvím portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="1abc4-105">This tutorial explains how to install Update 2 on a StorSimple device running an earlier software version via the Azure classic portal.</span></span> <span data-ttu-id="1abc4-106">Tento kurz také popisuje kroky potřebné k aktualizaci, když brána je nakonfigurovaná na síťovém rozhraní než DATA 0 zařízení StorSimple a se pokoušíte aktualizovat z verze 1 před aktualizací softwaru.</span><span class="sxs-lookup"><span data-stu-id="1abc4-106">The tutorial also covers the steps required for the update when a gateway is configured on a network interface other than DATA 0 of the StorSimple device and you are trying to update from a pre-Update 1 software version.</span></span>

<span data-ttu-id="1abc4-107">Aktualizace 2 zahrnuje zařízení aktualizací softwaru, aktualizací ovladače LSI a aktualizace firmwaru disku.</span><span class="sxs-lookup"><span data-stu-id="1abc4-107">Update 2 includes device software updates, LSI driver updates, and disk firmware updates.</span></span> <span data-ttu-id="1abc4-108">Software zařízení a LSI aktualizace jsou omezovaly aktualizace a provádět prostřednictvím portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="1abc4-108">The device software and LSI updates are non-disruptive updates and can be applied via the Azure classic portal.</span></span> <span data-ttu-id="1abc4-109">Aktualizace firmwaru disku rušivý aktualizace a lze použít pouze prostřednictvím rozhraní Windows PowerShell zařízení.</span><span class="sxs-lookup"><span data-stu-id="1abc4-109">The disk firmware updates are disruptive updates and can only be applied via the Windows PowerShell interface of the device.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="1abc4-110">Update 2 nemusí zobrazit okamžitě, protože provedeme postupné zavádění aktualizací.</span><span class="sxs-lookup"><span data-stu-id="1abc4-110">You may not see Update 2 immediately because we do a phased rollout of the updates.</span></span> <span data-ttu-id="1abc4-111">Kontrola aktualizací za pár dní znovu jako tato aktualizace brzy bude dostupná.</span><span class="sxs-lookup"><span data-stu-id="1abc4-111">Scan for updates in a few days again as this Update will become available soon.</span></span>
> * <span data-ttu-id="1abc4-112">Sadu ruční a Automatická předběžné kontroly se provádějí před instalací, který měl zjistit stav zařízení z hlediska hardwaru stavu a připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="1abc4-112">A set of manual and automatic pre-checks are done prior to the install to determine the device health in terms of hardware state and network connectivity.</span></span> <span data-ttu-id="1abc4-113">Tyto předběžné kontroly se provádí pouze v případě, že aktualizace použít z portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="1abc4-113">These pre-checks are performed only if you apply the updates from the Azure classic portal.</span></span>
> * <span data-ttu-id="1abc4-114">Doporučujeme instalovat aktualizace softwaru a ovladačů prostřednictvím portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="1abc4-114">We recommend that you install the software and driver updates via the Azure  classic portal.</span></span> <span data-ttu-id="1abc4-115">Má jenom přejděte na rozhraní prostředí Windows PowerShell na zařízení (instalovat aktualizace) Pokud selže kontrola před aktualizací brány na portálu.</span><span class="sxs-lookup"><span data-stu-id="1abc4-115">You should only go to the Windows PowerShell interface of the device (to install updates) if the pre-update gateway check fails in the portal.</span></span> <span data-ttu-id="1abc4-116">Aktualizace může trvat 4-7 hodin k instalaci (včetně aktualizací Windows).</span><span class="sxs-lookup"><span data-stu-id="1abc4-116">The updates may take 4-7 hours to install (including the Windows Updates).</span></span> <span data-ttu-id="1abc4-117">Aktualizace režimu údržby musí být nainstalován prostřednictvím rozhraní Windows PowerShell zařízení.</span><span class="sxs-lookup"><span data-stu-id="1abc4-117">The maintenance mode updates must be installed via the Windows PowerShell interface of the device.</span></span> <span data-ttu-id="1abc4-118">Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto povede k výpadkům pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="1abc4-118">As maintenance mode updates are disruptive updates, these will result in a down time for your device.</span></span>
> * <span data-ttu-id="1abc4-119">Pokud běží volitelné Snapshot Manager zařízení StorSimple, zajistěte, aby upgradu vaší verzí Snapshot Manager na Update 2 před aktualizací zařízení.</span><span class="sxs-lookup"><span data-stu-id="1abc4-119">If running the optional StorSimple Snapshot Manager, ensure that you have upgraded your Snapshot Manager version to Update 2 prior to updating the device.</span></span>
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a><span data-ttu-id="1abc4-120">Instalaci aktualizace 2 prostřednictvím portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="1abc4-120">Install Update 2 via the Azure classic portal</span></span>
<span data-ttu-id="1abc4-121">Proveďte následující kroky k aktualizaci zařízení [Update 2](storsimple-update2-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="1abc4-121">Perform the following steps to update your device to [Update 2](storsimple-update2-release-notes.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1abc4-122">Aktualizace 2 umožňuje Microsoftu jiné diagnostické informace z tohoto zařízení pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="1abc4-122">Update 2 enables Microsoft to pull additional diagnostic information from the device.</span></span> <span data-ttu-id="1abc4-123">Výsledkem je když náš tým operations identifikuje zařízení, která došlo k potížím, jsme jsou lépe vybaveny shromažďovat informace ze zařízení a diagnostikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="1abc4-123">As a result, when our operations team identifies devices that are having problems, we are better equipped to collect information from the device and diagnose issues.</span></span> <span data-ttu-id="1abc4-124">Přijetím Update 2, můžete nám umožňují poskytovat tento proaktivní podporu.</span><span class="sxs-lookup"><span data-stu-id="1abc4-124">By accepting Update 2, you allow us to provide this proactive support.</span></span>
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. <span data-ttu-id="1abc4-125">Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady Update 2 (6.3.9600.17673)**.</span><span class="sxs-lookup"><span data-stu-id="1abc4-125">Verify that your device is running **StorSimple 8000 Series Update 2 (6.3.9600.17673)**.</span></span> <span data-ttu-id="1abc4-126">**Datum poslední aktualizace** také by měl být upraven.</span><span class="sxs-lookup"><span data-stu-id="1abc4-126">The **Last updated date** should also be modified.</span></span> <span data-ttu-id="1abc4-127">Dozvíte se taky, že jsou k dispozici aktualizace režim údržby (Tato zpráva může nadále zobrazovat až 24 hodin po instalaci aktualizace).</span><span class="sxs-lookup"><span data-stu-id="1abc4-127">You'll also see that Maintenance mode updates are available (this message might continue to be displayed for up to 24 hours after you install the updates).</span></span>
   
   <span data-ttu-id="1abc4-128">Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="1abc4-128">Maintenance mode updates are disruptive updates that result in device downtime and can only be applied via the Windows PowerShell interface of your device.</span></span> <span data-ttu-id="1abc4-129">V některých případech při spuštění aktualizace 1.2 firmware disku mohou být již aktuální, v takovém případě nemusíte instalovat všechny aktualizace režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="1abc4-129">In some cases when you are running Update 1.2, your disk firmware might already be up-to-date, in which case you don't need to install any maintenance mode updates.</span></span>
2. <span data-ttu-id="1abc4-130">Stáhnout aktualizace režimu údržby pomocí kroků uvedených v [ke stažení opravy hotfix](#to-download-hotfixes) k hledání a stahování KB3121899, který nainstaluje aktualizace firmwaru disku (s jinými aktualizacemi musí už být nainstalovaný nyní).</span><span class="sxs-lookup"><span data-stu-id="1abc4-130">Download the maintenance mode updates by using the steps listed in [To download hotfixes](#to-download-hotfixes) to search for and download KB3121899, which installs disk firmware updates (the other updates should already be installed by now).</span></span>
3. <span data-ttu-id="1abc4-131">Postupujte podle kroků uvedených v [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) do režimu údržby instalace aktualizací.</span><span class="sxs-lookup"><span data-stu-id="1abc4-131">Follow the steps listed in [install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) to install the maintenance mode updates.</span></span>

## <a name="install-update-2-as-a-hotfix"></a><span data-ttu-id="1abc4-132">Instalaci aktualizace 2 jako oprava hotfix</span><span class="sxs-lookup"><span data-stu-id="1abc4-132">Install Update 2 as a hotfix</span></span>
<span data-ttu-id="1abc4-133">Tento postup použijte, pokud selže kontrola brány při pokusu o instalaci aktualizací prostřednictvím portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="1abc4-133">Use this procedure if you fail the gateway check when trying to install the updates through the Azure classic portal.</span></span> <span data-ttu-id="1abc4-134">Kontrola selže, jak máte bránu přiřazené 0 síťové rozhraní bez dat a vaše zařízení používá verzi softwaru před Update 1.</span><span class="sxs-lookup"><span data-stu-id="1abc4-134">The check fails as you have a gateway assigned to a non-DATA 0 network interface and your device is running a software version prior to Update 1.</span></span>

<span data-ttu-id="1abc4-135">Verze softwaru, které lze upgradovat pomocí metody opravy hotfix jsou Update 0.1, 0.2, aktualizace a Update 0.3, Update 1, aktualizace 1.1 a 1.2 aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1abc4-135">The software versions that can be upgraded using the hotfix method are Update 0.1, Update 0.2, and Update 0.3, Update 1, Update 1.1, and Update 1.2.</span></span> <span data-ttu-id="1abc4-136">Metoda opravy hotfix zahrnuje následující tři kroky:</span><span class="sxs-lookup"><span data-stu-id="1abc4-136">The hotfix method involves the following three steps:</span></span>

* <span data-ttu-id="1abc4-137">Stažení opravy hotfix z katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="1abc4-137">Download the hotfixes from the Microsoft Update Catalog.</span></span>
* <span data-ttu-id="1abc4-138">Instalace a ověřte regulární režimu opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="1abc4-138">Install and verify the regular mode hotfixes.</span></span>
* <span data-ttu-id="1abc4-139">Instalace a ověřte opravu hotfix režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="1abc4-139">Install and verify the maintenance mode hotfix.</span></span>

<span data-ttu-id="1abc4-140">K instalaci aktualizace 2 jako oprava hotfix, musíte stáhnout a nainstalovat následující opravy hotfix:</span><span class="sxs-lookup"><span data-stu-id="1abc4-140">To install Update 2 as a hotfix, you must download and install the following hotfixes:</span></span>

| <span data-ttu-id="1abc4-141">Pořadí</span><span class="sxs-lookup"><span data-stu-id="1abc4-141">Order</span></span> | <span data-ttu-id="1abc4-142">kB</span><span class="sxs-lookup"><span data-stu-id="1abc4-142">KB</span></span> | <span data-ttu-id="1abc4-143">Popis</span><span class="sxs-lookup"><span data-stu-id="1abc4-143">Description</span></span> | <span data-ttu-id="1abc4-144">Typ aktualizace</span><span class="sxs-lookup"><span data-stu-id="1abc4-144">Update type</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1abc4-145">1</span><span class="sxs-lookup"><span data-stu-id="1abc4-145">1</span></span> |<span data-ttu-id="1abc4-146">KB3121901</span><span class="sxs-lookup"><span data-stu-id="1abc4-146">KB3121901</span></span> |<span data-ttu-id="1abc4-147">Aktualizace softwaru</span><span class="sxs-lookup"><span data-stu-id="1abc4-147">Software update</span></span> |<span data-ttu-id="1abc4-148">Regulární</span><span class="sxs-lookup"><span data-stu-id="1abc4-148">Regular</span></span> |
| <span data-ttu-id="1abc4-149">2</span><span class="sxs-lookup"><span data-stu-id="1abc4-149">2</span></span> |<span data-ttu-id="1abc4-150">KB3121900</span><span class="sxs-lookup"><span data-stu-id="1abc4-150">KB3121900</span></span> |<span data-ttu-id="1abc4-151">LSI ovladačů</span><span class="sxs-lookup"><span data-stu-id="1abc4-151">LSI driver</span></span> |<span data-ttu-id="1abc4-152">Regulární</span><span class="sxs-lookup"><span data-stu-id="1abc4-152">Regular</span></span> |
| <span data-ttu-id="1abc4-153">3</span><span class="sxs-lookup"><span data-stu-id="1abc4-153">3</span></span> |<span data-ttu-id="1abc4-154">KB3080728</span><span class="sxs-lookup"><span data-stu-id="1abc4-154">KB3080728</span></span> |<span data-ttu-id="1abc4-155">Oprava Storport</span><span class="sxs-lookup"><span data-stu-id="1abc4-155">Storport fix</span></span> </br> <span data-ttu-id="1abc4-156">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="1abc4-156">Windows Server 2012 R2</span></span> |<span data-ttu-id="1abc4-157">Regulární</span><span class="sxs-lookup"><span data-stu-id="1abc4-157">Regular</span></span> |
| <span data-ttu-id="1abc4-158">4</span><span class="sxs-lookup"><span data-stu-id="1abc4-158">4</span></span> |<span data-ttu-id="1abc4-159">KB3090322</span><span class="sxs-lookup"><span data-stu-id="1abc4-159">KB3090322</span></span> |<span data-ttu-id="1abc4-160">Oprava spaceport</span><span class="sxs-lookup"><span data-stu-id="1abc4-160">Spaceport fix</span></span> </br> <span data-ttu-id="1abc4-161">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="1abc4-161">Windows Server 2012 R2</span></span> |<span data-ttu-id="1abc4-162">Regulární</span><span class="sxs-lookup"><span data-stu-id="1abc4-162">Regular</span></span> |
| <span data-ttu-id="1abc4-163">5</span><span class="sxs-lookup"><span data-stu-id="1abc4-163">5</span></span> |<span data-ttu-id="1abc4-164">KB3121899</span><span class="sxs-lookup"><span data-stu-id="1abc4-164">KB3121899</span></span> |<span data-ttu-id="1abc4-165">Firmware disku</span><span class="sxs-lookup"><span data-stu-id="1abc4-165">Disk firmware</span></span> |<span data-ttu-id="1abc4-166">Údržby</span><span class="sxs-lookup"><span data-stu-id="1abc4-166">Maintenance</span></span> |

> [!IMPORTANT]
> * <span data-ttu-id="1abc4-167">Pokud zařízení používá verzi (GA) verze, kontaktujte prosím [Microsoft Support](storsimple-contact-microsoft-support.md) a pomůže vám při aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="1abc4-167">If your device is running Release (GA) version, please contact [Microsoft Support](storsimple-contact-microsoft-support.md) to assist you with the update.</span></span>
> * <span data-ttu-id="1abc4-168">Tento postup je nutné provést pouze jednou použít Update 2.</span><span class="sxs-lookup"><span data-stu-id="1abc4-168">This procedure needs to be performed only once to apply Update 2.</span></span> <span data-ttu-id="1abc4-169">Portál Azure classic můžete použít následující aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1abc4-169">You can use the Azure classic portal to apply subsequent updates.</span></span>
> * <span data-ttu-id="1abc4-170">Každou instalaci oprav hotfix může trvat asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="1abc4-170">Each hotfix installation can take about 20 minutes to complete.</span></span> <span data-ttu-id="1abc4-171">Čas celkový instalace je blízko 2 hodiny.</span><span class="sxs-lookup"><span data-stu-id="1abc4-171">Total install time is close to 2 hours.</span></span>
> * <span data-ttu-id="1abc4-172">Před použitím tohoto postupu použít aktualizaci, ujistěte se, že oba řadiče zařízení jsou online a jsou hardwarové součásti v pořádku.</span><span class="sxs-lookup"><span data-stu-id="1abc4-172">Before using this procedure to apply the update, make sure that both device controllers are online and all the hardware components are healthy.</span></span>
> 
> 

<span data-ttu-id="1abc4-173">Proveďte následující kroky k instalaci této aktualizace jako oprava hotfix.</span><span class="sxs-lookup"><span data-stu-id="1abc4-173">Perform the following steps to apply this update as a hotfix.</span></span>

[!INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a><span data-ttu-id="1abc4-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1abc4-174">Next steps</span></span>
<span data-ttu-id="1abc4-175">Další informace o [verzi Update 2](storsimple-update2-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="1abc4-175">Learn more about the [Update 2 release](storsimple-update2-release-notes.md).</span></span>
