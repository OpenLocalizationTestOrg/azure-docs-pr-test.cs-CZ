---
title: "aaaArchive diagnostických protokolů Azure | Microsoft Docs"
description: "Zjistěte, jak tooarchive vaše Azure diagnostické protokoly pro dlouhodobé uchovávání v účtu úložiště."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 3a55c73f-2ef3-45f3-8956-bcf9c0cb7e05
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: bc9edbd3a649023a728b7fe77130dba2b6e6370d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="archive-azure-diagnostic-logs"></a><span data-ttu-id="ef047-103">Archivovat Azure diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="ef047-103">Archive Azure Diagnostic Logs</span></span>
<span data-ttu-id="ef047-104">V tomto článku jsme ukazují, jak můžete použít hello portál Azure, rutiny prostředí PowerShell, rozhraní příkazového řádku, nebo REST API tooarchive vaše [Azure diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md) v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ef047-104">In this article, we show how you can use hello Azure portal, PowerShell Cmdlets, CLI, or REST API tooarchive your [Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md) in a storage account.</span></span> <span data-ttu-id="ef047-105">Tato možnost je užitečná, pokud byste chtěli tooretain vaše diagnostické protokoly zásadami volitelné uchování pro audit, statické analýzy nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="ef047-105">This option is useful if you would like tooretain your diagnostic logs with an optional retention policy for audit, static analysis, or backup.</span></span> <span data-ttu-id="ef047-106">účet úložiště Hello nemá toobe v hello stejného předplatného jako prostředek hello emitování protokoly tak dlouho, dokud hello uživatel, který konfiguruje nastavení hello má příslušné předplatné tooboth přístupu RBAC.</span><span class="sxs-lookup"><span data-stu-id="ef047-106">hello storage account does not have toobe in hello same subscription as hello resource emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef047-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ef047-107">Prerequisites</span></span>
<span data-ttu-id="ef047-108">Než začnete, je třeba příliš[vytvořit účet úložiště](../storage/storage-create-storage-account.md) toowhich archivujete diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="ef047-108">Before you begin, you need too[create a storage account](../storage/storage-create-storage-account.md) toowhich you can archive your diagnostic logs.</span></span> <span data-ttu-id="ef047-109">Důrazně doporučujeme stávající účet úložiště, který má jiné, -monitoring data v ní uloženy, takže může lépe řídit přístup k datům toomonitoring nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="ef047-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access toomonitoring data.</span></span> <span data-ttu-id="ef047-110">Ale pokud jsou také archivaci protokolu aktivit a účet úložiště diagnostiky metriky tooa, může být smysl toouse daný účet úložiště pro vaše diagnostické protokoly a tookeep všechna monitorování data v centrálním umístění.</span><span class="sxs-lookup"><span data-stu-id="ef047-110">However, if you are also archiving your Activity Log and diagnostic metrics tooa storage account, it may make sense toouse that storage account for your diagnostic logs as well tookeep all monitoring data in a central location.</span></span> <span data-ttu-id="ef047-111">Hello účet úložiště, které používáte, musí být účet úložiště obecné účely, nikoli účet úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="ef047-111">hello storage account you use must be a general purpose storage account, not a blob storage account.</span></span>

