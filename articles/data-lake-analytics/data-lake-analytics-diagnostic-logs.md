---
title: "diagnostické protokoly aaaViewing pro Azure Data Lake Analytics | Microsoft Docs"
description: "Pochopit, jak toosetup a přístup diagnostické protokoly pro Azure Data Lake analytics "
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 4cd1eb6f585c1ef96c358340232ef85721a972b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a><span data-ttu-id="59491-103">Přístup k diagnostickým protokolům pro Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="59491-103">Accessing diagnostic logs for Azure Data Lake Analytics</span></span>

<span data-ttu-id="59491-104">Protokolování diagnostiky umožňuje záznamy auditu přístupu toocollect data.</span><span class="sxs-lookup"><span data-stu-id="59491-104">Diagnostic logging allows you toocollect data access audit trails.</span></span> <span data-ttu-id="59491-105">Tyto protokoly obsahují informace, jako:</span><span class="sxs-lookup"><span data-stu-id="59491-105">These logs provide information such as:</span></span>

* <span data-ttu-id="59491-106">Seznam uživatelů, kteří používaná hello data.</span><span class="sxs-lookup"><span data-stu-id="59491-106">A list of users that accessed hello data.</span></span>
* <span data-ttu-id="59491-107">Jak často se hello data používají.</span><span class="sxs-lookup"><span data-stu-id="59491-107">How frequently hello data is accessed.</span></span>
* <span data-ttu-id="59491-108">Množství dat, které je uložený v účtu hello.</span><span class="sxs-lookup"><span data-stu-id="59491-108">How much data is stored in hello account.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="59491-109">Povolit protokolování</span><span class="sxs-lookup"><span data-stu-id="59491-109">Enable logging</span></span>

