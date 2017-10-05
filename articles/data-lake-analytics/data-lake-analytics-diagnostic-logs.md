---
title: "Zobrazení diagnostických protokolů pro Azure Data Lake Analytics | Microsoft Docs"
description: "Pochopit, jak nastavit a přístupu k diagnostickým protokolům pro Azure Data Lake analytics "
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
ms.openlocfilehash: 6c74db1659742aa41306388273bec46800ba7609
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a><span data-ttu-id="19924-103">Přístup k diagnostickým protokolům pro Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="19924-103">Accessing diagnostic logs for Azure Data Lake Analytics</span></span>

<span data-ttu-id="19924-104">Protokolování diagnostiky umožňuje shromažďovat záznamy auditu přístupu data.</span><span class="sxs-lookup"><span data-stu-id="19924-104">Diagnostic logging allows you to collect data access audit trails.</span></span> <span data-ttu-id="19924-105">Tyto protokoly obsahují informace, jako:</span><span class="sxs-lookup"><span data-stu-id="19924-105">These logs provide information such as:</span></span>

* <span data-ttu-id="19924-106">Seznam uživatelů, kteří přístup data.</span><span class="sxs-lookup"><span data-stu-id="19924-106">A list of users that accessed the data.</span></span>
* <span data-ttu-id="19924-107">Jak často se data používají.</span><span class="sxs-lookup"><span data-stu-id="19924-107">How frequently the data is accessed.</span></span>
* <span data-ttu-id="19924-108">Množství dat, které je uložený v účtu.</span><span class="sxs-lookup"><span data-stu-id="19924-108">How much data is stored in the account.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="19924-109">Povolit protokolování</span><span class="sxs-lookup"><span data-stu-id="19924-109">Enable logging</span></span>

