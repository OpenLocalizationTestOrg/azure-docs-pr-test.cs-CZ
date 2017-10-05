---
title: "Archivovat Azure diagnostické protokoly | Microsoft Docs"
description: "Zjistěte, jak k archivaci k diagnostickým protokolům Azure pro dlouhodobé uchovávání v účtu úložiště."
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
ms.openlocfilehash: dbc5f89001dcb6cd1ab061cb0a9632e4e5d2c1c7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="archive-azure-diagnostic-logs"></a><span data-ttu-id="28bd3-103">Archivovat Azure diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="28bd3-103">Archive Azure Diagnostic Logs</span></span>
<span data-ttu-id="28bd3-104">V tomto článku jsme ukazují, jak pomocí portálu Azure, rutiny prostředí PowerShell, rozhraní příkazového řádku nebo REST API pro archivaci vaše [Azure diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md) v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="28bd3-104">In this article, we show how you can use the Azure portal, PowerShell Cmdlets, CLI, or REST API to archive your [Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md) in a storage account.</span></span> <span data-ttu-id="28bd3-105">Tato možnost je užitečná, pokud chcete zachovat k diagnostickým protokolům zásadami volitelné uchování pro audit, statické analýzy nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="28bd3-105">This option is useful if you would like to retain your diagnostic logs with an optional retention policy for audit, static analysis, or backup.</span></span> <span data-ttu-id="28bd3-106">Účet úložiště nemusí být ve stejném předplatném jako prostředek emitování protokoly tak dlouho, dokud uživatel, který konfiguruje nastavení, má odpovídající přístup RBAC do oba odběry.</span><span class="sxs-lookup"><span data-stu-id="28bd3-106">The storage account does not have to be in the same subscription as the resource emitting logs as long as the user who configures the setting has appropriate RBAC access to both subscriptions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28bd3-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="28bd3-107">Prerequisites</span></span>
<span data-ttu-id="28bd3-108">Než začnete, budete muset [vytvořit účet úložiště](../storage/storage-create-storage-account.md) ke kterému archivujete diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="28bd3-108">Before you begin, you need to [create a storage account](../storage/storage-create-storage-account.md) to which you can archive your diagnostic logs.</span></span> <span data-ttu-id="28bd3-109">Důrazně doporučujeme nepoužívejte stávající účet úložiště, který má jiné, -monitoring data v ní uloženy, takže může lépe řídit přístup k datům monitorování.</span><span class="sxs-lookup"><span data-stu-id="28bd3-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access to monitoring data.</span></span> <span data-ttu-id="28bd3-110">Ale pokud jsou také archivaci protokolu aktivit a diagnostiky metriky na účet úložiště, má smysl pro použití tohoto účtu úložiště pro svoje diagnostické protokoly pro monitorování všech dat v centrálním umístění.</span><span class="sxs-lookup"><span data-stu-id="28bd3-110">However, if you are also archiving your Activity Log and diagnostic metrics to a storage account, it may make sense to use that storage account for your diagnostic logs as well to keep all monitoring data in a central location.</span></span> <span data-ttu-id="28bd3-111">Účet úložiště, které používáte, musí být účet úložiště obecné účely, nikoli účet úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="28bd3-111">The storage account you use must be a general purpose storage account, not a blob storage account.</span></span>

