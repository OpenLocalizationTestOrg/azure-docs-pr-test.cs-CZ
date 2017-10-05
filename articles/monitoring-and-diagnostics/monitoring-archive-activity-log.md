---
title: "Archivovat protokol činnosti Azure | Microsoft Docs"
description: "Zjistěte, jak k archivaci váš protokol činnosti Azure pro dlouhodobé uchovávání v účtu úložiště."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
ms.openlocfilehash: 0e3a5b84f57eac96249430fa1c2c4cc076c2926a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="archive-the-azure-activity-log"></a><span data-ttu-id="f1ae3-103">Archiv protokol činnosti Azure</span><span class="sxs-lookup"><span data-stu-id="f1ae3-103">Archive the Azure Activity Log</span></span>
<span data-ttu-id="f1ae3-104">V tomto článku jsme ukazují, jak pomocí portálu Azure, rutiny prostředí PowerShell nebo rozhraní příkazového řádku a platformy pro archivaci vaše [ **protokol činnosti Azure** ](monitoring-overview-activity-logs.md) v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-104">In this article, we show how you can use the Azure portal, PowerShell Cmdlets, or Cross-Platform CLI to archive your [**Azure Activity Log**](monitoring-overview-activity-logs.md) in a storage account.</span></span> <span data-ttu-id="f1ae3-105">Tato možnost je užitečná, pokud chcete uchovat déle, než 90 dní (s plnou kontrolu nad zásady uchovávání informací) pro audit, statické analýzy nebo zálohování aktivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-105">This option is useful if you would like to retain your Activity Log longer than 90 days (with full control over the retention policy) for audit, static analysis, or backup.</span></span> <span data-ttu-id="f1ae3-106">Pokud potřebujete události uchovávány 90 dnů nebo méně není nutné nastavit archivace na účet úložiště, protože aktivity protokolu události se zachovají v platformy Azure za 90 dnů bez povolení archivace.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-106">If you only need to retain your events for 90 days or less you do not need to set up archival to a storage account, since Activity Log events are retained in the Azure platform for 90 days without enabling archival.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1ae3-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f1ae3-107">Prerequisites</span></span>
<span data-ttu-id="f1ae3-108">Než začnete, budete muset [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) ke kterému může archivaci protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-108">Before you begin, you need to [create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) to which you can archive your Activity Log.</span></span> <span data-ttu-id="f1ae3-109">Důrazně doporučujeme nepoužívejte stávající účet úložiště, který má jiné, -monitoring data v ní uloženy, takže může lépe řídit přístup k datům monitorování.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access to monitoring data.</span></span> <span data-ttu-id="f1ae3-110">Ale pokud jsou také archivace diagnostické protokoly a metriky na účet úložiště, má smysl pro použití tohoto účtu úložiště pro protokol aktivit také pro monitorování všech dat v centrálním umístění.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-110">However, if you are also archiving Diagnostic Logs and metrics to a storage account, it may make sense to use that storage account for your Activity Log as well to keep all monitoring data in a central location.</span></span> <span data-ttu-id="f1ae3-111">Účet úložiště, které používáte, musí být účet úložiště obecné účely, nikoli účet úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-111">The storage account you use must be a general purpose storage account, not a blob storage account.</span></span> <span data-ttu-id="f1ae3-112">Účet úložiště nemusí být ve stejném předplatném jako předplatné emitování protokoly tak dlouho, dokud uživatel, který konfiguruje nastavení, má odpovídající přístup RBAC do oba odběry.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-112">The storage account does not have to be in the same subscription as the subscription emitting logs as long as the user who configures the setting has appropriate RBAC access to both subscriptions.</span></span>

