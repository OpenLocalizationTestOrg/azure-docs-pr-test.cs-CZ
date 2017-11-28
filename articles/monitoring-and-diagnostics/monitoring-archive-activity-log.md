---
title: "aaaArchive hello protokol činnosti Azure | Microsoft Docs"
description: "Zjistěte, jak tooarchive si Azure aktivitu protokolu pro dlouhodobé uchovávání v účtu úložiště."
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
ms.openlocfilehash: 58c6d3a3a31398287f66f76999d48f2942ab5109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="archive-hello-azure-activity-log"></a><span data-ttu-id="466d6-103">Archiv hello protokol činnosti Azure</span><span class="sxs-lookup"><span data-stu-id="466d6-103">Archive hello Azure Activity Log</span></span>
<span data-ttu-id="466d6-104">V tomto článku jsme ukazují, jak můžete použít hello portálu Azure, rutiny prostředí PowerShell nebo rozhraní příkazového řádku a platformy tooarchive vaše [ **protokol činnosti Azure** ](monitoring-overview-activity-logs.md) v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="466d6-104">In this article, we show how you can use hello Azure portal, PowerShell Cmdlets, or Cross-Platform CLI tooarchive your [**Azure Activity Log**](monitoring-overview-activity-logs.md) in a storage account.</span></span> <span data-ttu-id="466d6-105">Tato možnost je užitečná, pokud byste chtěli tooretain delší než 90 dní (s plnou kontrolu nad zásady uchovávání informací hello) pro aktivitu protokolu auditování, statické analýzy, nebo zálohování.</span><span class="sxs-lookup"><span data-stu-id="466d6-105">This option is useful if you would like tooretain your Activity Log longer than 90 days (with full control over hello retention policy) for audit, static analysis, or backup.</span></span> <span data-ttu-id="466d6-106">Pokud potřebujete tooretain události za 90 dní nebo méně. můžete nemusí tooset archivace tooa účet úložiště, protože aktivity protokolu události se zachovají v hello platformy Azure za 90 dnů bez povolení archivace.</span><span class="sxs-lookup"><span data-stu-id="466d6-106">If you only need tooretain your events for 90 days or less you do not need tooset up archival tooa storage account, since Activity Log events are retained in hello Azure platform for 90 days without enabling archival.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="466d6-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="466d6-107">Prerequisites</span></span>
<span data-ttu-id="466d6-108">Než začnete, je třeba příliš[vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) toowhich archivujete protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="466d6-108">Before you begin, you need too[create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) toowhich you can archive your Activity Log.</span></span> <span data-ttu-id="466d6-109">Důrazně doporučujeme stávající účet úložiště, který má jiné, -monitoring data v ní uloženy, takže může lépe řídit přístup k datům toomonitoring nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="466d6-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access toomonitoring data.</span></span> <span data-ttu-id="466d6-110">Ale pokud jsou také archivace diagnostické protokoly a účet úložiště tooa metriky, může být smysl toouse daný účet úložiště pro vaše aktivity protokolu také tookeep všechna monitorování data v centrálním umístění.</span><span class="sxs-lookup"><span data-stu-id="466d6-110">However, if you are also archiving Diagnostic Logs and metrics tooa storage account, it may make sense toouse that storage account for your Activity Log as well tookeep all monitoring data in a central location.</span></span> <span data-ttu-id="466d6-111">Hello účet úložiště, které používáte, musí být účet úložiště obecné účely, nikoli účet úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="466d6-111">hello storage account you use must be a general purpose storage account, not a blob storage account.</span></span> <span data-ttu-id="466d6-112">účet úložiště Hello nemá toobe v hello stejnému předplatnému jako hello předplatné emitování protokoly tak dlouho, dokud hello uživatel, který konfiguruje nastavení hello má příslušné předplatné tooboth přístupu RBAC.</span><span class="sxs-lookup"><span data-stu-id="466d6-112">hello storage account does not have toobe in hello same subscription as hello subscription emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="log-profile"></a><span data-ttu-id="466d6-113">Profil protokolu</span><span class="sxs-lookup"><span data-stu-id="466d6-113">Log Profile</span></span>
<span data-ttu-id="466d6-114">tooarchive hello protokol aktivit pomocí metod hello níže, nastavíte hello **profil protokolu** pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="466d6-114">tooarchive hello Activity Log using any of hello methods below, you set hello **Log Profile** for a subscription.</span></span> <span data-ttu-id="466d6-115">Hello profil protokolu definuje hello typ události, které jsou uložené nebo prostřednictvím datového proudu a hello výstupy – úložiště účet nebo události rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="466d6-115">hello Log Profile defines hello type of events that are stored or streamed and hello outputs—storage account and/or event hub.</span></span> <span data-ttu-id="466d6-116">Definuje také zásady uchovávání informací hello (počet dní tooretain) pro události, které jsou uložené v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="466d6-116">It also defines hello retention policy (number of days tooretain) for events stored in a storage account.</span></span> <span data-ttu-id="466d6-117">Pokud zásady uchovávání informací hello nastavena toozero, události jsou uloženy po neomezenou dobu.</span><span class="sxs-lookup"><span data-stu-id="466d6-117">If hello retention policy is set toozero, events are stored indefinitely.</span></span> <span data-ttu-id="466d6-118">Jinak to je možné nastavit tooany hodnotu od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="466d6-118">Otherwise, this can be set tooany value between 1 and 2147483647.</span></span> <span data-ttu-id="466d6-119">Zásady uchovávání informací jsou použité za den, tak při hello Konec dne (UTC), zaprotokoluje hello dni, který je teď nad rámec zásad uchovávání informací hello budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="466d6-119">Retention policies are applied per-day, so at hello end of a day (UTC), logs from hello day that is now beyond hello retention policy will be deleted.</span></span> <span data-ttu-id="466d6-120">Například pokud jste měli zásady uchovávání informací jeden den, od začátku hello dnešní den hello hello protokoly z hello den před včerejšek by odstraněn.</span><span class="sxs-lookup"><span data-stu-id="466d6-120">For example, if you had a retention policy of one day, at hello beginning of hello day today hello logs from hello day before yesterday would be deleted.</span></span> <span data-ttu-id="466d6-121">[Další informace o protokolu profily zde](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span><span class="sxs-lookup"><span data-stu-id="466d6-121">[You can read more about log profiles here](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span></span> 

## <a name="archive-hello-activity-log-using-hello-portal"></a><span data-ttu-id="466d6-122">Archiv hello protokol aktivit portálu hello</span><span class="sxs-lookup"><span data-stu-id="466d6-122">Archive hello Activity Log using hello portal</span></span>
1. <span data-ttu-id="466d6-123">Hello portálu, klikněte na tlačítko hello **protokol aktivit** na hello levé navigační odkaz.</span><span class="sxs-lookup"><span data-stu-id="466d6-123">In hello portal, click hello **Activity Log** link on hello left-side navigation.</span></span> <span data-ttu-id="466d6-124">Pokud nevidíte odkaz pro hello protokol aktivit, klikněte na tlačítko hello **více služeb** nejprve odkaz.</span><span class="sxs-lookup"><span data-stu-id="466d6-124">If you don’t see a link for hello Activity Log, click hello **More Services** link first.</span></span>
   
    ![Přejděte tooActivity protokolu okno](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. <span data-ttu-id="466d6-126">V horní části hello hello okna, klikněte na tlačítko **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="466d6-126">At hello top of hello blade, click **Export**.</span></span>
   
    ![Klikněte na tlačítko pro Export hello](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. <span data-ttu-id="466d6-128">V okně hello, který se zobrazí, zaškrtněte políčko hello pro **exportovat účet úložiště tooa** a vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="466d6-128">In hello blade that appears, check hello box for **Export tooa storage account** and select a storage account.</span></span>
   
    ![Nastavit účet úložiště](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. <span data-ttu-id="466d6-130">Pomocí posuvníku hello nebo textové pole, zadejte počet dní, pro které by měly být udržovány aktivity protokolu události v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="466d6-130">Using hello slider or text box, define a number of days for which Activity Log events should be kept in your storage account.</span></span> <span data-ttu-id="466d6-131">Pokud dáváte přednost toohave data uchovávané v účtu úložiště hello bez omezení, nastavte toto číslo toozero.</span><span class="sxs-lookup"><span data-stu-id="466d6-131">If you prefer toohave your data persisted in hello storage account indefinitely, set this number toozero.</span></span>
5. <span data-ttu-id="466d6-132">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="466d6-132">Click **Save**.</span></span>

## <a name="archive-hello-activity-log-via-powershell"></a><span data-ttu-id="466d6-133">Archiv hello protokol aktivit pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="466d6-133">Archive hello Activity Log via PowerShell</span></span>
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| <span data-ttu-id="466d6-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="466d6-134">Property</span></span> | <span data-ttu-id="466d6-135">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="466d6-135">Required</span></span> | <span data-ttu-id="466d6-136">Popis</span><span class="sxs-lookup"><span data-stu-id="466d6-136">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="466d6-137">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="466d6-137">StorageAccountId</span></span> |<span data-ttu-id="466d6-138">Ne</span><span class="sxs-lookup"><span data-stu-id="466d6-138">No</span></span> |<span data-ttu-id="466d6-139">ID prostředku hello účet úložiště toowhich protokoly aktivity má být uložen.</span><span class="sxs-lookup"><span data-stu-id="466d6-139">Resource ID of hello Storage Account toowhich Activity Logs should be saved.</span></span> |
| <span data-ttu-id="466d6-140">Umístění</span><span class="sxs-lookup"><span data-stu-id="466d6-140">Locations</span></span> |<span data-ttu-id="466d6-141">Ano</span><span class="sxs-lookup"><span data-stu-id="466d6-141">Yes</span></span> |<span data-ttu-id="466d6-142">Seznam oddělený čárkami oblastí, pro které byste chtěli toocollect aktivity protokolu události.</span><span class="sxs-lookup"><span data-stu-id="466d6-142">Comma-separated list of regions for which you would like toocollect Activity Log events.</span></span> <span data-ttu-id="466d6-143">Můžete zobrazit seznam všech oblastech [této stránce](https://azure.microsoft.com/en-us/regions) nebo pomocí [hello REST API pro správu Azure](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="466d6-143">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="466d6-144">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="466d6-144">RetentionInDays</span></span> |<span data-ttu-id="466d6-145">Ano</span><span class="sxs-lookup"><span data-stu-id="466d6-145">Yes</span></span> |<span data-ttu-id="466d6-146">Počet dní pro události, které by měl být zachován, od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="466d6-146">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="466d6-147">Hodnota nula ukládá protokoly hello po neomezenou dobu (navždy).</span><span class="sxs-lookup"><span data-stu-id="466d6-147">A value of zero stores hello logs indefinitely (forever).</span></span> |
| <span data-ttu-id="466d6-148">Kategorie</span><span class="sxs-lookup"><span data-stu-id="466d6-148">Categories</span></span> |<span data-ttu-id="466d6-149">Ano</span><span class="sxs-lookup"><span data-stu-id="466d6-149">Yes</span></span> |<span data-ttu-id="466d6-150">Seznam oddělený čárkami kategorií událostí, které by měl být shromážděny.</span><span class="sxs-lookup"><span data-stu-id="466d6-150">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="466d6-151">Možné hodnoty jsou zápisu, odstranění a akce.</span><span class="sxs-lookup"><span data-stu-id="466d6-151">Possible values are Write, Delete, and Action.</span></span> |

## <a name="archive-hello-activity-log-via-cli"></a><span data-ttu-id="466d6-152">Archiv hello protokol aktivit prostřednictvím rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="466d6-152">Archive hello Activity Log via CLI</span></span>
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| <span data-ttu-id="466d6-153">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="466d6-153">Property</span></span> | <span data-ttu-id="466d6-154">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="466d6-154">Required</span></span> | <span data-ttu-id="466d6-155">Popis</span><span class="sxs-lookup"><span data-stu-id="466d6-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="466d6-156">jméno</span><span class="sxs-lookup"><span data-stu-id="466d6-156">name</span></span> |<span data-ttu-id="466d6-157">Ano</span><span class="sxs-lookup"><span data-stu-id="466d6-157">Yes</span></span> |<span data-ttu-id="466d6-158">Název vašeho profilu protokolu.</span><span class="sxs-lookup"><span data-stu-id="466d6-158">Name of your log profile.</span></span> |
| <span data-ttu-id="466d6-159">storageId</span><span class="sxs-lookup"><span data-stu-id="466d6-159">storageId</span></span> |<span data-ttu-id="466d6-160">Ne</span><span class="sxs-lookup"><span data-stu-id="466d6-160">No</span></span> |<span data-ttu-id="466d6-161">ID prostředku hello účet úložiště toowhich protokoly aktivity má být uložen.</span><span class="sxs-lookup"><span data-stu-id="466d6-161">Resource ID of hello Storage Account toowhich Activity Logs should be saved.</span></span> |
| <span data-ttu-id="466d6-162">Umístění</span><span class="sxs-lookup"><span data-stu-id="466d6-162">locations</span></span> |<span data-ttu-id="466d6-163">Ano</span><span class="sxs-lookup"><span data-stu-id="466d6-163">Yes</span></span> |<span data-ttu-id="466d6-164">Seznam oddělený čárkami oblastí, pro které byste chtěli toocollect aktivity protokolu události.</span><span class="sxs-lookup"><span data-stu-id="466d6-164">Comma-separated list of regions for which you would like toocollect Activity Log events.</span></span> <span data-ttu-id="466d6-165">Můžete zobrazit seznam všech oblastech [této stránce](https://azure.microsoft.com/en-us/regions) nebo pomocí [hello REST API pro správu Azure](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="466d6-165">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="466d6-166">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="466d6-166">retentionInDays</span></span> |<span data-ttu-id="466d6-167">Ano</span><span class="sxs-lookup"><span data-stu-id="466d6-167">Yes</span></span> |<span data-ttu-id="466d6-168">Počet dní pro události, které by měl být zachován, od 1 do 2147483647.</span><span class="sxs-lookup"><span data-stu-id="466d6-168">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="466d6-169">Hodnota nula budou ukládat protokoly hello po neomezenou dobu (navždy).</span><span class="sxs-lookup"><span data-stu-id="466d6-169">A value of zero will store hello logs indefinitely (forever).</span></span> |
| <span data-ttu-id="466d6-170">Kategorie</span><span class="sxs-lookup"><span data-stu-id="466d6-170">categories</span></span> |<span data-ttu-id="466d6-171">Ano</span><span class="sxs-lookup"><span data-stu-id="466d6-171">Yes</span></span> |<span data-ttu-id="466d6-172">Seznam oddělený čárkami kategorií událostí, které by měl být shromážděny.</span><span class="sxs-lookup"><span data-stu-id="466d6-172">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="466d6-173">Možné hodnoty jsou zápisu, odstranění a akce.</span><span class="sxs-lookup"><span data-stu-id="466d6-173">Possible values are Write, Delete, and Action.</span></span> |

## <a name="storage-schema-of-hello-activity-log"></a><span data-ttu-id="466d6-174">Schéma úložiště hello protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="466d6-174">Storage schema of hello Activity Log</span></span>
<span data-ttu-id="466d6-175">Jakmile jste nastavili archivace, kontejner úložiště bude vytvořen v účtu úložiště hello Jakmile dojde k aktivity protokolu události.</span><span class="sxs-lookup"><span data-stu-id="466d6-175">Once you have set up archival, a storage container will be created in hello storage account as soon as an Activity Log event occurs.</span></span> <span data-ttu-id="466d6-176">Hello objektů BLOB v kontejneru hello podle hello stejný formát napříč hello protokol aktivit a diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="466d6-176">hello blobs within hello container follow hello same format across hello Activity Log and Diagnostic Logs.</span></span> <span data-ttu-id="466d6-177">Struktura Hello tyto objekty BLOB je:</span><span class="sxs-lookup"><span data-stu-id="466d6-177">hello structure of these blobs is:</span></span>

> <span data-ttu-id="466d6-178">Statistika provozní protokoly/name = výchozí/resourceId = v odběry / {ID předplatného} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin</span><span class="sxs-lookup"><span data-stu-id="466d6-178">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="466d6-179">Například může být název objektu blob:</span><span class="sxs-lookup"><span data-stu-id="466d6-179">For example, a blob name might be:</span></span>

> <span data-ttu-id="466d6-180">insights-Operational-Logs/Name=default/resourceId=/Subscriptions/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="466d6-180">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="466d6-181">Každý objekt blob PT1H.json obsahuje objekt blob JSON událostí, které nastaly během hodiny hello zadaný v adrese URL objektu blob hello (například h = 12).</span><span class="sxs-lookup"><span data-stu-id="466d6-181">Each PT1H.json blob contains a JSON blob of events that occurred within hello hour specified in hello blob URL (e.g. h=12).</span></span> <span data-ttu-id="466d6-182">Během hello přítomen hodinu události jsou připojením toohello PT1H.json souboru, kdy k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="466d6-182">During hello present hour, events are appended toohello PT1H.json file as they occur.</span></span> <span data-ttu-id="466d6-183">Hello minutu hodnotu (m = 00) je vždy 00, protože aktivity protokolu události jsou rozdělená do jednotlivých objektů blob za hodinu.</span><span class="sxs-lookup"><span data-stu-id="466d6-183">hello minute value (m=00) is always 00, since Activity Log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="466d6-184">V souboru PT1H.json hello se ukládají všechny události v poli záznamů"hello", následující tento formát:</span><span class="sxs-lookup"><span data-stu-id="466d6-184">Within hello PT1H.json file, each event is stored in hello “records” array, following this format:</span></span>

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


| <span data-ttu-id="466d6-185">Název elementu</span><span class="sxs-lookup"><span data-stu-id="466d6-185">Element name</span></span> | <span data-ttu-id="466d6-186">Popis</span><span class="sxs-lookup"><span data-stu-id="466d6-186">Description</span></span> |
| --- | --- |
| <span data-ttu-id="466d6-187">time</span><span class="sxs-lookup"><span data-stu-id="466d6-187">time</span></span> |<span data-ttu-id="466d6-188">Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost.</span><span class="sxs-lookup"><span data-stu-id="466d6-188">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="466d6-189">resourceId</span><span class="sxs-lookup"><span data-stu-id="466d6-189">resourceId</span></span> |<span data-ttu-id="466d6-190">ID prostředku hello dopad prostředků.</span><span class="sxs-lookup"><span data-stu-id="466d6-190">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="466d6-191">operationName</span><span class="sxs-lookup"><span data-stu-id="466d6-191">operationName</span></span> |<span data-ttu-id="466d6-192">Název operace hello.</span><span class="sxs-lookup"><span data-stu-id="466d6-192">Name of hello operation.</span></span> |
| <span data-ttu-id="466d6-193">category</span><span class="sxs-lookup"><span data-stu-id="466d6-193">category</span></span> |<span data-ttu-id="466d6-194">Kategorie hello akce, např.</span><span class="sxs-lookup"><span data-stu-id="466d6-194">Category of hello action, eg.</span></span> <span data-ttu-id="466d6-195">Zápis, čtení, akce.</span><span class="sxs-lookup"><span data-stu-id="466d6-195">Write, Read, Action.</span></span> |
| <span data-ttu-id="466d6-196">resultType</span><span class="sxs-lookup"><span data-stu-id="466d6-196">resultType</span></span> |<span data-ttu-id="466d6-197">Typ výsledku hello Hello např.</span><span class="sxs-lookup"><span data-stu-id="466d6-197">hello type of hello result, eg.</span></span> <span data-ttu-id="466d6-198">Úspěch, chyba, spuštění</span><span class="sxs-lookup"><span data-stu-id="466d6-198">Success, Failure, Start</span></span> |
| <span data-ttu-id="466d6-199">resultSignature</span><span class="sxs-lookup"><span data-stu-id="466d6-199">resultSignature</span></span> |<span data-ttu-id="466d6-200">Závisí na typu prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="466d6-200">Depends on hello resource type.</span></span> |
| <span data-ttu-id="466d6-201">durationMs</span><span class="sxs-lookup"><span data-stu-id="466d6-201">durationMs</span></span> |<span data-ttu-id="466d6-202">Doba trvání operace hello v milisekundách</span><span class="sxs-lookup"><span data-stu-id="466d6-202">Duration of hello operation in milliseconds</span></span> |
| <span data-ttu-id="466d6-203">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="466d6-203">callerIpAddress</span></span> |<span data-ttu-id="466d6-204">IP adresa hello uživatele, který se má provést operaci hello, deklarace nebo SPN deklarace identity na základě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="466d6-204">IP address of hello user who has performed hello operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="466d6-205">correlationId</span><span class="sxs-lookup"><span data-stu-id="466d6-205">correlationId</span></span> |<span data-ttu-id="466d6-206">Obvykle GUID ve formátu řetězce hello.</span><span class="sxs-lookup"><span data-stu-id="466d6-206">Usually a GUID in hello string format.</span></span> <span data-ttu-id="466d6-207">Události, které sdílejí correlationId patří toohello stejné uber akce.</span><span class="sxs-lookup"><span data-stu-id="466d6-207">Events that share a correlationId belong toohello same uber action.</span></span> |
| <span data-ttu-id="466d6-208">identity</span><span class="sxs-lookup"><span data-stu-id="466d6-208">identity</span></span> |<span data-ttu-id="466d6-209">Objekt blob JSON s popisem hello autorizace a deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="466d6-209">JSON blob describing hello authorization and claims.</span></span> |
| <span data-ttu-id="466d6-210">Autorizace</span><span class="sxs-lookup"><span data-stu-id="466d6-210">authorization</span></span> |<span data-ttu-id="466d6-211">Objekt BLOB RBAC vlastností události hello.</span><span class="sxs-lookup"><span data-stu-id="466d6-211">Blob of RBAC properties of hello event.</span></span> <span data-ttu-id="466d6-212">Obvykle zahrnuje vlastnosti "action", "role" a "obor" hello.</span><span class="sxs-lookup"><span data-stu-id="466d6-212">Usually includes hello “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="466d6-213">úroveň</span><span class="sxs-lookup"><span data-stu-id="466d6-213">level</span></span> |<span data-ttu-id="466d6-214">Úroveň události hello.</span><span class="sxs-lookup"><span data-stu-id="466d6-214">Level of hello event.</span></span> <span data-ttu-id="466d6-215">Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné"</span><span class="sxs-lookup"><span data-stu-id="466d6-215">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="466d6-216">location</span><span class="sxs-lookup"><span data-stu-id="466d6-216">location</span></span> |<span data-ttu-id="466d6-217">Oblast, ve které hello umístění k chybě (nebo globální).</span><span class="sxs-lookup"><span data-stu-id="466d6-217">Region in which hello location occurred (or global).</span></span> |
| <span data-ttu-id="466d6-218">properties</span><span class="sxs-lookup"><span data-stu-id="466d6-218">properties</span></span> |<span data-ttu-id="466d6-219">Sada `<Key, Value>` páry (tj. slovník) popisující hello podrobnosti události hello.</span><span class="sxs-lookup"><span data-stu-id="466d6-219">Set of `<Key, Value>` pairs (i.e. Dictionary) describing hello details of hello event.</span></span> |

> [!NOTE]
> <span data-ttu-id="466d6-220">Hello vlastnosti a využití tyto vlastnosti se může lišit v závislosti na hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="466d6-220">hello properties and usage of those properties can vary depending on hello resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="466d6-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="466d6-221">Next steps</span></span>
* [<span data-ttu-id="466d6-222">Stáhnout objekty BLOB pro analýzu</span><span class="sxs-lookup"><span data-stu-id="466d6-222">Download blobs for analysis</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [<span data-ttu-id="466d6-223">Stream hello protokol aktivit tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="466d6-223">Stream hello Activity Log tooEvent Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
* [<span data-ttu-id="466d6-224">Další informace o hello protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="466d6-224">Read more about hello Activity Log</span></span>](monitoring-overview-activity-logs.md)

