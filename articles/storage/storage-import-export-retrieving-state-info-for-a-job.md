---
title: "Načítání informací o stavu pro úlohu Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak získat informace o stavu úloh služby Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 22d7e5f0-94da-49b4-a1ac-dd4c14a423c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
ms.openlocfilehash: 13169716c47cf9389c8f2651393ac744441bdd6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="retrieving-state-information-for-an-importexport-job"></a><span data-ttu-id="bee52-103">Načítání informací o stavu pro úlohu importu/exportu</span><span class="sxs-lookup"><span data-stu-id="bee52-103">Retrieving state information for an Import/Export job</span></span>
<span data-ttu-id="bee52-104">Můžete volat [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operace k načtení informací o obě import a export úloh.</span><span class="sxs-lookup"><span data-stu-id="bee52-104">You can call the [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation to retrieve information about both import and export jobs.</span></span> <span data-ttu-id="bee52-105">Vrátí následující informace:</span><span class="sxs-lookup"><span data-stu-id="bee52-105">The information returned includes:</span></span>

-   <span data-ttu-id="bee52-106">Aktuální stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="bee52-106">The current status of the job.</span></span>

-   <span data-ttu-id="bee52-107">Přibližná procento, každá úloha se dokončila.</span><span class="sxs-lookup"><span data-stu-id="bee52-107">The approximate percentage that each job has been completed.</span></span>

-   <span data-ttu-id="bee52-108">Aktuální stav každé jednotky.</span><span class="sxs-lookup"><span data-stu-id="bee52-108">The current state of each drive.</span></span>

-   <span data-ttu-id="bee52-109">Identifikátory URI pro objekty BLOB obsahující protokoly chyb a podrobné informace o protokolování (Pokud je povoleno).</span><span class="sxs-lookup"><span data-stu-id="bee52-109">URIs for blobs containing error logs and verbose logging information (if enabled).</span></span>

<span data-ttu-id="bee52-110">Následující části popisují informace o vrácené `Get Job` operace.</span><span class="sxs-lookup"><span data-stu-id="bee52-110">The following sections explain the information returned by the `Get Job` operation.</span></span>

## <a name="job-states"></a><span data-ttu-id="bee52-111">Stavy úlohy</span><span class="sxs-lookup"><span data-stu-id="bee52-111">Job states</span></span>
<span data-ttu-id="bee52-112">V tabulce a stavu obrázek popisují stavy, které úloha přejde prostřednictvím během jejich životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="bee52-112">The table and the state diagram below describe the states that a job transitions through during its life cycle.</span></span> <span data-ttu-id="bee52-113">Aktuální stav úlohy můžete určit voláním `Get Job` operaci.</span><span class="sxs-lookup"><span data-stu-id="bee52-113">The current state of the job can be determined by calling the `Get Job` operation.</span></span>

<span data-ttu-id="bee52-114">![JobStates](./media/storage-import-export-retrieving-state-info-for-a-job/JobStates.png "JobStates")</span><span class="sxs-lookup"><span data-stu-id="bee52-114">![JobStates](./media/storage-import-export-retrieving-state-info-for-a-job/JobStates.png "JobStates")</span></span>

<span data-ttu-id="bee52-115">Následující tabulka popisuje všechny stavy, které může předávat úlohu.</span><span class="sxs-lookup"><span data-stu-id="bee52-115">The following table describes each state that a job may pass through.</span></span>

|<span data-ttu-id="bee52-116">Stav úlohy</span><span class="sxs-lookup"><span data-stu-id="bee52-116">Job State</span></span>|<span data-ttu-id="bee52-117">Popis</span><span class="sxs-lookup"><span data-stu-id="bee52-117">Description</span></span>|
|---------------|-----------------|
|`Creating`|<span data-ttu-id="bee52-118">Po volání operace Put úlohy, vytvoření úlohy a její stav je nastavený na `Creating`.</span><span class="sxs-lookup"><span data-stu-id="bee52-118">After you call the Put Job operation, a job is created and its state is set to `Creating`.</span></span> <span data-ttu-id="bee52-119">Když úloha `Creating` stavu, službu Import/Export předpokládá jednotky nebyly byla odeslaná do datového centra.</span><span class="sxs-lookup"><span data-stu-id="bee52-119">While the job is in the `Creating` state, the Import/Export service assumes the drives have not been shipped to the data center.</span></span> <span data-ttu-id="bee52-120">Úlohu může zůstat v `Creating` dobu až dvou týdnů, po které se automaticky odstraní službou stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-120">An job may remain in the `Creating` state for up to two weeks, after which it is automatically deleted by the service.</span></span><br /><br /> <span data-ttu-id="bee52-121">Pokud provádíte volání operace vlastnosti úlohy aktualizace během úlohy `Creating` stavu úlohy zůstane v `Creating` stavu a interval vypršení časového limitu je obnovena na dva týdny.</span><span class="sxs-lookup"><span data-stu-id="bee52-121">If you call the Update Job Properties operation while the job is in the `Creating` state, the job remains in the `Creating` state, and the timeout interval is reset to two weeks.</span></span>|
|`Shipping`|<span data-ttu-id="bee52-122">Po dodáte vašeho balíčku, měli byste zavolat operaci aktualizace aktualizovat vlastnosti úlohy stav úlohy `Shipping`.</span><span class="sxs-lookup"><span data-stu-id="bee52-122">After you ship your package, you should call the Update Job Properties operation update the state of the job to `Shipping`.</span></span> <span data-ttu-id="bee52-123">Stav přesouvání lze nastavit pouze v případě `DeliveryPackage` (poštovní poskytovatel a sledování Číslo) a `ReturnAddress` byly nastaveny pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="bee52-123">The shipping state can be set only if the `DeliveryPackage` (postal carrier and tracking number) and the `ReturnAddress` properties have been set for the job.</span></span><br /><br /> <span data-ttu-id="bee52-124">Úloha zůstane ve stavu přesouvání dobu až dvou týdnů.</span><span class="sxs-lookup"><span data-stu-id="bee52-124">The job will remain in the Shipping state for up to two weeks.</span></span> <span data-ttu-id="bee52-125">Pokud byly úspěšně dva týdny, a jednotky nebyly přijaty, operátory službu Import/Export budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="bee52-125">If two weeks have passed and the drives have not been received, the Import/Export service operators will be notified.</span></span>|
|`Received`|<span data-ttu-id="bee52-126">Po všechny jednotky byly přijaty v datovém centru, stav úlohy bude nastavit stav přijaté.</span><span class="sxs-lookup"><span data-stu-id="bee52-126">After all drives have been received at the data center, the job state will be set to the Received state.</span></span>|
|`Transferring`|<span data-ttu-id="bee52-127">Poté, co byly přijaty jednotky v datovém centru a alespoň jednu jednotku zahájení zpracování, stav úlohy je možnost `Transferring` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-127">After the drives have been received at the data center and at least one drive has begun processing, the job state will be set to the `Transferring` state.</span></span> <span data-ttu-id="bee52-128">Najdete v článku `Drive States` části níže podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="bee52-128">See the `Drive States` section below for detailed information.</span></span>|
|`Packaging`|<span data-ttu-id="bee52-129">Po dokončení zpracování všech jednotkách, úloha bude uložena v umístění `Packaging` stavu, dokud jednotky jsou sice zpět na zákazníka.</span><span class="sxs-lookup"><span data-stu-id="bee52-129">After all drives have completed processing, the job will be placed in the `Packaging` state until the drives are shipped back to the customer.</span></span>|
|`Completed`|<span data-ttu-id="bee52-130">Po všechny jednotky byly dodány zpět na zákazníka, pokud úloha byla dokončena bez chyb, potom úlohu se nastaví `Completed` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-130">After all drives have been shipped back to the customer, if the job has completed without errors, then the job will be set to the `Completed` state.</span></span> <span data-ttu-id="bee52-131">Úlohy budou automaticky odstraněny po 90 dnech v `Completed` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-131">The job will be automatically deleted after 90 days in the `Completed` state.</span></span>|
|`Closed`|<span data-ttu-id="bee52-132">Po všechny jednotky byly dodány zpět na zákazníka, pokud zde nejsou žádné chyby během zpracování úlohy, potom úlohu se nastaví `Closed` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-132">After all drives have been shipped back to the customer, if there have been any errors during the processing of the job, then the job will be set to the `Closed` state.</span></span> <span data-ttu-id="bee52-133">Úlohy budou automaticky odstraněny po 90 dnech v `Closed` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-133">The job will be automatically deleted after 90 days in the `Closed` state.</span></span>|

<span data-ttu-id="bee52-134">Můžete zrušit úlohu jenom na určité stavy.</span><span class="sxs-lookup"><span data-stu-id="bee52-134">You can cancel a job only at certain states.</span></span> <span data-ttu-id="bee52-135">Zrušené úlohy přeskočí krok kopie dat, ale jinak postupuje stejné přechodů mezi stavy jako úlohu, která nebyla zrušena.</span><span class="sxs-lookup"><span data-stu-id="bee52-135">A cancelled job skips the data copy step, but otherwise it follows the same state transitions as a job that was not cancelled.</span></span>

<span data-ttu-id="bee52-136">Následující tabulka popisuje chyby, které se můžou vyskytnout pro každý stav úlohy a také vliv na úlohu, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="bee52-136">The following table describes errors that can occur for each job state, as well as the effect on the job when an error occurs.</span></span>

|<span data-ttu-id="bee52-137">Stav úlohy</span><span class="sxs-lookup"><span data-stu-id="bee52-137">Job State</span></span>|<span data-ttu-id="bee52-138">Událost</span><span class="sxs-lookup"><span data-stu-id="bee52-138">Event</span></span>|<span data-ttu-id="bee52-139">Řešení / další kroky</span><span class="sxs-lookup"><span data-stu-id="bee52-139">Resolution / Next Steps</span></span>|
|---------------|-----------|------------------------------|
|`Creating or Undefined`|<span data-ttu-id="bee52-140">Byly přijaty jedné nebo více jednotek pro úlohu, ale není v úlohy `Shipping` stavu nebo není žádný záznam úlohy ve službě.</span><span class="sxs-lookup"><span data-stu-id="bee52-140">One or more drives for a job arrived, but the job is not in the `Shipping` state or there is no record of the job in the service.</span></span>|<span data-ttu-id="bee52-141">Provozní tým služby importu a exportu se vás pokusí kontaktovat zákazník vytvořit nebo aktualizovat úlohu informace nezbytné k přejít úlohy.</span><span class="sxs-lookup"><span data-stu-id="bee52-141">The Import/Export service operations team will attempt to contact the customer to create or update the job with necessary information to move the job forward.</span></span><br /><br /> <span data-ttu-id="bee52-142">Pokud je provozní tým nelze kontaktovat zákazník během dvou týdnů, provozní tým se pokusí vrátit jednotky.</span><span class="sxs-lookup"><span data-stu-id="bee52-142">If the operations team is unable to contact the customer within two weeks, the operations team will attempt to return the drives.</span></span><br /><br /> <span data-ttu-id="bee52-143">V případě, že jednotky nelze vrátit, a zákazník nelze získat přístup, jednotky bude bezpečným způsobem zničena za 90 dní.</span><span class="sxs-lookup"><span data-stu-id="bee52-143">In the event that the drives cannot be returned and the customer cannot be reached, the drives will be securely destroyed in 90 days.</span></span><br /><br /> <span data-ttu-id="bee52-144">Všimněte si, že úlohy nelze zpracovat, dokud jeho stav se aktualizuje na `Shipping`.</span><span class="sxs-lookup"><span data-stu-id="bee52-144">Note that a job cannot be processed until its state is updated to `Shipping`.</span></span>|
|`Shipping`|<span data-ttu-id="bee52-145">Balíček pro úlohu nebyl doručen více než dva týdny.</span><span class="sxs-lookup"><span data-stu-id="bee52-145">The package for a job has not arrived for over two weeks.</span></span>|<span data-ttu-id="bee52-146">Provozní tým oznámí zákazník chybějící balíčku.</span><span class="sxs-lookup"><span data-stu-id="bee52-146">The operations team will notify the customer of the missing package.</span></span> <span data-ttu-id="bee52-147">Podle zákazníka odpovědi, provozní tým se rozšířit intervalu čekání balíčku, který má doručení, nebo úlohu zrušit.</span><span class="sxs-lookup"><span data-stu-id="bee52-147">Based on the customer's response, the operations team will either extend the interval to wait for the package to arrive, or cancel the job.</span></span><br /><br /> <span data-ttu-id="bee52-148">V případě, že zákazník nelze kontaktovat nebo nereaguje do 30 dní, provozní tým zahájí akce přesouvat úlohy z `Shipping` přímo do stavu `Closed` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-148">In the event that the customer cannot be contacted or does not respond within 30 days, the operations team will initiate action to move the job from the `Shipping` state directly to the `Closed` state.</span></span>|
|`Completed/Closed`|<span data-ttu-id="bee52-149">Jednotky nikdy dosaženo zpáteční adresu nebo jsou poškozené v dodávky (platí pouze pro úlohy exportu).</span><span class="sxs-lookup"><span data-stu-id="bee52-149">The drives never reached the return address or were damaged in shipment (applies only to an export job).</span></span>|<span data-ttu-id="bee52-150">Pokud jednotky nedojde k dosažení zpáteční adresu, musí zákazník nejprve volání operace Get Job nebo zkontrolujte stav úlohy na portálu a ujistěte se, že bylo odesláno jednotky.</span><span class="sxs-lookup"><span data-stu-id="bee52-150">If the drives do not reach the return address, the customer should first call the Get Job operation or check the job status in the portal to ensure that the drives have been shipped.</span></span> <span data-ttu-id="bee52-151">Pokud bylo dodáno na discích, musí zákazník obraťte na poskytovatele přesouvání a zkuste najít jednotky.</span><span class="sxs-lookup"><span data-stu-id="bee52-151">If the drives have been shipped, then the customer should contact the shipping provider to try and locate the drives.</span></span><br /><br /> <span data-ttu-id="bee52-152">Pokud během dodávky jsou poškozené jednotky, Zákazník může chtít požádat jiná úloha exportu nebo stáhnout chybějící objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="bee52-152">If the drives are damaged during shipment, the customer may want to request another export job, or download the missing blobs.</span></span>|
|`Transferring/Packaging`|<span data-ttu-id="bee52-153">Úloha má nesprávnou nebo chybějící zpáteční adresu.</span><span class="sxs-lookup"><span data-stu-id="bee52-153">Job has an incorrect or missing return address.</span></span>|<span data-ttu-id="bee52-154">Provozní tým bude přístup ke kontaktní osoby pro úlohu správnou adresu získat.</span><span class="sxs-lookup"><span data-stu-id="bee52-154">The operations team will reach out to the contact person for the job to obtain the correct address.</span></span><br /><br /> <span data-ttu-id="bee52-155">V případě, že zákazník nedostupné, jednotky bude bezpečným způsobem zničena za 90 dní.</span><span class="sxs-lookup"><span data-stu-id="bee52-155">In the event that the customer cannot be reached, the drives will be securely destroyed in 90 days.</span></span>|
|`Creating / Shipping/ Transferring`|<span data-ttu-id="bee52-156">Disk, který se nenachází v seznamu jednotek určených k importu je součástí balíčku přesouvání.</span><span class="sxs-lookup"><span data-stu-id="bee52-156">A drive that does not appear in the list of drives to be imported is included in the shipping package.</span></span>|<span data-ttu-id="bee52-157">Další jednotky nebude zpracován a obnoví se zákazník při dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="bee52-157">The extra drives will not be processed, and will be returned to the customer when the job is completed.</span></span>|

## <a name="drive-states"></a><span data-ttu-id="bee52-158">Stavy jednotky</span><span class="sxs-lookup"><span data-stu-id="bee52-158">Drive states</span></span>
<span data-ttu-id="bee52-159">V tabulce a na obrázku níže popisují životní cyklus jednotlivé jednotky jako přechází prostřednictvím úlohu import nebo export.</span><span class="sxs-lookup"><span data-stu-id="bee52-159">The table and the diagram below describe the life cycle of an individual drive as it transitions through an import or export job.</span></span> <span data-ttu-id="bee52-160">Aktuální stav disku můžete načíst pomocí volání `Get Job` operace a zkontrolujete `State` element `DriveList` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bee52-160">You can retrieve the current drive state by calling the `Get Job` operation and inspecting the `State` element of the `DriveList` property.</span></span>

<span data-ttu-id="bee52-161">![DriveStates](./media/storage-import-export-retrieving-state-info-for-a-job/DriveStates.png "DriveStates")</span><span class="sxs-lookup"><span data-stu-id="bee52-161">![DriveStates](./media/storage-import-export-retrieving-state-info-for-a-job/DriveStates.png "DriveStates")</span></span>

<span data-ttu-id="bee52-162">Následující tabulka popisuje všechny stavy, které může předávat na jednotku.</span><span class="sxs-lookup"><span data-stu-id="bee52-162">The following table describes each state that a drive may pass through.</span></span>

|<span data-ttu-id="bee52-163">Stav disku</span><span class="sxs-lookup"><span data-stu-id="bee52-163">Drive State</span></span>|<span data-ttu-id="bee52-164">Popis</span><span class="sxs-lookup"><span data-stu-id="bee52-164">Description</span></span>|
|-----------------|-----------------|
|`Specified`|<span data-ttu-id="bee52-165">Pro úlohu importu při vytvoření úlohy v operaci Put úlohy je počáteční stav pro jednotku `Specified` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-165">For an import job, when the job is created with the Put Job operation, the initial state for a drive is the `Specified` state.</span></span> <span data-ttu-id="bee52-166">Pro úlohy exportu, protože není zadána žádná jednotka při vytvoření úlohy, stav počáteční jednotky je `Received` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-166">For an export job, since no drive is specified when the job is created, the initial drive state is the `Received` state.</span></span>|
|`Received`|<span data-ttu-id="bee52-167">Jednotka přechází do `Received` stavu, při importu a exportu služby operátor zpracovala jednotky, které bylo přijato z společnosti přesouvání úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="bee52-167">The drive transitions to the `Received` state when the Import/Export service operator has processed the drives that were received from the shipping company for an import job.</span></span> <span data-ttu-id="bee52-168">Pro úlohy exportu, je stav počáteční jednotku `Received` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-168">For an export job, the initial drive state is the `Received` state.</span></span>|
|`NeverReceived`|<span data-ttu-id="bee52-169">Jednotka přesune do `NeverReceived` stav, když dorazí balíčku pro úlohy, ale balíček neobsahuje jednotku.</span><span class="sxs-lookup"><span data-stu-id="bee52-169">The drive will move to the `NeverReceived` state when the package for a job arrives but the package doesn't contain the drive.</span></span> <span data-ttu-id="bee52-170">Jednotku také můžete přesunout do tohoto stavu, pokud to bylo dva týdny, protože služba přijala přesouvání informace, ale balíček nebyl ještě přijaty v datovém centru.</span><span class="sxs-lookup"><span data-stu-id="bee52-170">A drive can also move into this state if it has been two weeks since the service received the shipping information, but the package has not yet arrived at the data center.</span></span>|
|`Transferring`|<span data-ttu-id="bee52-171">Bude přesouvat na jednotku `Transferring` stavu, kdy začne služba přenosu dat z jednotky do služby Windows Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bee52-171">A drive will move to the `Transferring` state when the service begins to transfer data from the drive to Windows Azure Storage.</span></span>|
|`Completed`|<span data-ttu-id="bee52-172">Bude přesouvat na jednotku `Completed` stavu, když má služba úspěšně přenesla všechna data bez chyb.</span><span class="sxs-lookup"><span data-stu-id="bee52-172">A drive will move to the `Completed` state when the service has successfully transferred all the data with no errors.</span></span>|
|`CompletedMoreInfo`|<span data-ttu-id="bee52-173">Bude přesouvat na jednotku `CompletedMoreInfo` stavu, kdy služba narazila některé problémy při kopírování dat z nebo na jednotku.</span><span class="sxs-lookup"><span data-stu-id="bee52-173">A drive will move to the `CompletedMoreInfo` state when the service has encountered some issues while copying data either from or to the drive.</span></span> <span data-ttu-id="bee52-174">Informace může obsahovat chyby, upozornění a informativní zprávy o přepsání objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="bee52-174">The information can include errors, warnings, or informational messages about overwriting blobs.</span></span>|
|`ShippedBack`|<span data-ttu-id="bee52-175">Jednotka přesune do `ShippedBack` stav, když ho byl dodán z center zálohování dat zpáteční adresu.</span><span class="sxs-lookup"><span data-stu-id="bee52-175">The drive will move to the `ShippedBack` state when it has been shipped from the data center back to the return address.</span></span>|

<span data-ttu-id="bee52-176">Následující tabulka popisuje stavy selhání jednotky a akcí provedených pro každý stav.</span><span class="sxs-lookup"><span data-stu-id="bee52-176">The following table describes the drive failure states and the actions taken for each state.</span></span>

|<span data-ttu-id="bee52-177">Stav disku</span><span class="sxs-lookup"><span data-stu-id="bee52-177">Drive State</span></span>|<span data-ttu-id="bee52-178">Událost</span><span class="sxs-lookup"><span data-stu-id="bee52-178">Event</span></span>|<span data-ttu-id="bee52-179">Řešení / další krok</span><span class="sxs-lookup"><span data-stu-id="bee52-179">Resolution / Next step</span></span>|
|-----------------|-----------|-----------------------------|
|`NeverReceived`|<span data-ttu-id="bee52-180">Jednotku, která je označena jako `NeverReceived` (protože nebyla přijata jako součást úlohy dodávky) dorazí v jiné dodávky.</span><span class="sxs-lookup"><span data-stu-id="bee52-180">A drive that is marked as `NeverReceived` (because it was not received as part of the job's shipment) arrives in another shipment.</span></span>|<span data-ttu-id="bee52-181">Provozní tým přesune jednotky a `Received` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-181">The operations team will move the drive to the `Received` state.</span></span>|
|`N/A`|<span data-ttu-id="bee52-182">Jednotku, která není součástí všechny úlohy dorazí v datovém centru jako součást jiná úloha.</span><span class="sxs-lookup"><span data-stu-id="bee52-182">A drive that is not part of any job arrives at the data center as part of another job.</span></span>|<span data-ttu-id="bee52-183">Jednotka budou označeny jako další jednotky a obnoví se zákazník při dokončení úlohy spojené s původní balíčku.</span><span class="sxs-lookup"><span data-stu-id="bee52-183">The drive will be marked as an extra drive and will be returned to the customer when the job associated with the original package is completed.</span></span>|

## <a name="faulted-states"></a><span data-ttu-id="bee52-184">Chybný stav</span><span class="sxs-lookup"><span data-stu-id="bee52-184">Faulted states</span></span>
<span data-ttu-id="bee52-185">Pokud úlohy nebo jednotka nezdaří normálně projděte očekávané celou dobu životního cyklu, úlohy nebo jednotka bude přesunuta do `Faulted` stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-185">When a job or drive fails to progress normally through its expected life cycle, the job or drive will be moved into a `Faulted` state.</span></span> <span data-ttu-id="bee52-186">V tomto bodě provozní tým vás bude kontaktovat zákazník e-mailem nebo telefon.</span><span class="sxs-lookup"><span data-stu-id="bee52-186">At that point, the operations team will contact the customer by email or phone.</span></span> <span data-ttu-id="bee52-187">Jakmile je problém vyřešen, chybný úlohy nebo jednotka se provedou mimo `Faulted` stavu a přesouvat do příslušné stavu.</span><span class="sxs-lookup"><span data-stu-id="bee52-187">Once the issue is resolved, the faulted job or drive will be taken out of the `Faulted` state and moved into the appropriate state.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bee52-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bee52-188">Next steps</span></span>

* [<span data-ttu-id="bee52-189">Pomocí REST API služby importu a exportu</span><span class="sxs-lookup"><span data-stu-id="bee52-189">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)