## <a name="log-profile"></a><span data-ttu-id="f1ae3-113">Profil protokolu</span><span class="sxs-lookup"><span data-stu-id="f1ae3-113">Log Profile</span></span>
<span data-ttu-id="f1ae3-114">K archivaci protokolu aktivit pomocí kteréhokoli z následujících metod, můžete nastavit **profil protokolu** pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-114">To archive the Activity Log using any of the methods below, you set the **Log Profile** for a subscription.</span></span> <span data-ttu-id="f1ae3-115">Profil protokolu definuje typ události, které jsou uložené nebo prostřednictvím datového proudu a výstupy – úložiště účet nebo události rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-115">The Log Profile defines the type of events that are stored or streamed and the outputs—storage account and/or event hub.</span></span> <span data-ttu-id="f1ae3-116">Definuje také zásady uchovávání informací (počet dní, které chcete zachovat) pro události, které jsou uložené v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-116">It also defines the retention policy (number of days to retain) for events stored in a storage account.</span></span> <span data-ttu-id="f1ae3-117">Pokud zásady uchovávání informací je nastaven na hodnotu nula, události jsou uloženy po neomezenou dobu.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-117">If the retention policy is set to zero, events are stored indefinitely.</span></span> <span data-ttu-id="f1ae3-118">Jinak to je možné nastavit na jakoukoli hodnotu od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-118">Otherwise, this can be set to any value between 1 and 2147483647.</span></span> <span data-ttu-id="f1ae3-119">Zásady uchovávání informací jsou použité denní, takže na konci za den (UTC), protokoly dnem, který je teď nad rámec uchovávání se zásada odstraní.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-119">Retention policies are applied per-day, so at the end of a day (UTC), logs from the day that is now beyond the retention policy will be deleted.</span></span> <span data-ttu-id="f1ae3-120">Například pokud jste měli zásady uchovávání informací jeden den, od začátku dnešní den protokoly z včerejšek před den by odstraněn.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-120">For example, if you had a retention policy of one day, at the beginning of the day today the logs from the day before yesterday would be deleted.</span></span> <span data-ttu-id="f1ae3-121">[Další informace o protokolu profily zde](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span><span class="sxs-lookup"><span data-stu-id="f1ae3-121">[You can read more about log profiles here](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span></span> 