1. <span data-ttu-id="19924-110">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="19924-110">Sign on to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="19924-111">Otevřete účet Data Lake Analytics a vyberte **diagnostické protokoly** z __monitorování__ části.</span><span class="sxs-lookup"><span data-stu-id="19924-111">Open your Data Lake Analytics account and select **Diagnostic logs** from the __Monitor__ section.</span></span> <span data-ttu-id="19924-112">Potom vyberte __zapněte diagnostiku__.</span><span class="sxs-lookup"><span data-stu-id="19924-112">Next, select __Turn on diagnostics__.</span></span>

    ![Zapněte diagnostiku shromažďovat auditu a protokolů žádosti](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. <span data-ttu-id="19924-114">Z __nastavení diagnostiky__, nastaví stav na __na__ a vyberte možnosti protokolování.</span><span class="sxs-lookup"><span data-stu-id="19924-114">From __Diagnostics settings__, set the status to __On__ and select logging options.</span></span>

    <span data-ttu-id="19924-115">![Zapněte diagnostiku shromažďovat auditu a protokolů žádosti](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "povolení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="19924-115">![Turn on diagnostics to collect audit and request logs](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>

   * <span data-ttu-id="19924-116">Nastavit **stav** k **na** povolit protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="19924-116">Set **Status** to **On** to enable diagnostic logging.</span></span>

   * <span data-ttu-id="19924-117">Můžete úložiště/zpracovat data třemi různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="19924-117">You can choose to store/process the data in three different ways.</span></span>

     * <span data-ttu-id="19924-118">Vyberte __archivu do účtu úložiště__ k ukládání protokolů v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="19924-118">Select __Archive to a storage account__ to store logs in an Azure storage account.</span></span> <span data-ttu-id="19924-119">Tuto možnost použijte, pokud chcete archivovat data.</span><span class="sxs-lookup"><span data-stu-id="19924-119">Use this option if you want to archive the data.</span></span> <span data-ttu-id="19924-120">Pokud vyberete tuto možnost, je nutné zadat účet úložiště Azure pro uložení protokolů.</span><span class="sxs-lookup"><span data-stu-id="19924-120">If you select this option, you must provide an Azure storage account to save the logs to.</span></span>

     * <span data-ttu-id="19924-121">Vyberte **datový proud do centra událostí** na datový proud protokolu data do centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="19924-121">Select **Stream to an Event Hub** to stream log data to an Azure Event Hub.</span></span> <span data-ttu-id="19924-122">Tuto možnost použijte, pokud máte kanál zpracování příjmu dat, který analyzuje příchozí protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="19924-122">Use this option if you have a downstream processing pipeline that is analyzing incoming logs in real time.</span></span> <span data-ttu-id="19924-123">Pokud vyberete tuto možnost, je nutné zadat podrobnosti pro centra událostí Azure, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="19924-123">If you select this option, you must provide the details for the Azure Event Hub you want to use.</span></span>

     * <span data-ttu-id="19924-124">Vyberte __odeslat k analýze protokolů__ k odeslání dat službě Analýza protokolů.</span><span class="sxs-lookup"><span data-stu-id="19924-124">Select __Send to Log Analytics__ to send the data to the Log Analytics service.</span></span> <span data-ttu-id="19924-125">Tuto možnost použijte, pokud chcete použít k shromáždění a analýza protokolů analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="19924-125">Use this option if you want to use Log Analytics to gather and analyze logs.</span></span>
   * <span data-ttu-id="19924-126">Určete, jestli chcete získat protokoly auditu nebo protokoly požadavku nebo obě.</span><span class="sxs-lookup"><span data-stu-id="19924-126">Specify whether you want to get audit logs or request logs or both.</span></span>  <span data-ttu-id="19924-127">Žádost protokolu zaznamená každého požadavku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="19924-127">A request log captures every API request.</span></span> <span data-ttu-id="19924-128">Protokol auditování zaznamenává všechny operace, které jsou aktivovány tohoto požadavku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="19924-128">An audit log records all operations that are triggered by that API request.</span></span>

   * <span data-ttu-id="19924-129">Pro __archivu do účtu úložiště__, zadejte počet dní, které chcete zachovat data.</span><span class="sxs-lookup"><span data-stu-id="19924-129">For __Archive to a storage account__, specify the number of days to retain the data.</span></span>

   * <span data-ttu-id="19924-130">Klikněte na __Uložit__.</span><span class="sxs-lookup"><span data-stu-id="19924-130">Click __Save__.</span></span>

        > [!NOTE]
        > <span data-ttu-id="19924-131">Je nutné vybrat buď __archivu do účtu úložiště__, __datový proud do centra událostí__ nebo __odeslat k analýze protokolů__ před kliknutím na tlačítko __Uložit__ tlačítko.</span><span class="sxs-lookup"><span data-stu-id="19924-131">You must select either __Archive to a storage account__, __Stream to an Event Hub__ or __Send to Log Analytics__ before clicking the __Save__ button.</span></span>

<span data-ttu-id="19924-132">Jakmile povolíte nastavení diagnostiky, můžete se vrátit __protokolů diagnostiky__ okno k zobrazení protokolů.</span><span class="sxs-lookup"><span data-stu-id="19924-132">Once you have enabled diagnostic settings, you can return to the __Diagnostics logs__ blade to view the logs.</span></span>

## <a name="view-logs"></a><span data-ttu-id="19924-133">Zobrazit protokoly</span><span class="sxs-lookup"><span data-stu-id="19924-133">View logs</span></span>

### <a name="use-the-data-lake-analytics-view"></a><span data-ttu-id="19924-134">Pomocí zobrazení Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="19924-134">Use the Data Lake Analytics view</span></span>

1. <span data-ttu-id="19924-135">Z vaše Data Lake Analytics účet okno, v části **monitorování**, vyberte **diagnostické protokoly** a potom vyberte položku, chcete-li zobrazit protokoly pro.</span><span class="sxs-lookup"><span data-stu-id="19924-135">From your Data Lake Analytics account blade, under **Monitoring**, select **Diagnostic Logs** and then select an entry to display logs for.</span></span>

    <span data-ttu-id="19924-136">![Protokolování diagnostiky zobrazení](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="19924-136">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span>

2. <span data-ttu-id="19924-137">Protokoly jsou klasifikovány podle **protokoly auditu** a **požadavku protokoly**.</span><span class="sxs-lookup"><span data-stu-id="19924-137">The logs are categorized by **Audit Logs** and **Request Logs**.</span></span>

    ![položky protokolu](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * <span data-ttu-id="19924-139">Protokoly žádosti o zachycení každý API požadavek na účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="19924-139">Request logs capture every API request made on the Data Lake Analytics account.</span></span>
   * <span data-ttu-id="19924-140">Protokoly auditu jsou podobná žádosti protokoly, ale poskytují mnohem podrobnější rozpis operací.</span><span class="sxs-lookup"><span data-stu-id="19924-140">Audit Logs are similar to request Logs but provide a much more detailed breakdown of the operations.</span></span> <span data-ttu-id="19924-141">Například může způsobit nahrávání jednoho volání rozhraní API v požadavku protokolu více operací "Připojit" v protokolu auditu.</span><span class="sxs-lookup"><span data-stu-id="19924-141">For example, a single upload API call in a request log can result in multiple "Append" operations in its audit log.</span></span>

3. <span data-ttu-id="19924-142">Klikněte **Stáhnout** odkaz pro položky protokolu ke stažení tohoto protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-142">Click the **Download** link for a log entry to download that log.</span></span>

### <a name="use-the-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="19924-143">Použít účet úložiště Azure, který obsahuje data protokolu</span><span class="sxs-lookup"><span data-stu-id="19924-143">Use the Azure Storage account that contains log data</span></span>

1. <span data-ttu-id="19924-144">Otevřete okno účtu úložiště Azure spojené s Data Lake Analytics pro protokolování a potom klikněte na __objekty BLOB__.</span><span class="sxs-lookup"><span data-stu-id="19924-144">Open the Azure Storage account blade associated with Data Lake Analytics for logging, and then click __Blobs__.</span></span> <span data-ttu-id="19924-145">**Služba objektů Blob** okno uvádí dva kontejnery.</span><span class="sxs-lookup"><span data-stu-id="19924-145">The **Blob service** blade lists two containers.</span></span>

    <span data-ttu-id="19924-146">![Protokolování diagnostiky zobrazení](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="19924-146">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>

   * <span data-ttu-id="19924-147">Kontejner **přehledy. protokoly auditu** obsahující protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="19924-147">The container **insights-logs-audit** contains the audit logs.</span></span>
   * <span data-ttu-id="19924-148">Kontejner **přehledy. protokoly žádosti** obsahující protokoly požadavku.</span><span class="sxs-lookup"><span data-stu-id="19924-148">The container **insights-logs-requests** contains the request logs.</span></span>
2. <span data-ttu-id="19924-149">V rámci těchto kontejnerů jsou uloženy protokoly pod následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="19924-149">Within these containers, the logs are stored under the following structure:</span></span>

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
   > <span data-ttu-id="19924-150">`##` Položky v cestě obsahují rok, měsíc, den a hodina, ve kterém byla vytvořena v protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-150">The `##` entries in the path contain the year, month, day, and hour in which the log was created.</span></span> <span data-ttu-id="19924-151">Data Lake Analytics vytvoří jeden soubor každou hodinu, takže `m=` vždy obsahuje hodnotu `00`.</span><span class="sxs-lookup"><span data-stu-id="19924-151">Data Lake Analytics creates one file every hour, so `m=` always contains a value of `00`.</span></span>

    <span data-ttu-id="19924-152">Jako příklad může být úplná cesta k protokolu auditování:</span><span class="sxs-lookup"><span data-stu-id="19924-152">As an example, the complete path to an audit log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    <span data-ttu-id="19924-153">Podobně může být úplná cesta k požadavku protokolu:</span><span class="sxs-lookup"><span data-stu-id="19924-153">Similarly, the complete path to a request log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a><span data-ttu-id="19924-154">Struktura protokolu</span><span class="sxs-lookup"><span data-stu-id="19924-154">Log structure</span></span>

<span data-ttu-id="19924-155">Protokoly auditu a žádosti o jsou v strukturovaného formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="19924-155">The audit and request logs are in a structured JSON format.</span></span>

### <a name="request-logs"></a><span data-ttu-id="19924-156">Žádost o protokoly</span><span class="sxs-lookup"><span data-stu-id="19924-156">Request logs</span></span>

<span data-ttu-id="19924-157">Zde je vzorového vstupu do formátu JSON žádost protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-157">Here's a sample entry in the JSON-formatted request log.</span></span> <span data-ttu-id="19924-158">Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-158">Each blob has one root object called **records** that contains an array of log objects.</span></span>

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

#### <a name="request-log-schema"></a><span data-ttu-id="19924-159">Schéma požadavku protokolu</span><span class="sxs-lookup"><span data-stu-id="19924-159">Request log schema</span></span>

| <span data-ttu-id="19924-160">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="19924-160">Name</span></span> | <span data-ttu-id="19924-161">Typ</span><span class="sxs-lookup"><span data-stu-id="19924-161">Type</span></span> | <span data-ttu-id="19924-162">Popis</span><span class="sxs-lookup"><span data-stu-id="19924-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="19924-163">time</span><span class="sxs-lookup"><span data-stu-id="19924-163">time</span></span> |<span data-ttu-id="19924-164">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-164">String</span></span> |<span data-ttu-id="19924-165">Časové razítko (ve formátu UTC) v protokolu</span><span class="sxs-lookup"><span data-stu-id="19924-165">The timestamp (in UTC) of the log</span></span> |
| <span data-ttu-id="19924-166">resourceId</span><span class="sxs-lookup"><span data-stu-id="19924-166">resourceId</span></span> |<span data-ttu-id="19924-167">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-167">String</span></span> |<span data-ttu-id="19924-168">Identifikátor prostředku, který operace trvalo umístit na</span><span class="sxs-lookup"><span data-stu-id="19924-168">The identifier of the resource that operation took place on</span></span> |
| <span data-ttu-id="19924-169">category</span><span class="sxs-lookup"><span data-stu-id="19924-169">category</span></span> |<span data-ttu-id="19924-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-170">String</span></span> |<span data-ttu-id="19924-171">Kategorie protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-171">The log category.</span></span> <span data-ttu-id="19924-172">Například **požadavky**.</span><span class="sxs-lookup"><span data-stu-id="19924-172">For example, **Requests**.</span></span> |
| <span data-ttu-id="19924-173">operationName</span><span class="sxs-lookup"><span data-stu-id="19924-173">operationName</span></span> |<span data-ttu-id="19924-174">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-174">String</span></span> |<span data-ttu-id="19924-175">Název operace, která je zaznamenána.</span><span class="sxs-lookup"><span data-stu-id="19924-175">Name of the operation that is logged.</span></span> <span data-ttu-id="19924-176">Například GetAggregatedJobHistory.</span><span class="sxs-lookup"><span data-stu-id="19924-176">For example, GetAggregatedJobHistory.</span></span> |
| <span data-ttu-id="19924-177">resultType</span><span class="sxs-lookup"><span data-stu-id="19924-177">resultType</span></span> |<span data-ttu-id="19924-178">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-178">String</span></span> |<span data-ttu-id="19924-179">Stav operace, například 200.</span><span class="sxs-lookup"><span data-stu-id="19924-179">The status of the operation, For example, 200.</span></span> |
| <span data-ttu-id="19924-180">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="19924-180">callerIpAddress</span></span> |<span data-ttu-id="19924-181">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-181">String</span></span> |<span data-ttu-id="19924-182">IP adresa klienta vytvoření požadavku</span><span class="sxs-lookup"><span data-stu-id="19924-182">The IP address of the client making the request</span></span> |
| <span data-ttu-id="19924-183">correlationId</span><span class="sxs-lookup"><span data-stu-id="19924-183">correlationId</span></span> |<span data-ttu-id="19924-184">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-184">String</span></span> |<span data-ttu-id="19924-185">Identifikátor protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-185">The identifier of the log.</span></span> <span data-ttu-id="19924-186">Tato hodnota slouží k seskupení sady položek související protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-186">This value can be used to group a set of related log entries.</span></span> |
| <span data-ttu-id="19924-187">identity</span><span class="sxs-lookup"><span data-stu-id="19924-187">identity</span></span> |<span data-ttu-id="19924-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="19924-188">Object</span></span> |<span data-ttu-id="19924-189">Identity, která generuje protokol</span><span class="sxs-lookup"><span data-stu-id="19924-189">The identity that generated the log</span></span> |
| <span data-ttu-id="19924-190">properties</span><span class="sxs-lookup"><span data-stu-id="19924-190">properties</span></span> |<span data-ttu-id="19924-191">JSON</span><span class="sxs-lookup"><span data-stu-id="19924-191">JSON</span></span> |<span data-ttu-id="19924-192">Najdete v další části (požadavku protokolu vlastnosti schéma), podrobnosti</span><span class="sxs-lookup"><span data-stu-id="19924-192">See the next section (Request log properties schema) for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="19924-193">Schéma vlastnosti požadavku protokolu</span><span class="sxs-lookup"><span data-stu-id="19924-193">Request log properties schema</span></span>

| <span data-ttu-id="19924-194">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="19924-194">Name</span></span> | <span data-ttu-id="19924-195">Typ</span><span class="sxs-lookup"><span data-stu-id="19924-195">Type</span></span> | <span data-ttu-id="19924-196">Popis</span><span class="sxs-lookup"><span data-stu-id="19924-196">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="19924-197">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="19924-197">HttpMethod</span></span> |<span data-ttu-id="19924-198">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-198">String</span></span> |<span data-ttu-id="19924-199">Metoda HTTP se používá pro operaci.</span><span class="sxs-lookup"><span data-stu-id="19924-199">The HTTP Method used for the operation.</span></span> <span data-ttu-id="19924-200">Například získáte.</span><span class="sxs-lookup"><span data-stu-id="19924-200">For example, GET.</span></span> |
| <span data-ttu-id="19924-201">Cesta</span><span class="sxs-lookup"><span data-stu-id="19924-201">Path</span></span> |<span data-ttu-id="19924-202">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-202">String</span></span> |<span data-ttu-id="19924-203">Cesta k operaci byla provedena na</span><span class="sxs-lookup"><span data-stu-id="19924-203">The path the operation was performed on</span></span> |
| <span data-ttu-id="19924-204">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="19924-204">RequestContentLength</span></span> |<span data-ttu-id="19924-205">celá čísla</span><span class="sxs-lookup"><span data-stu-id="19924-205">int</span></span> |<span data-ttu-id="19924-206">Délka obsahu požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="19924-206">The content length of the HTTP request</span></span> |
| <span data-ttu-id="19924-207">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="19924-207">ClientRequestId</span></span> |<span data-ttu-id="19924-208">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-208">String</span></span> |<span data-ttu-id="19924-209">Identifikátor, který jedinečně identifikuje tuto žádost</span><span class="sxs-lookup"><span data-stu-id="19924-209">The identifier that uniquely identifies this request</span></span> |
| <span data-ttu-id="19924-210">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="19924-210">StartTime</span></span> |<span data-ttu-id="19924-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-211">String</span></span> |<span data-ttu-id="19924-212">Čas, na které server přijal žádost</span><span class="sxs-lookup"><span data-stu-id="19924-212">The time at which the server received the request</span></span> |
| <span data-ttu-id="19924-213">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="19924-213">EndTime</span></span> |<span data-ttu-id="19924-214">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-214">String</span></span> |<span data-ttu-id="19924-215">Čas, kdy server odeslal odpověď</span><span class="sxs-lookup"><span data-stu-id="19924-215">The time at which the server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="19924-216">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="19924-216">Audit logs</span></span>

<span data-ttu-id="19924-217">Zde je vzorového vstupu v protokolu auditování formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="19924-217">Here's a sample entry in the JSON-formatted audit log.</span></span> <span data-ttu-id="19924-218">Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-218">Each blob has one root object called **records** that contains an array of log objects.</span></span>

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

#### <a name="audit-log-schema"></a><span data-ttu-id="19924-219">Schéma protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="19924-219">Audit log schema</span></span>

| <span data-ttu-id="19924-220">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="19924-220">Name</span></span> | <span data-ttu-id="19924-221">Typ</span><span class="sxs-lookup"><span data-stu-id="19924-221">Type</span></span> | <span data-ttu-id="19924-222">Popis</span><span class="sxs-lookup"><span data-stu-id="19924-222">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="19924-223">time</span><span class="sxs-lookup"><span data-stu-id="19924-223">time</span></span> |<span data-ttu-id="19924-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-224">String</span></span> |<span data-ttu-id="19924-225">Časové razítko (ve formátu UTC) v protokolu</span><span class="sxs-lookup"><span data-stu-id="19924-225">The timestamp (in UTC) of the log</span></span> |
| <span data-ttu-id="19924-226">resourceId</span><span class="sxs-lookup"><span data-stu-id="19924-226">resourceId</span></span> |<span data-ttu-id="19924-227">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-227">String</span></span> |<span data-ttu-id="19924-228">Identifikátor prostředku, který operace trvalo umístit na</span><span class="sxs-lookup"><span data-stu-id="19924-228">The identifier of the resource that operation took place on</span></span> |
| <span data-ttu-id="19924-229">category</span><span class="sxs-lookup"><span data-stu-id="19924-229">category</span></span> |<span data-ttu-id="19924-230">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-230">String</span></span> |<span data-ttu-id="19924-231">Kategorie protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-231">The log category.</span></span> <span data-ttu-id="19924-232">Například **auditu**.</span><span class="sxs-lookup"><span data-stu-id="19924-232">For example, **Audit**.</span></span> |
| <span data-ttu-id="19924-233">operationName</span><span class="sxs-lookup"><span data-stu-id="19924-233">operationName</span></span> |<span data-ttu-id="19924-234">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-234">String</span></span> |<span data-ttu-id="19924-235">Název operace, která je zaznamenána.</span><span class="sxs-lookup"><span data-stu-id="19924-235">Name of the operation that is logged.</span></span> <span data-ttu-id="19924-236">Například JobSubmitted.</span><span class="sxs-lookup"><span data-stu-id="19924-236">For example, JobSubmitted.</span></span> |
| <span data-ttu-id="19924-237">resultType</span><span class="sxs-lookup"><span data-stu-id="19924-237">resultType</span></span> |<span data-ttu-id="19924-238">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-238">String</span></span> |<span data-ttu-id="19924-239">Podřízený stav pro stav úlohy (operationName).</span><span class="sxs-lookup"><span data-stu-id="19924-239">A substatus for the job status (operationName).</span></span> |
| <span data-ttu-id="19924-240">resultSignature</span><span class="sxs-lookup"><span data-stu-id="19924-240">resultSignature</span></span> |<span data-ttu-id="19924-241">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-241">String</span></span> |<span data-ttu-id="19924-242">Další informace o stavu úlohy (operationName).</span><span class="sxs-lookup"><span data-stu-id="19924-242">Additional details on the job status (operationName).</span></span> |
| <span data-ttu-id="19924-243">identity</span><span class="sxs-lookup"><span data-stu-id="19924-243">identity</span></span> |<span data-ttu-id="19924-244">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-244">String</span></span> |<span data-ttu-id="19924-245">Uživatel, který požadovanou operaci.</span><span class="sxs-lookup"><span data-stu-id="19924-245">The user that requested the operation.</span></span> <span data-ttu-id="19924-246">Například, susan@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="19924-246">For example, susan@contoso.com.</span></span> |
| <span data-ttu-id="19924-247">properties</span><span class="sxs-lookup"><span data-stu-id="19924-247">properties</span></span> |<span data-ttu-id="19924-248">JSON</span><span class="sxs-lookup"><span data-stu-id="19924-248">JSON</span></span> |<span data-ttu-id="19924-249">Viz další část (schéma vlastnosti protokolu auditu) podrobností</span><span class="sxs-lookup"><span data-stu-id="19924-249">See the next section (Audit log properties schema) for details</span></span> |

> [!NOTE]
> <span data-ttu-id="19924-250">**resultType** a **resultSignature** poskytují informace o výsledku operace a obsahovat pouze hodnotu, pokud operace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="19924-250">**resultType** and **resultSignature** provide information on the result of an operation, and only contain a value if an operation has completed.</span></span> <span data-ttu-id="19924-251">Například pouze obsahovat hodnotu při **operationName** obsahuje hodnotu **JobStarted** nebo **JobEnded**.</span><span class="sxs-lookup"><span data-stu-id="19924-251">For example, they only contain a value when **operationName** contains a value of **JobStarted** or **JobEnded**.</span></span>
>
>

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="19924-252">Schéma vlastnosti protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="19924-252">Audit log properties schema</span></span>

| <span data-ttu-id="19924-253">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="19924-253">Name</span></span> | <span data-ttu-id="19924-254">Typ</span><span class="sxs-lookup"><span data-stu-id="19924-254">Type</span></span> | <span data-ttu-id="19924-255">Popis</span><span class="sxs-lookup"><span data-stu-id="19924-255">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="19924-256">JobId</span><span class="sxs-lookup"><span data-stu-id="19924-256">JobId</span></span> |<span data-ttu-id="19924-257">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-257">String</span></span> |<span data-ttu-id="19924-258">ID přiřazené úlohy</span><span class="sxs-lookup"><span data-stu-id="19924-258">The ID assigned to the job</span></span> |
| <span data-ttu-id="19924-259">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="19924-259">JobName</span></span> |<span data-ttu-id="19924-260">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-260">String</span></span> |<span data-ttu-id="19924-261">Název, která byla poskytnuta pro úlohu</span><span class="sxs-lookup"><span data-stu-id="19924-261">The name that was provided for the job</span></span> |
| <span data-ttu-id="19924-262">JobRunTime</span><span class="sxs-lookup"><span data-stu-id="19924-262">JobRunTime</span></span> |<span data-ttu-id="19924-263">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-263">String</span></span> |<span data-ttu-id="19924-264">Modul runtime používá ke zpracování úlohy</span><span class="sxs-lookup"><span data-stu-id="19924-264">The runtime used to process the job</span></span> |
| <span data-ttu-id="19924-265">SubmitTime</span><span class="sxs-lookup"><span data-stu-id="19924-265">SubmitTime</span></span> |<span data-ttu-id="19924-266">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-266">String</span></span> |<span data-ttu-id="19924-267">Čas (v UTC), který úloha byla odeslána.</span><span class="sxs-lookup"><span data-stu-id="19924-267">The time (in UTC) that the job was submitted</span></span> |
| <span data-ttu-id="19924-268">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="19924-268">StartTime</span></span> |<span data-ttu-id="19924-269">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-269">String</span></span> |<span data-ttu-id="19924-270">Čas úlohy spuštění po odeslání (ve formátu UTC)</span><span class="sxs-lookup"><span data-stu-id="19924-270">The time the job started running after submission (in UTC)</span></span> |
| <span data-ttu-id="19924-271">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="19924-271">EndTime</span></span> |<span data-ttu-id="19924-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-272">String</span></span> |<span data-ttu-id="19924-273">Čas ukončení úkolu</span><span class="sxs-lookup"><span data-stu-id="19924-273">The time the job ended</span></span> |
| <span data-ttu-id="19924-274">Paralelismus</span><span class="sxs-lookup"><span data-stu-id="19924-274">Parallelism</span></span> |<span data-ttu-id="19924-275">Řetězec</span><span class="sxs-lookup"><span data-stu-id="19924-275">String</span></span> |<span data-ttu-id="19924-276">Počet jednotek Data Lake Analytics požadovaný pro tuto úlohu při odesílání</span><span class="sxs-lookup"><span data-stu-id="19924-276">The number of Data Lake Analytics units requested for this job during submission</span></span> |

> [!NOTE]
> <span data-ttu-id="19924-277">**SubmitTime**, **StartTime**, **EndTime**, a **paralelismus** poskytují informace o operaci.</span><span class="sxs-lookup"><span data-stu-id="19924-277">**SubmitTime**, **StartTime**, **EndTime**, and **Parallelism** provide information on an operation.</span></span> <span data-ttu-id="19924-278">Tyto položky pouze obsahovat hodnotu, pokud které operace má spustit nebo byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="19924-278">These entries only contain a value if that operation has started or completed.</span></span> <span data-ttu-id="19924-279">Například **SubmitTime** pouze obsahuje hodnotu po **operationName** má hodnotu **JobSubmitted**.</span><span class="sxs-lookup"><span data-stu-id="19924-279">For example, **SubmitTime** only contains a value after **operationName** has the value **JobSubmitted**.</span></span>

## <a name="process-the-log-data"></a><span data-ttu-id="19924-280">Zpracování dat protokolu</span><span class="sxs-lookup"><span data-stu-id="19924-280">Process the log data</span></span>

<span data-ttu-id="19924-281">Azure Data Lake Analytics poskytuje vzorku o tom, jak zpracovávat a analyzovat data protokolu.</span><span class="sxs-lookup"><span data-stu-id="19924-281">Azure Data Lake Analytics provides a sample on how to process and analyze the log data.</span></span> <span data-ttu-id="19924-282">Můžete najít ukázku najdete na adrese [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="19924-282">You can find the sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span>

## <a name="next-steps"></a><span data-ttu-id="19924-283">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19924-283">Next steps</span></span>
* [<span data-ttu-id="19924-284">Přehled Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="19924-284">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