## <a name="diagnostic-settings"></a><span data-ttu-id="ef047-112">Nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="ef047-112">Diagnostic settings</span></span>
<span data-ttu-id="ef047-113">tooarchive vaše diagnostické protokoly, pomocí metod hello níže, nastavíte **nastavení diagnostiky** pro určitý prostředek.</span><span class="sxs-lookup"><span data-stu-id="ef047-113">tooarchive your diagnostic logs using any of hello methods below, you set a **diagnostic setting** for a particular resource.</span></span> <span data-ttu-id="ef047-114">Nastavení diagnostiky pro prostředek definuje kategorie hello protokolů a metriky data odeslána tooa cílového (účet úložiště, obor názvů služby Event Hubs nebo analýzy protokolů).</span><span class="sxs-lookup"><span data-stu-id="ef047-114">A diagnostic setting for a resource defines hello categories of logs and metric data sent tooa destination (storage account, Event Hubs namespace, or Log Analytics).</span></span> <span data-ttu-id="ef047-115">Definuje také zásady uchovávání informací hello (počet dní tooretain) pro události pro každé kategorie protokolu a metriky data uložená v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ef047-115">It also defines hello retention policy (number of days tooretain) for events of each log category and metric data stored in a storage account.</span></span> <span data-ttu-id="ef047-116">Pokud zásady uchovávání informací nastavena toozero, události pro tuto kategorii protokolu se ukládají po neomezenou dobu (které je toosay, trvale).</span><span class="sxs-lookup"><span data-stu-id="ef047-116">If a retention policy is set toozero, events for that log category are stored indefinitely (that is toosay, forever).</span></span> <span data-ttu-id="ef047-117">Zásady uchovávání informací v opačném případě může být libovolný počet dnů od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="ef047-117">A retention policy can otherwise be any number of days between 1 and 2147483647.</span></span> <span data-ttu-id="ef047-118">[Další informace o nastavení pro diagnostiku zde](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="ef047-118">[You can read more about diagnostic settings here](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span> <span data-ttu-id="ef047-119">Zásady uchovávání informací jsou použité za den, tak při hello Konec dne (UTC), zaprotokoluje hello dni, který je teď nad rámec zásad uchovávání informací hello budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="ef047-119">Retention policies are applied per-day, so at hello end of a day (UTC), logs from hello day that is now beyond hello retention policy will be deleted.</span></span> <span data-ttu-id="ef047-120">Například pokud jste měli zásady uchovávání informací jeden den, od začátku hello dnešní den hello hello protokoly z hello den před včerejšek by odstraněn</span><span class="sxs-lookup"><span data-stu-id="ef047-120">For example, if you had a retention policy of one day, at hello beginning of hello day today hello logs from hello day before yesterday would be deleted</span></span>