## <a name="archive-the-activity-log-using-the-portal"></a><span data-ttu-id="f1ae3-122">Archivovat protokol aktivit pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="f1ae3-122">Archive the Activity Log using the portal</span></span>
1. <span data-ttu-id="f1ae3-123">Na portálu, klikněte **protokol aktivit** na levé straně navigační odkaz.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-123">In the portal, click the **Activity Log** link on the left-side navigation.</span></span> <span data-ttu-id="f1ae3-124">Pokud nevidíte odkaz pro protokol aktivit, klikněte na tlačítko **více služeb** nejprve odkaz.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-124">If you don’t see a link for the Activity Log, click the **More Services** link first.</span></span>
   
    ![Přejděte do okna Protokol aktivit](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. <span data-ttu-id="f1ae3-126">V horní části okna klikněte na tlačítko **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-126">At the top of the blade, click **Export**.</span></span>
   
    ![Klikněte na tlačítko Export](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. <span data-ttu-id="f1ae3-128">V okně, který se zobrazí, zaškrtněte políčko pro **exportovat do účtu úložiště** a vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-128">In the blade that appears, check the box for **Export to a storage account** and select a storage account.</span></span>
   
    ![Nastavit účet úložiště](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. <span data-ttu-id="f1ae3-130">Pomocí posuvníku nebo textové pole, zadejte počet dní, pro které by měly být udržovány aktivity protokolu události v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-130">Using the slider or text box, define a number of days for which Activity Log events should be kept in your storage account.</span></span> <span data-ttu-id="f1ae3-131">Pokud chcete, aby vaše data uchovávané v účtu úložiště bez omezení, nastavte toto číslo na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-131">If you prefer to have your data persisted in the storage account indefinitely, set this number to zero.</span></span>
5. <span data-ttu-id="f1ae3-132">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-132">Click **Save**.</span></span>

## <a name="archive-the-activity-log-via-powershell"></a><span data-ttu-id="f1ae3-133">Archiv protokol aktivit pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1ae3-133">Archive the Activity Log via PowerShell</span></span>
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| <span data-ttu-id="f1ae3-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f1ae3-134">Property</span></span> | <span data-ttu-id="f1ae3-135">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f1ae3-135">Required</span></span> | <span data-ttu-id="f1ae3-136">Popis</span><span class="sxs-lookup"><span data-stu-id="f1ae3-136">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f1ae3-137">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="f1ae3-137">StorageAccountId</span></span> |<span data-ttu-id="f1ae3-138">Ne</span><span class="sxs-lookup"><span data-stu-id="f1ae3-138">No</span></span> |<span data-ttu-id="f1ae3-139">ID prostředku účtu úložiště, který má být uložen protokoly aktivity.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-139">Resource ID of the Storage Account to which Activity Logs should be saved.</span></span> |
| <span data-ttu-id="f1ae3-140">Umístění</span><span class="sxs-lookup"><span data-stu-id="f1ae3-140">Locations</span></span> |<span data-ttu-id="f1ae3-141">Ano</span><span class="sxs-lookup"><span data-stu-id="f1ae3-141">Yes</span></span> |<span data-ttu-id="f1ae3-142">Seznam oddělený čárkami oblastí, pro které chcete shromažďovat aktivity protokolu události.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-142">Comma-separated list of regions for which you would like to collect Activity Log events.</span></span> <span data-ttu-id="f1ae3-143">Můžete zobrazit seznam všech oblastech [této stránce](https://azure.microsoft.com/en-us/regions) nebo pomocí [REST API pro správu Azure](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1ae3-143">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [the Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="f1ae3-144">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="f1ae3-144">RetentionInDays</span></span> |<span data-ttu-id="f1ae3-145">Ano</span><span class="sxs-lookup"><span data-stu-id="f1ae3-145">Yes</span></span> |<span data-ttu-id="f1ae3-146">Počet dní pro události, které by měl být zachován, od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-146">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="f1ae3-147">Hodnota nula ukládá protokoly bez omezení (navždy).</span><span class="sxs-lookup"><span data-stu-id="f1ae3-147">A value of zero stores the logs indefinitely (forever).</span></span> |
| <span data-ttu-id="f1ae3-148">Kategorie</span><span class="sxs-lookup"><span data-stu-id="f1ae3-148">Categories</span></span> |<span data-ttu-id="f1ae3-149">Ano</span><span class="sxs-lookup"><span data-stu-id="f1ae3-149">Yes</span></span> |<span data-ttu-id="f1ae3-150">Seznam oddělený čárkami kategorií událostí, které by měl být shromážděny.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-150">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="f1ae3-151">Možné hodnoty jsou zápisu, odstranění a akce.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-151">Possible values are Write, Delete, and Action.</span></span> |

## <a name="archive-the-activity-log-via-cli"></a><span data-ttu-id="f1ae3-152">Archiv protokol aktivit prostřednictvím rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f1ae3-152">Archive the Activity Log via CLI</span></span>
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| <span data-ttu-id="f1ae3-153">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f1ae3-153">Property</span></span> | <span data-ttu-id="f1ae3-154">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f1ae3-154">Required</span></span> | <span data-ttu-id="f1ae3-155">Popis</span><span class="sxs-lookup"><span data-stu-id="f1ae3-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f1ae3-156">jméno</span><span class="sxs-lookup"><span data-stu-id="f1ae3-156">name</span></span> |<span data-ttu-id="f1ae3-157">Ano</span><span class="sxs-lookup"><span data-stu-id="f1ae3-157">Yes</span></span> |<span data-ttu-id="f1ae3-158">Název vašeho profilu protokolu.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-158">Name of your log profile.</span></span> |
| <span data-ttu-id="f1ae3-159">storageId</span><span class="sxs-lookup"><span data-stu-id="f1ae3-159">storageId</span></span> |<span data-ttu-id="f1ae3-160">Ne</span><span class="sxs-lookup"><span data-stu-id="f1ae3-160">No</span></span> |<span data-ttu-id="f1ae3-161">ID prostředku účtu úložiště, který má být uložen protokoly aktivity.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-161">Resource ID of the Storage Account to which Activity Logs should be saved.</span></span> |
| <span data-ttu-id="f1ae3-162">Umístění</span><span class="sxs-lookup"><span data-stu-id="f1ae3-162">locations</span></span> |<span data-ttu-id="f1ae3-163">Ano</span><span class="sxs-lookup"><span data-stu-id="f1ae3-163">Yes</span></span> |<span data-ttu-id="f1ae3-164">Seznam oddělený čárkami oblastí, pro které chcete shromažďovat aktivity protokolu události.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-164">Comma-separated list of regions for which you would like to collect Activity Log events.</span></span> <span data-ttu-id="f1ae3-165">Můžete zobrazit seznam všech oblastech [této stránce](https://azure.microsoft.com/en-us/regions) nebo pomocí [REST API pro správu Azure](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1ae3-165">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [the Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="f1ae3-166">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="f1ae3-166">retentionInDays</span></span> |<span data-ttu-id="f1ae3-167">Ano</span><span class="sxs-lookup"><span data-stu-id="f1ae3-167">Yes</span></span> |<span data-ttu-id="f1ae3-168">Počet dní pro události, které by měl být zachován, od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-168">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="f1ae3-169">Hodnota nula bude po neomezenou dobu ukládání protokolů (navždy).</span><span class="sxs-lookup"><span data-stu-id="f1ae3-169">A value of zero will store the logs indefinitely (forever).</span></span> |
| <span data-ttu-id="f1ae3-170">Kategorie</span><span class="sxs-lookup"><span data-stu-id="f1ae3-170">categories</span></span> |<span data-ttu-id="f1ae3-171">Ano</span><span class="sxs-lookup"><span data-stu-id="f1ae3-171">Yes</span></span> |<span data-ttu-id="f1ae3-172">Seznam oddělený čárkami kategorií událostí, které by měl být shromážděny.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-172">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="f1ae3-173">Možné hodnoty jsou zápisu, odstranění a akce.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-173">Possible values are Write, Delete, and Action.</span></span> |

## <a name="storage-schema-of-the-activity-log"></a><span data-ttu-id="f1ae3-174">Schéma úložiště protokolu činnosti</span><span class="sxs-lookup"><span data-stu-id="f1ae3-174">Storage schema of the Activity Log</span></span>
<span data-ttu-id="f1ae3-175">Jakmile jste nastavili archivace, kontejner úložiště bude vytvořen v účtu úložiště Jakmile dojde k aktivity protokolu události.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-175">Once you have set up archival, a storage container will be created in the storage account as soon as an Activity Log event occurs.</span></span> <span data-ttu-id="f1ae3-176">Objekty BLOB v kontejneru použijte stejný formát přes protokol aktivit a diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-176">The blobs within the container follow the same format across the Activity Log and Diagnostic Logs.</span></span> <span data-ttu-id="f1ae3-177">Struktura tyto objekty BLOB je:</span><span class="sxs-lookup"><span data-stu-id="f1ae3-177">The structure of these blobs is:</span></span>

> <span data-ttu-id="f1ae3-178">Statistika provozní protokoly/name = výchozí/resourceId = v odběry / {ID předplatného} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin</span><span class="sxs-lookup"><span data-stu-id="f1ae3-178">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="f1ae3-179">Například může být název objektu blob:</span><span class="sxs-lookup"><span data-stu-id="f1ae3-179">For example, a blob name might be:</span></span>

> <span data-ttu-id="f1ae3-180">insights-Operational-Logs/Name=default/resourceId=/Subscriptions/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="f1ae3-180">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="f1ae3-181">Každý objekt blob PT1H.json obsahuje objekt blob JSON událostí, které nastaly během hodiny zadaného v adrese URL objektu blob (například h = 12).</span><span class="sxs-lookup"><span data-stu-id="f1ae3-181">Each PT1H.json blob contains a JSON blob of events that occurred within the hour specified in the blob URL (e.g. h=12).</span></span> <span data-ttu-id="f1ae3-182">Během přítomen hodinu jsou události připojen k souboru PT1H.json, kdy k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-182">During the present hour, events are appended to the PT1H.json file as they occur.</span></span> <span data-ttu-id="f1ae3-183">Hodnota minutu (m = 00) je vždy 00, protože aktivity protokolu události jsou rozdělená do jednotlivých objektů blob za hodinu.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-183">The minute value (m=00) is always 00, since Activity Log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="f1ae3-184">V souboru PT1H.json se ukládají všechny události v poli "záznamy" následující tento formát:</span><span class="sxs-lookup"><span data-stu-id="f1ae3-184">Within the PT1H.json file, each event is stored in the “records” array, following this format:</span></span>

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| <span data-ttu-id="f1ae3-185">Název elementu</span><span class="sxs-lookup"><span data-stu-id="f1ae3-185">Element name</span></span> | <span data-ttu-id="f1ae3-186">Popis</span><span class="sxs-lookup"><span data-stu-id="f1ae3-186">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f1ae3-187">time</span><span class="sxs-lookup"><span data-stu-id="f1ae3-187">time</span></span> |<span data-ttu-id="f1ae3-188">Časové razítko při zpracování požadavku odpovídající události služby Azure vygenerovalo událost.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-188">Timestamp when the event was generated by the Azure service processing the request corresponding the event.</span></span> |
| <span data-ttu-id="f1ae3-189">resourceId</span><span class="sxs-lookup"><span data-stu-id="f1ae3-189">resourceId</span></span> |<span data-ttu-id="f1ae3-190">ID prostředku ovlivněné prostředku.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-190">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="f1ae3-191">operationName</span><span class="sxs-lookup"><span data-stu-id="f1ae3-191">operationName</span></span> |<span data-ttu-id="f1ae3-192">Název operace.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-192">Name of the operation.</span></span> |
| <span data-ttu-id="f1ae3-193">category</span><span class="sxs-lookup"><span data-stu-id="f1ae3-193">category</span></span> |<span data-ttu-id="f1ae3-194">Kategorie Akce, např.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-194">Category of the action, eg.</span></span> <span data-ttu-id="f1ae3-195">Zápis, čtení, akce.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-195">Write, Read, Action.</span></span> |
| <span data-ttu-id="f1ae3-196">resultType</span><span class="sxs-lookup"><span data-stu-id="f1ae3-196">resultType</span></span> |<span data-ttu-id="f1ae3-197">Typ výsledku, např.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-197">The type of the result, eg.</span></span> <span data-ttu-id="f1ae3-198">Úspěch, chyba, spuštění</span><span class="sxs-lookup"><span data-stu-id="f1ae3-198">Success, Failure, Start</span></span> |
| <span data-ttu-id="f1ae3-199">resultSignature</span><span class="sxs-lookup"><span data-stu-id="f1ae3-199">resultSignature</span></span> |<span data-ttu-id="f1ae3-200">Závisí na typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-200">Depends on the resource type.</span></span> |
| <span data-ttu-id="f1ae3-201">durationMs</span><span class="sxs-lookup"><span data-stu-id="f1ae3-201">durationMs</span></span> |<span data-ttu-id="f1ae3-202">Doba trvání operace v milisekundách</span><span class="sxs-lookup"><span data-stu-id="f1ae3-202">Duration of the operation in milliseconds</span></span> |
| <span data-ttu-id="f1ae3-203">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="f1ae3-203">callerIpAddress</span></span> |<span data-ttu-id="f1ae3-204">IP adresa uživatele, který se má provést operace, deklarace hlavní název uživatele nebo název SPN deklarace identity na základě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-204">IP address of the user who has performed the operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="f1ae3-205">correlationId</span><span class="sxs-lookup"><span data-stu-id="f1ae3-205">correlationId</span></span> |<span data-ttu-id="f1ae3-206">Obvykle GUID ve formátu řetězce.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-206">Usually a GUID in the string format.</span></span> <span data-ttu-id="f1ae3-207">Události, které sdílejí correlationId patřit do stejné uber akce.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-207">Events that share a correlationId belong to the same uber action.</span></span> |
| <span data-ttu-id="f1ae3-208">identity</span><span class="sxs-lookup"><span data-stu-id="f1ae3-208">identity</span></span> |<span data-ttu-id="f1ae3-209">Objekt blob JSON s popisem autorizace a deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-209">JSON blob describing the authorization and claims.</span></span> |
| <span data-ttu-id="f1ae3-210">Autorizace</span><span class="sxs-lookup"><span data-stu-id="f1ae3-210">authorization</span></span> |<span data-ttu-id="f1ae3-211">Objekt BLOB RBAC vlastností události.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-211">Blob of RBAC properties of the event.</span></span> <span data-ttu-id="f1ae3-212">Obvykle zahrnuje vlastnosti "action", "role" a "obor".</span><span class="sxs-lookup"><span data-stu-id="f1ae3-212">Usually includes the “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="f1ae3-213">úroveň</span><span class="sxs-lookup"><span data-stu-id="f1ae3-213">level</span></span> |<span data-ttu-id="f1ae3-214">Úroveň události.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-214">Level of the event.</span></span> <span data-ttu-id="f1ae3-215">Jeden z následujících hodnot: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné"</span><span class="sxs-lookup"><span data-stu-id="f1ae3-215">One of the following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="f1ae3-216">location</span><span class="sxs-lookup"><span data-stu-id="f1ae3-216">location</span></span> |<span data-ttu-id="f1ae3-217">Oblast, ve které došlo k umístění (nebo globální).</span><span class="sxs-lookup"><span data-stu-id="f1ae3-217">Region in which the location occurred (or global).</span></span> |
| <span data-ttu-id="f1ae3-218">properties</span><span class="sxs-lookup"><span data-stu-id="f1ae3-218">properties</span></span> |<span data-ttu-id="f1ae3-219">Sada `<Key, Value>` páry (tj. slovník) popisující podrobnosti o události.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-219">Set of `<Key, Value>` pairs (i.e. Dictionary) describing the details of the event.</span></span> |

> [!NOTE]
> <span data-ttu-id="f1ae3-220">Vlastnosti a využití tyto vlastnosti se může lišit v závislosti na prostředek.</span><span class="sxs-lookup"><span data-stu-id="f1ae3-220">The properties and usage of those properties can vary depending on the resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="f1ae3-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1ae3-221">Next steps</span></span>
* [<span data-ttu-id="f1ae3-222">Stáhnout objekty BLOB pro analýzu</span><span class="sxs-lookup"><span data-stu-id="f1ae3-222">Download blobs for analysis</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [<span data-ttu-id="f1ae3-223">Datový proud protokolu aktivit do centra událostí</span><span class="sxs-lookup"><span data-stu-id="f1ae3-223">Stream the Activity Log to Event Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
* [<span data-ttu-id="f1ae3-224">Další informace o protokolu aktivit</span><span class="sxs-lookup"><span data-stu-id="f1ae3-224">Read more about the Activity Log</span></span>](monitoring-overview-activity-logs.md)