1. <span data-ttu-id="59491-110">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="59491-110">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="59491-111">Otevřete účet Data Lake Analytics a vyberte **diagnostické protokoly** z hello __monitorování__ části.</span><span class="sxs-lookup"><span data-stu-id="59491-111">Open your Data Lake Analytics account and select **Diagnostic logs** from hello __Monitor__ section.</span></span> <span data-ttu-id="59491-112">Potom vyberte __zapněte diagnostiku__.</span><span class="sxs-lookup"><span data-stu-id="59491-112">Next, select __Turn on diagnostics__.</span></span>

    ![Zapněte diagnostiku toocollect auditu a požadovat protokoly](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. <span data-ttu-id="59491-114">Z __nastavení diagnostiky__, nastavit stav too__On__ hello a vyberte možnosti protokolování.</span><span class="sxs-lookup"><span data-stu-id="59491-114">From __Diagnostics settings__, set hello status too__On__ and select logging options.</span></span>

    <span data-ttu-id="59491-115">![Zapněte diagnostiku toocollect auditu a požadovat protokoly](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "povolení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="59491-115">![Turn on diagnostics toocollect audit and request logs](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>

   * <span data-ttu-id="59491-116">Nastavit **stav** příliš**na** tooenable protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="59491-116">Set **Status** too**On** tooenable diagnostic logging.</span></span>

   * <span data-ttu-id="59491-117">Můžete data toostore/procesu hello třemi různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="59491-117">You can choose toostore/process hello data in three different ways.</span></span>

     * <span data-ttu-id="59491-118">Vyberte __archivu účet úložiště tooa__ toostore protokolů v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="59491-118">Select __Archive tooa storage account__ toostore logs in an Azure storage account.</span></span> <span data-ttu-id="59491-119">Tuto možnost použijte, pokud chcete tooarchive hello data.</span><span class="sxs-lookup"><span data-stu-id="59491-119">Use this option if you want tooarchive hello data.</span></span> <span data-ttu-id="59491-120">Pokud vyberete tuto možnost, je nutné zadat úložiště Azure účet toosave hello protokoluje události do.</span><span class="sxs-lookup"><span data-stu-id="59491-120">If you select this option, you must provide an Azure storage account toosave hello logs to.</span></span>

     * <span data-ttu-id="59491-121">Vyberte **Stream tooan centra událostí** toostream protokolu data tooan centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="59491-121">Select **Stream tooan Event Hub** toostream log data tooan Azure Event Hub.</span></span> <span data-ttu-id="59491-122">Tuto možnost použijte, pokud máte kanál zpracování příjmu dat, který analyzuje příchozí protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="59491-122">Use this option if you have a downstream processing pipeline that is analyzing incoming logs in real time.</span></span> <span data-ttu-id="59491-123">Pokud vyberete tuto možnost, je nutné zadat hello podrobnosti pro hello chcete toouse centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="59491-123">If you select this option, you must provide hello details for hello Azure Event Hub you want toouse.</span></span>

     * <span data-ttu-id="59491-124">Vyberte __odeslat tooLog Analytics__ toosend hello data toohello analýzy protokolů služby.</span><span class="sxs-lookup"><span data-stu-id="59491-124">Select __Send tooLog Analytics__ toosend hello data toohello Log Analytics service.</span></span> <span data-ttu-id="59491-125">Tuto možnost použijte, pokud chcete toogather toouse analýzy protokolů a analýza protokolů.</span><span class="sxs-lookup"><span data-stu-id="59491-125">Use this option if you want toouse Log Analytics toogather and analyze logs.</span></span>
   * <span data-ttu-id="59491-126">Zadejte, zda chcete protokoly auditu tooget nebo protokoly požadavku nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="59491-126">Specify whether you want tooget audit logs or request logs or both.</span></span>  <span data-ttu-id="59491-127">Žádost protokolu zaznamená každého požadavku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59491-127">A request log captures every API request.</span></span> <span data-ttu-id="59491-128">Protokol auditování zaznamenává všechny operace, které jsou aktivovány tohoto požadavku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59491-128">An audit log records all operations that are triggered by that API request.</span></span>

   * <span data-ttu-id="59491-129">Pro __archivu účet úložiště tooa__, zadejte hello počet dní tooretain hello data.</span><span class="sxs-lookup"><span data-stu-id="59491-129">For __Archive tooa storage account__, specify hello number of days tooretain hello data.</span></span>

   * <span data-ttu-id="59491-130">Klikněte na __Uložit__.</span><span class="sxs-lookup"><span data-stu-id="59491-130">Click __Save__.</span></span>

        > [!NOTE]
        > <span data-ttu-id="59491-131">Je nutné vybrat buď __archivu účet úložiště tooa__, __Stream tooan centra událostí__ nebo __odeslat tooLog Analytics__ před kliknutím na tlačítko hello __Uložit__tlačítko.</span><span class="sxs-lookup"><span data-stu-id="59491-131">You must select either __Archive tooa storage account__, __Stream tooan Event Hub__ or __Send tooLog Analytics__ before clicking hello __Save__ button.</span></span>

<span data-ttu-id="59491-132">Jakmile povolíte nastavení diagnostiky, můžete se vrátit toohello __protokolů diagnostiky__ okno tooview hello protokoly.</span><span class="sxs-lookup"><span data-stu-id="59491-132">Once you have enabled diagnostic settings, you can return toohello __Diagnostics logs__ blade tooview hello logs.</span></span>

## <a name="view-logs"></a><span data-ttu-id="59491-133">Zobrazit protokoly</span><span class="sxs-lookup"><span data-stu-id="59491-133">View logs</span></span>

### <a name="use-hello-data-lake-analytics-view"></a><span data-ttu-id="59491-134">Pomocí zobrazení hello Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="59491-134">Use hello Data Lake Analytics view</span></span>

1. <span data-ttu-id="59491-135">Z vaše Data Lake Analytics účet okno, v části **monitorování**, vyberte **diagnostické protokoly** a poté vyberte toodisplay položka protokoly pro.</span><span class="sxs-lookup"><span data-stu-id="59491-135">From your Data Lake Analytics account blade, under **Monitoring**, select **Diagnostic Logs** and then select an entry toodisplay logs for.</span></span>

    <span data-ttu-id="59491-136">![Protokolování diagnostiky zobrazení](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="59491-136">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span>

2. <span data-ttu-id="59491-137">Hello protokoly jsou klasifikovány podle **protokoly auditu** a **požadavku protokoly**.</span><span class="sxs-lookup"><span data-stu-id="59491-137">hello logs are categorized by **Audit Logs** and **Request Logs**.</span></span>

    ![položky protokolu](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * <span data-ttu-id="59491-139">Protokoly žádosti o zachycení každý API požadavek na hello účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="59491-139">Request logs capture every API request made on hello Data Lake Analytics account.</span></span>
   * <span data-ttu-id="59491-140">Protokoly auditu jsou podobné toorequest protokoly, ale poskytují mnohem podrobnější rozpis hello operací.</span><span class="sxs-lookup"><span data-stu-id="59491-140">Audit Logs are similar toorequest Logs but provide a much more detailed breakdown of hello operations.</span></span> <span data-ttu-id="59491-141">Například může způsobit nahrávání jednoho volání rozhraní API v požadavku protokolu více operací "Připojit" v protokolu auditu.</span><span class="sxs-lookup"><span data-stu-id="59491-141">For example, a single upload API call in a request log can result in multiple "Append" operations in its audit log.</span></span>

3. <span data-ttu-id="59491-142">Klikněte na tlačítko hello **Stáhnout** odkaz pro toodownload položky protokolu, který protokolu.</span><span class="sxs-lookup"><span data-stu-id="59491-142">Click hello **Download** link for a log entry toodownload that log.</span></span>

### <a name="use-hello-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="59491-143">Použít účet úložiště Azure hello, který obsahuje data protokolu</span><span class="sxs-lookup"><span data-stu-id="59491-143">Use hello Azure Storage account that contains log data</span></span>

1. <span data-ttu-id="59491-144">Otevřete okno účtu Azure Storage hello spojené s Data Lake Analytics pro protokolování a potom klikněte na __objekty BLOB__.</span><span class="sxs-lookup"><span data-stu-id="59491-144">Open hello Azure Storage account blade associated with Data Lake Analytics for logging, and then click __Blobs__.</span></span> <span data-ttu-id="59491-145">Hello **služba objektů Blob** okno uvádí dva kontejnery.</span><span class="sxs-lookup"><span data-stu-id="59491-145">hello **Blob service** blade lists two containers.</span></span>

    <span data-ttu-id="59491-146">![Protokolování diagnostiky zobrazení](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="59491-146">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>

   * <span data-ttu-id="59491-147">kontejner Hello **přehledy. protokoly auditu** obsahuje hello protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="59491-147">hello container **insights-logs-audit** contains hello audit logs.</span></span>
   * <span data-ttu-id="59491-148">kontejner Hello **přehledy. protokoly žádosti** obsahuje hello požadavek protokoly.</span><span class="sxs-lookup"><span data-stu-id="59491-148">hello container **insights-logs-requests** contains hello request logs.</span></span>
2. <span data-ttu-id="59491-149">V rámci těchto kontejnerů hello protokoly jsou uloženy v části hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="59491-149">Within these containers, hello logs are stored under hello following structure:</span></span>

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > <span data-ttu-id="59491-150">Hello `##` položky v cestě hello obsahují hello rok, měsíc, den a hodinu, ve které hello vytvoření protokolu.</span><span class="sxs-lookup"><span data-stu-id="59491-150">hello `##` entries in hello path contain hello year, month, day, and hour in which hello log was created.</span></span> <span data-ttu-id="59491-151">Data Lake Analytics vytvoří jeden soubor každou hodinu, takže `m=` vždy obsahuje hodnotu `00`.</span><span class="sxs-lookup"><span data-stu-id="59491-151">Data Lake Analytics creates one file every hour, so `m=` always contains a value of `00`.</span></span>

    <span data-ttu-id="59491-152">Jako příklad může být protokolu auditování tooan hello úplnou cestu:</span><span class="sxs-lookup"><span data-stu-id="59491-152">As an example, hello complete path tooan audit log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    <span data-ttu-id="59491-153">Podobně může být hello kompletní cesta tooa žádost protokolu:</span><span class="sxs-lookup"><span data-stu-id="59491-153">Similarly, hello complete path tooa request log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a><span data-ttu-id="59491-154">Struktura protokolu</span><span class="sxs-lookup"><span data-stu-id="59491-154">Log structure</span></span>

<span data-ttu-id="59491-155">Hello protokoly auditu a požadavek se v strukturovaného formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="59491-155">hello audit and request logs are in a structured JSON format.</span></span>

### <a name="request-logs"></a><span data-ttu-id="59491-156">Žádost o protokoly</span><span class="sxs-lookup"><span data-stu-id="59491-156">Request logs</span></span>

<span data-ttu-id="59491-157">Zde je vzorového vstupu v protokolu hello požadavků formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="59491-157">Here's a sample entry in hello JSON-formatted request log.</span></span> <span data-ttu-id="59491-158">Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.</span><span class="sxs-lookup"><span data-stu-id="59491-158">Each blob has one root object called **records** that contains an array of log objects.</span></span>

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a><span data-ttu-id="59491-159">Schéma požadavku protokolu</span><span class="sxs-lookup"><span data-stu-id="59491-159">Request log schema</span></span>

| <span data-ttu-id="59491-160">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="59491-160">Name</span></span> | <span data-ttu-id="59491-161">Typ</span><span class="sxs-lookup"><span data-stu-id="59491-161">Type</span></span> | <span data-ttu-id="59491-162">Popis</span><span class="sxs-lookup"><span data-stu-id="59491-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="59491-163">time</span><span class="sxs-lookup"><span data-stu-id="59491-163">time</span></span> |<span data-ttu-id="59491-164">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-164">String</span></span> |<span data-ttu-id="59491-165">časové razítko Hello (ve formátu UTC) hello protokolu</span><span class="sxs-lookup"><span data-stu-id="59491-165">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="59491-166">resourceId</span><span class="sxs-lookup"><span data-stu-id="59491-166">resourceId</span></span> |<span data-ttu-id="59491-167">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-167">String</span></span> |<span data-ttu-id="59491-168">identifikátor Hello hello prostředku, který operace trvalo umístit na</span><span class="sxs-lookup"><span data-stu-id="59491-168">hello identifier of hello resource that operation took place on</span></span> |
| <span data-ttu-id="59491-169">category</span><span class="sxs-lookup"><span data-stu-id="59491-169">category</span></span> |<span data-ttu-id="59491-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-170">String</span></span> |<span data-ttu-id="59491-171">kategorie Hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="59491-171">hello log category.</span></span> <span data-ttu-id="59491-172">Například **požadavky**.</span><span class="sxs-lookup"><span data-stu-id="59491-172">For example, **Requests**.</span></span> |
| <span data-ttu-id="59491-173">operationName</span><span class="sxs-lookup"><span data-stu-id="59491-173">operationName</span></span> |<span data-ttu-id="59491-174">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-174">String</span></span> |<span data-ttu-id="59491-175">Název hello operace, která je zaznamenána.</span><span class="sxs-lookup"><span data-stu-id="59491-175">Name of hello operation that is logged.</span></span> <span data-ttu-id="59491-176">Například GetAggregatedJobHistory.</span><span class="sxs-lookup"><span data-stu-id="59491-176">For example, GetAggregatedJobHistory.</span></span> |
| <span data-ttu-id="59491-177">resultType</span><span class="sxs-lookup"><span data-stu-id="59491-177">resultType</span></span> |<span data-ttu-id="59491-178">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-178">String</span></span> |<span data-ttu-id="59491-179">Stav Hello hello operace, například 200.</span><span class="sxs-lookup"><span data-stu-id="59491-179">hello status of hello operation, For example, 200.</span></span> |
| <span data-ttu-id="59491-180">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="59491-180">callerIpAddress</span></span> |<span data-ttu-id="59491-181">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-181">String</span></span> |<span data-ttu-id="59491-182">Hello IP adresa klienta hello vytváření hello požadavku</span><span class="sxs-lookup"><span data-stu-id="59491-182">hello IP address of hello client making hello request</span></span> |
| <span data-ttu-id="59491-183">correlationId</span><span class="sxs-lookup"><span data-stu-id="59491-183">correlationId</span></span> |<span data-ttu-id="59491-184">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-184">String</span></span> |<span data-ttu-id="59491-185">identifikátor Hello hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="59491-185">hello identifier of hello log.</span></span> <span data-ttu-id="59491-186">Tato hodnota může být použité toogroup sada položek související protokolu.</span><span class="sxs-lookup"><span data-stu-id="59491-186">This value can be used toogroup a set of related log entries.</span></span> |
| <span data-ttu-id="59491-187">identity</span><span class="sxs-lookup"><span data-stu-id="59491-187">identity</span></span> |<span data-ttu-id="59491-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="59491-188">Object</span></span> |<span data-ttu-id="59491-189">Hello identity, která vygenerovala hello protokolu</span><span class="sxs-lookup"><span data-stu-id="59491-189">hello identity that generated hello log</span></span> |
| <span data-ttu-id="59491-190">properties</span><span class="sxs-lookup"><span data-stu-id="59491-190">properties</span></span> |<span data-ttu-id="59491-191">JSON</span><span class="sxs-lookup"><span data-stu-id="59491-191">JSON</span></span> |<span data-ttu-id="59491-192">Viz další část hello (požadavku protokolu vlastnosti schéma) podrobnosti</span><span class="sxs-lookup"><span data-stu-id="59491-192">See hello next section (Request log properties schema) for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="59491-193">Schéma vlastnosti požadavku protokolu</span><span class="sxs-lookup"><span data-stu-id="59491-193">Request log properties schema</span></span>

| <span data-ttu-id="59491-194">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="59491-194">Name</span></span> | <span data-ttu-id="59491-195">Typ</span><span class="sxs-lookup"><span data-stu-id="59491-195">Type</span></span> | <span data-ttu-id="59491-196">Popis</span><span class="sxs-lookup"><span data-stu-id="59491-196">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="59491-197">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="59491-197">HttpMethod</span></span> |<span data-ttu-id="59491-198">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-198">String</span></span> |<span data-ttu-id="59491-199">Hello metoda HTTP pro hello operaci použít.</span><span class="sxs-lookup"><span data-stu-id="59491-199">hello HTTP Method used for hello operation.</span></span> <span data-ttu-id="59491-200">Například získáte.</span><span class="sxs-lookup"><span data-stu-id="59491-200">For example, GET.</span></span> |
| <span data-ttu-id="59491-201">Cesta</span><span class="sxs-lookup"><span data-stu-id="59491-201">Path</span></span> |<span data-ttu-id="59491-202">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-202">String</span></span> |<span data-ttu-id="59491-203">operace hello Hello cesta byla provedena na</span><span class="sxs-lookup"><span data-stu-id="59491-203">hello path hello operation was performed on</span></span> |
| <span data-ttu-id="59491-204">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="59491-204">RequestContentLength</span></span> |<span data-ttu-id="59491-205">celá čísla</span><span class="sxs-lookup"><span data-stu-id="59491-205">int</span></span> |<span data-ttu-id="59491-206">Délka obsahu Hello hello HTTP požadavku</span><span class="sxs-lookup"><span data-stu-id="59491-206">hello content length of hello HTTP request</span></span> |
| <span data-ttu-id="59491-207">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="59491-207">ClientRequestId</span></span> |<span data-ttu-id="59491-208">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-208">String</span></span> |<span data-ttu-id="59491-209">Hello identifikátor, který jedinečně identifikuje tuto žádost</span><span class="sxs-lookup"><span data-stu-id="59491-209">hello identifier that uniquely identifies this request</span></span> |
| <span data-ttu-id="59491-210">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="59491-210">StartTime</span></span> |<span data-ttu-id="59491-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-211">String</span></span> |<span data-ttu-id="59491-212">Hello čas, který hello server obdržel hello požadavku</span><span class="sxs-lookup"><span data-stu-id="59491-212">hello time at which hello server received hello request</span></span> |
| <span data-ttu-id="59491-213">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="59491-213">EndTime</span></span> |<span data-ttu-id="59491-214">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-214">String</span></span> |<span data-ttu-id="59491-215">Hello čas, který hello serveru odeslání odpovědi</span><span class="sxs-lookup"><span data-stu-id="59491-215">hello time at which hello server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="59491-216">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="59491-216">Audit logs</span></span>

<span data-ttu-id="59491-217">Zde je vstup vzorového ve formátu JSON auditní protokol hello.</span><span class="sxs-lookup"><span data-stu-id="59491-217">Here's a sample entry in hello JSON-formatted audit log.</span></span> <span data-ttu-id="59491-218">Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.</span><span class="sxs-lookup"><span data-stu-id="59491-218">Each blob has one root object called **records** that contains an array of log objects.</span></span>

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a><span data-ttu-id="59491-219">Schéma protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="59491-219">Audit log schema</span></span>

| <span data-ttu-id="59491-220">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="59491-220">Name</span></span> | <span data-ttu-id="59491-221">Typ</span><span class="sxs-lookup"><span data-stu-id="59491-221">Type</span></span> | <span data-ttu-id="59491-222">Popis</span><span class="sxs-lookup"><span data-stu-id="59491-222">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="59491-223">time</span><span class="sxs-lookup"><span data-stu-id="59491-223">time</span></span> |<span data-ttu-id="59491-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-224">String</span></span> |<span data-ttu-id="59491-225">časové razítko Hello (ve formátu UTC) hello protokolu</span><span class="sxs-lookup"><span data-stu-id="59491-225">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="59491-226">resourceId</span><span class="sxs-lookup"><span data-stu-id="59491-226">resourceId</span></span> |<span data-ttu-id="59491-227">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-227">String</span></span> |<span data-ttu-id="59491-228">identifikátor Hello hello prostředku, který operace trvalo umístit na</span><span class="sxs-lookup"><span data-stu-id="59491-228">hello identifier of hello resource that operation took place on</span></span> |
| <span data-ttu-id="59491-229">category</span><span class="sxs-lookup"><span data-stu-id="59491-229">category</span></span> |<span data-ttu-id="59491-230">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-230">String</span></span> |<span data-ttu-id="59491-231">kategorie Hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="59491-231">hello log category.</span></span> <span data-ttu-id="59491-232">Například **auditu**.</span><span class="sxs-lookup"><span data-stu-id="59491-232">For example, **Audit**.</span></span> |
| <span data-ttu-id="59491-233">operationName</span><span class="sxs-lookup"><span data-stu-id="59491-233">operationName</span></span> |<span data-ttu-id="59491-234">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-234">String</span></span> |<span data-ttu-id="59491-235">Název hello operace, která je zaznamenána.</span><span class="sxs-lookup"><span data-stu-id="59491-235">Name of hello operation that is logged.</span></span> <span data-ttu-id="59491-236">Například JobSubmitted.</span><span class="sxs-lookup"><span data-stu-id="59491-236">For example, JobSubmitted.</span></span> |
| <span data-ttu-id="59491-237">resultType</span><span class="sxs-lookup"><span data-stu-id="59491-237">resultType</span></span> |<span data-ttu-id="59491-238">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-238">String</span></span> |<span data-ttu-id="59491-239">Podřízený stav pro stav úlohy hello (operationName).</span><span class="sxs-lookup"><span data-stu-id="59491-239">A substatus for hello job status (operationName).</span></span> |
| <span data-ttu-id="59491-240">resultSignature</span><span class="sxs-lookup"><span data-stu-id="59491-240">resultSignature</span></span> |<span data-ttu-id="59491-241">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-241">String</span></span> |<span data-ttu-id="59491-242">Další informace o stavu úlohy hello (operationName).</span><span class="sxs-lookup"><span data-stu-id="59491-242">Additional details on hello job status (operationName).</span></span> |
| <span data-ttu-id="59491-243">identity</span><span class="sxs-lookup"><span data-stu-id="59491-243">identity</span></span> |<span data-ttu-id="59491-244">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-244">String</span></span> |<span data-ttu-id="59491-245">Hello uživatel, který požadovanou operaci hello.</span><span class="sxs-lookup"><span data-stu-id="59491-245">hello user that requested hello operation.</span></span> <span data-ttu-id="59491-246">Například, susan@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="59491-246">For example, susan@contoso.com.</span></span> |
| <span data-ttu-id="59491-247">properties</span><span class="sxs-lookup"><span data-stu-id="59491-247">properties</span></span> |<span data-ttu-id="59491-248">JSON</span><span class="sxs-lookup"><span data-stu-id="59491-248">JSON</span></span> |<span data-ttu-id="59491-249">Viz další část hello (schéma vlastnosti protokolu auditu) podrobnosti</span><span class="sxs-lookup"><span data-stu-id="59491-249">See hello next section (Audit log properties schema) for details</span></span> |

> [!NOTE]
> <span data-ttu-id="59491-250">**resultType** a **resultSignature** poskytují informace o hello výsledek operace a obsahovat pouze hodnotu, pokud operace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="59491-250">**resultType** and **resultSignature** provide information on hello result of an operation, and only contain a value if an operation has completed.</span></span> <span data-ttu-id="59491-251">Například pouze obsahovat hodnotu při **operationName** obsahuje hodnotu **JobStarted** nebo **JobEnded**.</span><span class="sxs-lookup"><span data-stu-id="59491-251">For example, they only contain a value when **operationName** contains a value of **JobStarted** or **JobEnded**.</span></span>
>
>

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="59491-252">Schéma vlastnosti protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="59491-252">Audit log properties schema</span></span>

| <span data-ttu-id="59491-253">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="59491-253">Name</span></span> | <span data-ttu-id="59491-254">Typ</span><span class="sxs-lookup"><span data-stu-id="59491-254">Type</span></span> | <span data-ttu-id="59491-255">Popis</span><span class="sxs-lookup"><span data-stu-id="59491-255">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="59491-256">JobId</span><span class="sxs-lookup"><span data-stu-id="59491-256">JobId</span></span> |<span data-ttu-id="59491-257">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-257">String</span></span> |<span data-ttu-id="59491-258">Úloha přiřazené toohello ID Hello</span><span class="sxs-lookup"><span data-stu-id="59491-258">hello ID assigned toohello job</span></span> |
| <span data-ttu-id="59491-259">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="59491-259">JobName</span></span> |<span data-ttu-id="59491-260">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-260">String</span></span> |<span data-ttu-id="59491-261">Název Hello, která byla poskytnuta pro úlohy hello</span><span class="sxs-lookup"><span data-stu-id="59491-261">hello name that was provided for hello job</span></span> |
| <span data-ttu-id="59491-262">JobRunTime</span><span class="sxs-lookup"><span data-stu-id="59491-262">JobRunTime</span></span> |<span data-ttu-id="59491-263">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-263">String</span></span> |<span data-ttu-id="59491-264">modul runtime Hello používá tooprocess hello úlohy</span><span class="sxs-lookup"><span data-stu-id="59491-264">hello runtime used tooprocess hello job</span></span> |
| <span data-ttu-id="59491-265">SubmitTime</span><span class="sxs-lookup"><span data-stu-id="59491-265">SubmitTime</span></span> |<span data-ttu-id="59491-266">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-266">String</span></span> |<span data-ttu-id="59491-267">Hello čas (ve formátu UTC) byla odeslána tuto úlohu hello</span><span class="sxs-lookup"><span data-stu-id="59491-267">hello time (in UTC) that hello job was submitted</span></span> |
| <span data-ttu-id="59491-268">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="59491-268">StartTime</span></span> |<span data-ttu-id="59491-269">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-269">String</span></span> |<span data-ttu-id="59491-270">Hello čase hello úloha spuštění po odeslání (ve formátu UTC)</span><span class="sxs-lookup"><span data-stu-id="59491-270">hello time hello job started running after submission (in UTC)</span></span> |
| <span data-ttu-id="59491-271">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="59491-271">EndTime</span></span> |<span data-ttu-id="59491-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-272">String</span></span> |<span data-ttu-id="59491-273">Hello čas hello úloha byla ukončena</span><span class="sxs-lookup"><span data-stu-id="59491-273">hello time hello job ended</span></span> |
| <span data-ttu-id="59491-274">Paralelismus</span><span class="sxs-lookup"><span data-stu-id="59491-274">Parallelism</span></span> |<span data-ttu-id="59491-275">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59491-275">String</span></span> |<span data-ttu-id="59491-276">Hello počet jednotek Data Lake Analytics požadovaný pro tuto úlohu při odesílání</span><span class="sxs-lookup"><span data-stu-id="59491-276">hello number of Data Lake Analytics units requested for this job during submission</span></span> |

> [!NOTE]
> <span data-ttu-id="59491-277">**SubmitTime**, **StartTime**, **EndTime**, a **paralelismus** poskytují informace o operaci.</span><span class="sxs-lookup"><span data-stu-id="59491-277">**SubmitTime**, **StartTime**, **EndTime**, and **Parallelism** provide information on an operation.</span></span> <span data-ttu-id="59491-278">Tyto položky pouze obsahovat hodnotu, pokud které operace má spustit nebo byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="59491-278">These entries only contain a value if that operation has started or completed.</span></span> <span data-ttu-id="59491-279">Například **SubmitTime** pouze obsahuje hodnotu po **operationName** má hodnotu hello **JobSubmitted**.</span><span class="sxs-lookup"><span data-stu-id="59491-279">For example, **SubmitTime** only contains a value after **operationName** has hello value **JobSubmitted**.</span></span>

## <a name="process-hello-log-data"></a><span data-ttu-id="59491-280">Data protokolu zpracování hello</span><span class="sxs-lookup"><span data-stu-id="59491-280">Process hello log data</span></span>

<span data-ttu-id="59491-281">Azure Data Lake Analytics obsahuje ukázku tooprocess a analýze dat protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="59491-281">Azure Data Lake Analytics provides a sample on how tooprocess and analyze hello log data.</span></span> <span data-ttu-id="59491-282">Můžete najít hello ukázku najdete na adrese [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="59491-282">You can find hello sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span>

## <a name="next-steps"></a><span data-ttu-id="59491-283">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59491-283">Next steps</span></span>
* [<span data-ttu-id="59491-284">Přehled Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="59491-284">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