## <a name="diagnostic-settings"></a><span data-ttu-id="28bd3-112">Nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="28bd3-112">Diagnostic settings</span></span>
<span data-ttu-id="28bd3-113">K archivaci k diagnostickým protokolům pomocí kteréhokoli z následujících metod, můžete nastavit **nastavení diagnostiky** pro určitý prostředek.</span><span class="sxs-lookup"><span data-stu-id="28bd3-113">To archive your diagnostic logs using any of the methods below, you set a **diagnostic setting** for a particular resource.</span></span> <span data-ttu-id="28bd3-114">Nastavení diagnostiky pro prostředek definuje kategorie protokolů a metriky dat odesílaných do cílového umístění (účet úložiště, obor názvů služby Event Hubs nebo analýzy protokolů).</span><span class="sxs-lookup"><span data-stu-id="28bd3-114">A diagnostic setting for a resource defines the categories of logs and metric data sent to a destination (storage account, Event Hubs namespace, or Log Analytics).</span></span> <span data-ttu-id="28bd3-115">Definuje také zásady uchovávání informací (počet dní, které chcete zachovat) pro události pro každé kategorie protokolu a metriky data uložená v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="28bd3-115">It also defines the retention policy (number of days to retain) for events of each log category and metric data stored in a storage account.</span></span> <span data-ttu-id="28bd3-116">Pokud zásady uchovávání informací je nastaven na hodnotu nula, události pro tuto kategorii protokolu se ukládají po neomezenou dobu (tzn. Tím vyjádříte navždy).</span><span class="sxs-lookup"><span data-stu-id="28bd3-116">If a retention policy is set to zero, events for that log category are stored indefinitely (that is to say, forever).</span></span> <span data-ttu-id="28bd3-117">Zásady uchovávání informací v opačném případě může být libovolný počet dnů od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="28bd3-117">A retention policy can otherwise be any number of days between 1 and 2147483647.</span></span> <span data-ttu-id="28bd3-118">[Další informace o nastavení pro diagnostiku zde](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="28bd3-118">[You can read more about diagnostic settings here](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span> <span data-ttu-id="28bd3-119">Zásady uchovávání informací jsou použité denní, takže na konci za den (UTC), protokoly dnem, který je teď nad rámec uchovávání se zásada odstraní.</span><span class="sxs-lookup"><span data-stu-id="28bd3-119">Retention policies are applied per-day, so at the end of a day (UTC), logs from the day that is now beyond the retention policy will be deleted.</span></span> <span data-ttu-id="28bd3-120">Například pokud jste měli zásady uchovávání informací jeden den, od začátku dnešní den protokoly z včerejšek před den by odstraněn</span><span class="sxs-lookup"><span data-stu-id="28bd3-120">For example, if you had a retention policy of one day, at the beginning of the day today the logs from the day before yesterday would be deleted</span></span>

## <a name="archive-diagnostic-logs-using-the-portal"></a><span data-ttu-id="28bd3-121">Diagnostické protokoly archivu pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="28bd3-121">Archive diagnostic logs using the portal</span></span>
1. <span data-ttu-id="28bd3-122">Na portálu, přejděte do monitorování Azure a klikněte na **nastavení diagnostiky**</span><span class="sxs-lookup"><span data-stu-id="28bd3-122">In the portal, navigate to Azure Monitor and click on **Diagnostic Settings**</span></span>

    ![Monitorování části monitorování Azure](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. <span data-ttu-id="28bd3-124">Volitelně můžete seznam filtrovat podle skupiny prostředků nebo typ prostředku, a potom klikněte na prostředek, pro kterou chcete provést nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="28bd3-124">Optionally filter the list by resource group or resource type, then click on the resource for which you would like to set a diagnostic setting.</span></span>

3. <span data-ttu-id="28bd3-125">Pokud neexistuje žádné nastavení na prostředku jste vybrali, zobrazí se výzva k vytvoření nastavení.</span><span class="sxs-lookup"><span data-stu-id="28bd3-125">If no settings exist on the resource you have selected, you are prompted to create a setting.</span></span> <span data-ttu-id="28bd3-126">Klikněte na tlačítko "Zapnout diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="28bd3-126">Click "Turn on diagnostics."</span></span>

   ![Přidat nastavení diagnostiky - žádná existující nastavení](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   <span data-ttu-id="28bd3-128">Pokud máte stávající nastavení na prostředek, zobrazí se seznam nastavení, které jsou již nakonfigurována na tomto prostředku.</span><span class="sxs-lookup"><span data-stu-id="28bd3-128">If there are existing settings on the resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="28bd3-129">Klikněte na tlačítko "Přidat nastavení diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="28bd3-129">Click "Add diagnostic setting."</span></span>

   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="28bd3-131">Zadejte název nastavení a zaškrtněte políčko pro **exportovat do účtu úložiště**, pak vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="28bd3-131">Give your setting a name and check the box for **Export to Storage Account**, then select a storage account.</span></span> <span data-ttu-id="28bd3-132">Volitelně můžete nastavit počet dní, abyste tyto protokoly uchovávat pomocí **uchovávání dat (dny)** posuvníky.</span><span class="sxs-lookup"><span data-stu-id="28bd3-132">Optionally, set a number of days to retain these logs by using the **Retention (days)** sliders.</span></span> <span data-ttu-id="28bd3-133">Uchování nulový počet dnů po neomezenou dobu ukládá protokoly.</span><span class="sxs-lookup"><span data-stu-id="28bd3-133">A retention of zero days stores the logs indefinitely.</span></span>
   
   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. <span data-ttu-id="28bd3-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="28bd3-135">Click **Save**.</span></span>

<span data-ttu-id="28bd3-136">Po chvíli se nové nastavení se zobrazí v seznamu nastavení pro tento prostředek a diagnostické protokoly jsou archivovány k tomuto účtu úložiště, jakmile se vygeneruje nová data události.</span><span class="sxs-lookup"><span data-stu-id="28bd3-136">After a few moments, the new setting appears in your list of settings for this resource, and diagnostic logs are archived to that storage account as soon as new event data is generated.</span></span>

## <a name="archive-diagnostic-logs-via-azure-powershell"></a><span data-ttu-id="28bd3-137">Archiv diagnostických protokolů pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="28bd3-137">Archive diagnostic logs via Azure PowerShell</span></span>
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| <span data-ttu-id="28bd3-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="28bd3-138">Property</span></span> | <span data-ttu-id="28bd3-139">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="28bd3-139">Required</span></span> | <span data-ttu-id="28bd3-140">Popis</span><span class="sxs-lookup"><span data-stu-id="28bd3-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="28bd3-141">ID prostředku</span><span class="sxs-lookup"><span data-stu-id="28bd3-141">ResourceId</span></span> |<span data-ttu-id="28bd3-142">Ano</span><span class="sxs-lookup"><span data-stu-id="28bd3-142">Yes</span></span> |<span data-ttu-id="28bd3-143">ID prostředku prostředku, na kterém chcete nastavit nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="28bd3-143">Resource ID of the resource on which you want to set a diagnostic setting.</span></span> |
| <span data-ttu-id="28bd3-144">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="28bd3-144">StorageAccountId</span></span> |<span data-ttu-id="28bd3-145">Ne</span><span class="sxs-lookup"><span data-stu-id="28bd3-145">No</span></span> |<span data-ttu-id="28bd3-146">ID prostředku účtu úložiště, který má být uložen diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="28bd3-146">Resource ID of the Storage Account to which Diagnostic Logs should be saved.</span></span> |
| <span data-ttu-id="28bd3-147">Kategorie</span><span class="sxs-lookup"><span data-stu-id="28bd3-147">Categories</span></span> |<span data-ttu-id="28bd3-148">Ne</span><span class="sxs-lookup"><span data-stu-id="28bd3-148">No</span></span> |<span data-ttu-id="28bd3-149">Čárkami oddělený seznam kategorií protokolu povolit.</span><span class="sxs-lookup"><span data-stu-id="28bd3-149">Comma-separated list of log categories to enable.</span></span> |
| <span data-ttu-id="28bd3-150">Povoleno</span><span class="sxs-lookup"><span data-stu-id="28bd3-150">Enabled</span></span> |<span data-ttu-id="28bd3-151">Ano</span><span class="sxs-lookup"><span data-stu-id="28bd3-151">Yes</span></span> |<span data-ttu-id="28bd3-152">Logická hodnota, která určuje, zda diagnostiky jsou zapnutá nebo vypnutá u tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="28bd3-152">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |
| <span data-ttu-id="28bd3-153">RetentionEnabled</span><span class="sxs-lookup"><span data-stu-id="28bd3-153">RetentionEnabled</span></span> |<span data-ttu-id="28bd3-154">Ne</span><span class="sxs-lookup"><span data-stu-id="28bd3-154">No</span></span> |<span data-ttu-id="28bd3-155">Logická hodnota, která udává, pokud jsou zásady uchovávání informací na tento prostředek povolené.</span><span class="sxs-lookup"><span data-stu-id="28bd3-155">Boolean indicating if a retention policy are enabled on this resource.</span></span> |
| <span data-ttu-id="28bd3-156">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="28bd3-156">RetentionInDays</span></span> |<span data-ttu-id="28bd3-157">Ne</span><span class="sxs-lookup"><span data-stu-id="28bd3-157">No</span></span> |<span data-ttu-id="28bd3-158">Počet dní, pro které by měl být zachován události od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="28bd3-158">Number of days for which events should be retained between 1 and 2147483647.</span></span> <span data-ttu-id="28bd3-159">Hodnota nula ukládá protokoly bez omezení.</span><span class="sxs-lookup"><span data-stu-id="28bd3-159">A value of zero stores the logs indefinitely.</span></span> |

## <a name="archive-diagnostic-logs-via-the-cross-platform-cli"></a><span data-ttu-id="28bd3-160">Diagnostické protokoly archivu prostřednictvím rozhraní příkazového řádku pro různé platformy</span><span class="sxs-lookup"><span data-stu-id="28bd3-160">Archive diagnostic logs via the Cross-Platform CLI</span></span>
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| <span data-ttu-id="28bd3-161">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="28bd3-161">Property</span></span> | <span data-ttu-id="28bd3-162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="28bd3-162">Required</span></span> | <span data-ttu-id="28bd3-163">Popis</span><span class="sxs-lookup"><span data-stu-id="28bd3-163">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="28bd3-164">resourceId</span><span class="sxs-lookup"><span data-stu-id="28bd3-164">resourceId</span></span> |<span data-ttu-id="28bd3-165">Ano</span><span class="sxs-lookup"><span data-stu-id="28bd3-165">Yes</span></span> |<span data-ttu-id="28bd3-166">ID prostředku prostředku, na kterém chcete nastavit nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="28bd3-166">Resource ID of the resource on which you want to set a diagnostic setting.</span></span> |
| <span data-ttu-id="28bd3-167">storageId</span><span class="sxs-lookup"><span data-stu-id="28bd3-167">storageId</span></span> |<span data-ttu-id="28bd3-168">Ne</span><span class="sxs-lookup"><span data-stu-id="28bd3-168">No</span></span> |<span data-ttu-id="28bd3-169">ID prostředku účtu úložiště, který má být uložen diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="28bd3-169">Resource ID of the Storage Account to which diagnostic logs should be saved.</span></span> |
| <span data-ttu-id="28bd3-170">Kategorie</span><span class="sxs-lookup"><span data-stu-id="28bd3-170">categories</span></span> |<span data-ttu-id="28bd3-171">Ne</span><span class="sxs-lookup"><span data-stu-id="28bd3-171">No</span></span> |<span data-ttu-id="28bd3-172">Čárkami oddělený seznam kategorií protokolu povolit.</span><span class="sxs-lookup"><span data-stu-id="28bd3-172">Comma-separated list of log categories to enable.</span></span> |
| <span data-ttu-id="28bd3-173">povoleno</span><span class="sxs-lookup"><span data-stu-id="28bd3-173">enabled</span></span> |<span data-ttu-id="28bd3-174">Ano</span><span class="sxs-lookup"><span data-stu-id="28bd3-174">Yes</span></span> |<span data-ttu-id="28bd3-175">Logická hodnota, která určuje, zda diagnostiky jsou zapnutá nebo vypnutá u tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="28bd3-175">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a><span data-ttu-id="28bd3-176">Diagnostické protokoly archivu přes REST API</span><span class="sxs-lookup"><span data-stu-id="28bd3-176">Archive diagnostic logs via the REST API</span></span>
<span data-ttu-id="28bd3-177">[Najdete v tomto dokumentu](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) informace o tom, jak můžete nastavit pomocí rozhraní REST API Azure monitorování diagnostické nastavení.</span><span class="sxs-lookup"><span data-stu-id="28bd3-177">[See this document](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) for information on how you can set up a diagnostic setting using the Azure Monitor REST API.</span></span>

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a><span data-ttu-id="28bd3-178">Schéma diagnostických protokolů v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="28bd3-178">Schema of diagnostic logs in the storage account</span></span>
<span data-ttu-id="28bd3-179">Jakmile jste nastavili archivace, kontejner úložiště se vytvoří v účtu úložiště, jakmile dojde k události v jedné z protokolu kategorií, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="28bd3-179">Once you have set up archival, a storage container is created in the storage account as soon as an event occurs in one of the log categories you have enabled.</span></span> <span data-ttu-id="28bd3-180">Objekty BLOB v kontejneru použijte stejný formát napříč diagnostické protokoly a protokolu aktivity.</span><span class="sxs-lookup"><span data-stu-id="28bd3-180">The blobs within the container follow the same format across Diagnostic Logs and the Activity Log.</span></span> <span data-ttu-id="28bd3-181">Struktura tyto objekty BLOB je:</span><span class="sxs-lookup"><span data-stu-id="28bd3-181">The structure of these blobs is:</span></span>

> <span data-ttu-id="28bd3-182">insights - logs-{název kategorie protokolu} / resourceId = v odběry / {ID předplatného} /RESOURCEGROUPS/ {název skupiny prostředků} /PROVIDERS/ {název zprostředkovatele prostředků} / {resource type} nebo {název prostředku} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin</span><span class="sxs-lookup"><span data-stu-id="28bd3-182">insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="28bd3-183">Nebo více jednoduše</span><span class="sxs-lookup"><span data-stu-id="28bd3-183">Or, more simply,</span></span>

> <span data-ttu-id="28bd3-184">insights - logs-{název kategorie protokolu} / resourceId = / {Id prostředku} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin</span><span class="sxs-lookup"><span data-stu-id="28bd3-184">insights-logs-{log category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="28bd3-185">Například může být název objektu blob:</span><span class="sxs-lookup"><span data-stu-id="28bd3-185">For example, a blob name might be:</span></span>

> <span data-ttu-id="28bd3-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="28bd3-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="28bd3-187">Každý objekt blob PT1H.json obsahuje objekt blob JSON událostí, které nastaly během hodiny zadaného v adrese URL objektu blob (například h = 12).</span><span class="sxs-lookup"><span data-stu-id="28bd3-187">Each PT1H.json blob contains a JSON blob of events that occurred within the hour specified in the blob URL (for example, h=12).</span></span> <span data-ttu-id="28bd3-188">Během přítomen hodinu jsou události připojen k souboru PT1H.json, kdy k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="28bd3-188">During the present hour, events are appended to the PT1H.json file as they occur.</span></span> <span data-ttu-id="28bd3-189">Hodnota minutu (m = 00) je vždy 00, protože protokolů diagnostiky události jsou rozdělená do jednotlivých objektů blob za hodinu.</span><span class="sxs-lookup"><span data-stu-id="28bd3-189">The minute value (m=00) is always 00, since diagnostic log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="28bd3-190">V souboru PT1H.json se ukládají všechny události v poli "záznamy" následující tento formát:</span><span class="sxs-lookup"><span data-stu-id="28bd3-190">Within the PT1H.json file, each event is stored in the “records” array, following this format:</span></span>

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

| <span data-ttu-id="28bd3-191">Název elementu</span><span class="sxs-lookup"><span data-stu-id="28bd3-191">Element name</span></span> | <span data-ttu-id="28bd3-192">Popis</span><span class="sxs-lookup"><span data-stu-id="28bd3-192">Description</span></span> |
| --- | --- |
| <span data-ttu-id="28bd3-193">time</span><span class="sxs-lookup"><span data-stu-id="28bd3-193">time</span></span> |<span data-ttu-id="28bd3-194">Časové razítko při zpracování požadavku odpovídající události služby Azure vygenerovalo událost.</span><span class="sxs-lookup"><span data-stu-id="28bd3-194">Timestamp when the event was generated by the Azure service processing the request corresponding the event.</span></span> |
| <span data-ttu-id="28bd3-195">resourceId</span><span class="sxs-lookup"><span data-stu-id="28bd3-195">resourceId</span></span> |<span data-ttu-id="28bd3-196">ID prostředku ovlivněné prostředku.</span><span class="sxs-lookup"><span data-stu-id="28bd3-196">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="28bd3-197">operationName</span><span class="sxs-lookup"><span data-stu-id="28bd3-197">operationName</span></span> |<span data-ttu-id="28bd3-198">Název operace.</span><span class="sxs-lookup"><span data-stu-id="28bd3-198">Name of the operation.</span></span> |
| <span data-ttu-id="28bd3-199">category</span><span class="sxs-lookup"><span data-stu-id="28bd3-199">category</span></span> |<span data-ttu-id="28bd3-200">Kategorie protokolu události.</span><span class="sxs-lookup"><span data-stu-id="28bd3-200">Log category of the event.</span></span> |
| <span data-ttu-id="28bd3-201">properties</span><span class="sxs-lookup"><span data-stu-id="28bd3-201">properties</span></span> |<span data-ttu-id="28bd3-202">Sada `<Key, Value>` páry (tj. slovník) popisující podrobnosti o události.</span><span class="sxs-lookup"><span data-stu-id="28bd3-202">Set of `<Key, Value>` pairs (i.e. Dictionary) describing the details of the event.</span></span> |

> [!NOTE]
> <span data-ttu-id="28bd3-203">Vlastnosti a využití tyto vlastnosti se může lišit v závislosti na prostředek.</span><span class="sxs-lookup"><span data-stu-id="28bd3-203">The properties and usage of those properties can vary depending on the resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="28bd3-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28bd3-204">Next steps</span></span>
* [<span data-ttu-id="28bd3-205">Stáhnout objekty BLOB pro analýzu</span><span class="sxs-lookup"><span data-stu-id="28bd3-205">Download blobs for analysis</span></span>](../storage/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="28bd3-206">Diagnostické protokoly datový proud na obor názvů služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="28bd3-206">Stream diagnostic logs to an Event Hubs namespace</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [<span data-ttu-id="28bd3-207">Další informace o diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="28bd3-207">Read more about diagnostic logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