## <a name="archive-diagnostic-logs-using-hello-portal"></a><span data-ttu-id="ef047-121">Diagnostické protokoly archivu pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="ef047-121">Archive diagnostic logs using hello portal</span></span>
1. <span data-ttu-id="ef047-122">Hello portálu, přejděte tooAzure monitorování a klikněte na **nastavení diagnostiky**</span><span class="sxs-lookup"><span data-stu-id="ef047-122">In hello portal, navigate tooAzure Monitor and click on **Diagnostic Settings**</span></span>

    ![Monitorování části monitorování Azure](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. <span data-ttu-id="ef047-124">Volitelně můžete filtrovat seznam hello skupinu prostředků nebo typ prostředku, a potom klikněte na hello prostředků, pro kterou chcete tooset nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="ef047-124">Optionally filter hello list by resource group or resource type, then click on hello resource for which you would like tooset a diagnostic setting.</span></span>

3. <span data-ttu-id="ef047-125">Pokud neexistuje žádné nastavení na hello prostředku, který jste zvolili, jste výzvami toocreate nastavení.</span><span class="sxs-lookup"><span data-stu-id="ef047-125">If no settings exist on hello resource you have selected, you are prompted toocreate a setting.</span></span> <span data-ttu-id="ef047-126">Klikněte na tlačítko "Zapnout diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="ef047-126">Click "Turn on diagnostics."</span></span>

   ![Přidat nastavení diagnostiky - žádná existující nastavení](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   <span data-ttu-id="ef047-128">Pokud máte stávající nastavení na hello prostředku, zobrazí se seznam nastavení, které jsou již nakonfigurována na tomto prostředku.</span><span class="sxs-lookup"><span data-stu-id="ef047-128">If there are existing settings on hello resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="ef047-129">Klikněte na tlačítko "Přidat nastavení diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="ef047-129">Click "Add diagnostic setting."</span></span>

   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="ef047-131">Zadejte název nastavení a zaškrtněte políčko hello pro **exportovat tooStorage účet**, pak vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="ef047-131">Give your setting a name and check hello box for **Export tooStorage Account**, then select a storage account.</span></span> <span data-ttu-id="ef047-132">Volitelně můžete nastavit počet dní tooretain tyto protokoly s použitím hello **uchovávání dat (dny)** posuvníky.</span><span class="sxs-lookup"><span data-stu-id="ef047-132">Optionally, set a number of days tooretain these logs by using hello **Retention (days)** sliders.</span></span> <span data-ttu-id="ef047-133">Uchování nulový počet dnů po neomezenou dobu ukládá protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="ef047-133">A retention of zero days stores hello logs indefinitely.</span></span>
   
   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. <span data-ttu-id="ef047-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ef047-135">Click **Save**.</span></span>

<span data-ttu-id="ef047-136">Po chvíli se hello nové nastavení se zobrazí v seznamu nastavení pro tento prostředek a diagnostické protokoly jsou archivovaný toothat úložiště účet, jakmile se vygeneruje nová data události.</span><span class="sxs-lookup"><span data-stu-id="ef047-136">After a few moments, hello new setting appears in your list of settings for this resource, and diagnostic logs are archived toothat storage account as soon as new event data is generated.</span></span>

## <a name="archive-diagnostic-logs-via-azure-powershell"></a><span data-ttu-id="ef047-137">Archiv diagnostických protokolů pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef047-137">Archive diagnostic logs via Azure PowerShell</span></span>
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| <span data-ttu-id="ef047-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ef047-138">Property</span></span> | <span data-ttu-id="ef047-139">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ef047-139">Required</span></span> | <span data-ttu-id="ef047-140">Popis</span><span class="sxs-lookup"><span data-stu-id="ef047-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ef047-141">ID prostředku</span><span class="sxs-lookup"><span data-stu-id="ef047-141">ResourceId</span></span> |<span data-ttu-id="ef047-142">Ano</span><span class="sxs-lookup"><span data-stu-id="ef047-142">Yes</span></span> |<span data-ttu-id="ef047-143">ID prostředku hello prostředku, na kterém chcete tooset nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="ef047-143">Resource ID of hello resource on which you want tooset a diagnostic setting.</span></span> |
| <span data-ttu-id="ef047-144">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="ef047-144">StorageAccountId</span></span> |<span data-ttu-id="ef047-145">Ne</span><span class="sxs-lookup"><span data-stu-id="ef047-145">No</span></span> |<span data-ttu-id="ef047-146">ID prostředku hello účet úložiště toowhich diagnostických protokolů má být uložen.</span><span class="sxs-lookup"><span data-stu-id="ef047-146">Resource ID of hello Storage Account toowhich Diagnostic Logs should be saved.</span></span> |
| <span data-ttu-id="ef047-147">Kategorie</span><span class="sxs-lookup"><span data-stu-id="ef047-147">Categories</span></span> |<span data-ttu-id="ef047-148">Ne</span><span class="sxs-lookup"><span data-stu-id="ef047-148">No</span></span> |<span data-ttu-id="ef047-149">Textový soubor s oddělovači seznam kategorií tooenable protokolu.</span><span class="sxs-lookup"><span data-stu-id="ef047-149">Comma-separated list of log categories tooenable.</span></span> |
| <span data-ttu-id="ef047-150">Povoleno</span><span class="sxs-lookup"><span data-stu-id="ef047-150">Enabled</span></span> |<span data-ttu-id="ef047-151">Ano</span><span class="sxs-lookup"><span data-stu-id="ef047-151">Yes</span></span> |<span data-ttu-id="ef047-152">Logická hodnota, která určuje, zda diagnostiky jsou zapnutá nebo vypnutá u tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="ef047-152">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |
| <span data-ttu-id="ef047-153">RetentionEnabled</span><span class="sxs-lookup"><span data-stu-id="ef047-153">RetentionEnabled</span></span> |<span data-ttu-id="ef047-154">Ne</span><span class="sxs-lookup"><span data-stu-id="ef047-154">No</span></span> |<span data-ttu-id="ef047-155">Logická hodnota, která udává, pokud jsou zásady uchovávání informací na tento prostředek povolené.</span><span class="sxs-lookup"><span data-stu-id="ef047-155">Boolean indicating if a retention policy are enabled on this resource.</span></span> |
| <span data-ttu-id="ef047-156">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="ef047-156">RetentionInDays</span></span> |<span data-ttu-id="ef047-157">Ne</span><span class="sxs-lookup"><span data-stu-id="ef047-157">No</span></span> |<span data-ttu-id="ef047-158">Počet dní, pro které by měl být zachován události od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="ef047-158">Number of days for which events should be retained between 1 and 2147483647.</span></span> <span data-ttu-id="ef047-159">Hodnota nula ukládá protokoly hello po neomezenou dobu.</span><span class="sxs-lookup"><span data-stu-id="ef047-159">A value of zero stores hello logs indefinitely.</span></span> |

## <a name="archive-diagnostic-logs-via-hello-cross-platform-cli"></a><span data-ttu-id="ef047-160">Diagnostické protokoly archivu prostřednictvím hello napříč platformami rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="ef047-160">Archive diagnostic logs via hello Cross-Platform CLI</span></span>
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| <span data-ttu-id="ef047-161">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ef047-161">Property</span></span> | <span data-ttu-id="ef047-162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ef047-162">Required</span></span> | <span data-ttu-id="ef047-163">Popis</span><span class="sxs-lookup"><span data-stu-id="ef047-163">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ef047-164">resourceId</span><span class="sxs-lookup"><span data-stu-id="ef047-164">resourceId</span></span> |<span data-ttu-id="ef047-165">Ano</span><span class="sxs-lookup"><span data-stu-id="ef047-165">Yes</span></span> |<span data-ttu-id="ef047-166">ID prostředku hello prostředku, na kterém chcete tooset nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="ef047-166">Resource ID of hello resource on which you want tooset a diagnostic setting.</span></span> |
| <span data-ttu-id="ef047-167">storageId</span><span class="sxs-lookup"><span data-stu-id="ef047-167">storageId</span></span> |<span data-ttu-id="ef047-168">Ne</span><span class="sxs-lookup"><span data-stu-id="ef047-168">No</span></span> |<span data-ttu-id="ef047-169">ID prostředku hello účet úložiště toowhich diagnostických protokolů má být uložen.</span><span class="sxs-lookup"><span data-stu-id="ef047-169">Resource ID of hello Storage Account toowhich diagnostic logs should be saved.</span></span> |
| <span data-ttu-id="ef047-170">Kategorie</span><span class="sxs-lookup"><span data-stu-id="ef047-170">categories</span></span> |<span data-ttu-id="ef047-171">Ne</span><span class="sxs-lookup"><span data-stu-id="ef047-171">No</span></span> |<span data-ttu-id="ef047-172">Textový soubor s oddělovači seznam kategorií tooenable protokolu.</span><span class="sxs-lookup"><span data-stu-id="ef047-172">Comma-separated list of log categories tooenable.</span></span> |
| <span data-ttu-id="ef047-173">povoleno</span><span class="sxs-lookup"><span data-stu-id="ef047-173">enabled</span></span> |<span data-ttu-id="ef047-174">Ano</span><span class="sxs-lookup"><span data-stu-id="ef047-174">Yes</span></span> |<span data-ttu-id="ef047-175">Logická hodnota, která určuje, zda diagnostiky jsou zapnutá nebo vypnutá u tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="ef047-175">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |

## <a name="archive-diagnostic-logs-via-hello-rest-api"></a><span data-ttu-id="ef047-176">Diagnostické protokoly archivu prostřednictvím hello REST API</span><span class="sxs-lookup"><span data-stu-id="ef047-176">Archive diagnostic logs via hello REST API</span></span>
<span data-ttu-id="ef047-177">[Najdete v tomto dokumentu](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) informace o tom, jak můžete nastavit pomocí hello REST API služby Azure monitorování diagnostické nastavení.</span><span class="sxs-lookup"><span data-stu-id="ef047-177">[See this document](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) for information on how you can set up a diagnostic setting using hello Azure Monitor REST API.</span></span>

## <a name="schema-of-diagnostic-logs-in-hello-storage-account"></a><span data-ttu-id="ef047-178">Schéma diagnostických protokolů v účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="ef047-178">Schema of diagnostic logs in hello storage account</span></span>
<span data-ttu-id="ef047-179">Jakmile jste nastavili archivace, kontejner úložiště se vytvoří v účtu úložiště hello, jakmile dojde k události v jedné z hello protokolu kategorií, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="ef047-179">Once you have set up archival, a storage container is created in hello storage account as soon as an event occurs in one of hello log categories you have enabled.</span></span> <span data-ttu-id="ef047-180">Hello objektů BLOB v kontejneru hello podle hello stejný formát napříč diagnostické protokoly a hello protokol aktivit.</span><span class="sxs-lookup"><span data-stu-id="ef047-180">hello blobs within hello container follow hello same format across Diagnostic Logs and hello Activity Log.</span></span> <span data-ttu-id="ef047-181">Struktura Hello tyto objekty BLOB je:</span><span class="sxs-lookup"><span data-stu-id="ef047-181">hello structure of these blobs is:</span></span>

> <span data-ttu-id="ef047-182">insights - logs-{název kategorie protokolu} / resourceId = v odběry / {ID předplatného} /RESOURCEGROUPS/ {název skupiny prostředků} /PROVIDERS/ {název zprostředkovatele prostředků} / {resource type} nebo {název prostředku} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin</span><span class="sxs-lookup"><span data-stu-id="ef047-182">insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="ef047-183">Nebo více jednoduše</span><span class="sxs-lookup"><span data-stu-id="ef047-183">Or, more simply,</span></span>

> <span data-ttu-id="ef047-184">insights - logs-{název kategorie protokolu} / resourceId = / {Id prostředku} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin</span><span class="sxs-lookup"><span data-stu-id="ef047-184">insights-logs-{log category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="ef047-185">Například může být název objektu blob:</span><span class="sxs-lookup"><span data-stu-id="ef047-185">For example, a blob name might be:</span></span>

> <span data-ttu-id="ef047-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="ef047-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="ef047-187">Každý objekt blob PT1H.json obsahuje objekt blob JSON událostí, které nastaly během hodiny hello zadaný v adrese URL objektu blob hello (například h = 12).</span><span class="sxs-lookup"><span data-stu-id="ef047-187">Each PT1H.json blob contains a JSON blob of events that occurred within hello hour specified in hello blob URL (for example, h=12).</span></span> <span data-ttu-id="ef047-188">Během hello přítomen hodinu události jsou připojením toohello PT1H.json souboru, kdy k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="ef047-188">During hello present hour, events are appended toohello PT1H.json file as they occur.</span></span> <span data-ttu-id="ef047-189">Hello minutu hodnotu (m = 00) je vždy 00, protože protokolů diagnostiky události jsou rozdělená do jednotlivých objektů blob za hodinu.</span><span class="sxs-lookup"><span data-stu-id="ef047-189">hello minute value (m=00) is always 00, since diagnostic log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="ef047-190">V souboru PT1H.json hello se ukládají všechny události v poli záznamů"hello", následující tento formát:</span><span class="sxs-lookup"><span data-stu-id="ef047-190">Within hello PT1H.json file, each event is stored in hello “records” array, following this format:</span></span>

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| <span data-ttu-id="ef047-191">Název elementu</span><span class="sxs-lookup"><span data-stu-id="ef047-191">Element name</span></span> | <span data-ttu-id="ef047-192">Popis</span><span class="sxs-lookup"><span data-stu-id="ef047-192">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ef047-193">time</span><span class="sxs-lookup"><span data-stu-id="ef047-193">time</span></span> |<span data-ttu-id="ef047-194">Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost.</span><span class="sxs-lookup"><span data-stu-id="ef047-194">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="ef047-195">resourceId</span><span class="sxs-lookup"><span data-stu-id="ef047-195">resourceId</span></span> |<span data-ttu-id="ef047-196">ID prostředku hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="ef047-196">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="ef047-197">operationName</span><span class="sxs-lookup"><span data-stu-id="ef047-197">operationName</span></span> |<span data-ttu-id="ef047-198">Název operace hello.</span><span class="sxs-lookup"><span data-stu-id="ef047-198">Name of hello operation.</span></span> |
| <span data-ttu-id="ef047-199">category</span><span class="sxs-lookup"><span data-stu-id="ef047-199">category</span></span> |<span data-ttu-id="ef047-200">Kategorie protokolu události hello.</span><span class="sxs-lookup"><span data-stu-id="ef047-200">Log category of hello event.</span></span> |
| <span data-ttu-id="ef047-201">properties</span><span class="sxs-lookup"><span data-stu-id="ef047-201">properties</span></span> |<span data-ttu-id="ef047-202">Sada `<Key, Value>` páry (tj. slovník) popisující hello podrobnosti události hello.</span><span class="sxs-lookup"><span data-stu-id="ef047-202">Set of `<Key, Value>` pairs (i.e. Dictionary) describing hello details of hello event.</span></span> |

> [!NOTE]
> <span data-ttu-id="ef047-203">Hello vlastnosti a využití tyto vlastnosti se může lišit v závislosti na hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="ef047-203">hello properties and usage of those properties can vary depending on hello resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="ef047-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef047-204">Next steps</span></span>
* [<span data-ttu-id="ef047-205">Stáhnout objekty BLOB pro analýzu</span><span class="sxs-lookup"><span data-stu-id="ef047-205">Download blobs for analysis</span></span>](../storage/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="ef047-206">Datový proud diagnostické protokoly obor názvů tooan Event Hubs</span><span class="sxs-lookup"><span data-stu-id="ef047-206">Stream diagnostic logs tooan Event Hubs namespace</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [<span data-ttu-id="ef047-207">Další informace o diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="ef047-207">Read more about diagnostic logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
