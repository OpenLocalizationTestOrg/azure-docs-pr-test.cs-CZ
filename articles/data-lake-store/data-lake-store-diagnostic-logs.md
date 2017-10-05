---
title: "Zobrazení diagnostických protokolů pro Azure Data Lake Store | Microsoft Docs"
description: "Pochopit, jak nastavit a přístupu k diagnostickým protokolům pro Azure Data Lake Store "
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: b7a38ec445ef0ce13f3f1931e8ee246dce6412a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a><span data-ttu-id="73688-103">Přístup k diagnostickým protokolům pro Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73688-103">Accessing diagnostic logs for Azure Data Lake Store</span></span>
<span data-ttu-id="73688-104">Další informace o tom, jak povolit protokolování diagnostiky pro váš účet Data Lake Store a postup zobrazení shromážděné pro váš účet protokoly.</span><span class="sxs-lookup"><span data-stu-id="73688-104">Learn about how to enable diagnostic logging for your Data Lake Store account and how to view the logs collected for your account.</span></span>

<span data-ttu-id="73688-105">Organizace může povolit protokolování diagnostiky ke svému účtu Azure Data Lake Store ke shromažďování dat záznamy auditu přístupu, která poskytuje informace, jako je seznam uživatelům přístup k datům, jak často se data používají, kolik dat je uložené v účtu atd.</span><span class="sxs-lookup"><span data-stu-id="73688-105">Organizations can enable diagnostic logging for their Azure Data Lake Store account to collect data access audit trails that provides information such as list of users accessing the data, how frequently the data is accessed, how much data is stored in the account, etc.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73688-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="73688-106">Prerequisites</span></span>
* <span data-ttu-id="73688-107">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="73688-107">**An Azure subscription**.</span></span> <span data-ttu-id="73688-108">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="73688-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="73688-109">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="73688-109">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="73688-110">Postupujte podle pokynů v tématu [Začínáme s Azure Data Lake Store s použitím webu Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="73688-110">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span>

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a><span data-ttu-id="73688-111">Povolit protokolování diagnostiky pro váš účet Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73688-111">Enable diagnostic logging for your Data Lake Store account</span></span>
1. <span data-ttu-id="73688-112">Přihlaste se k novému [webu Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="73688-112">Sign on to the new [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="73688-113">Otevřete účet Data Lake Store a z vaší okně účtu Data Lake Store klikněte na tlačítko **nastavení**a potom klikněte na **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="73688-113">Open your Data Lake Store account, and from your Data Lake Store account blade, click **Settings**, and then click **Diagnostic logs**.</span></span>
3. <span data-ttu-id="73688-114">V **protokolů diagnostiky** okně klikněte na tlačítko **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="73688-114">In the **Diagnostics logs** blade, click **Turn on diagnostics**.</span></span>

    <span data-ttu-id="73688-115">![Povolit protokolování diagnostiky](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "povolení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="73688-115">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "Enable diagnostic logs")</span></span>

3. <span data-ttu-id="73688-116">V **diagnostiky** okno, proveďte následující změny konfigurace protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="73688-116">In the **Diagnostic** blade, make the following changes to configure diagnostic logging.</span></span>
   
    <span data-ttu-id="73688-117">![Povolit protokolování diagnostiky](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "povolení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="73688-117">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>
   
   * <span data-ttu-id="73688-118">Nastavit **stav** k **na** povolit protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="73688-118">Set **Status** to **On** to enable diagnostic logging.</span></span>
   * <span data-ttu-id="73688-119">Můžete úložiště/zpracovat data různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="73688-119">You can choose to store/process the data in different ways.</span></span>
     
        * <span data-ttu-id="73688-120">Vyberte možnost **archivu do účtu úložiště** k ukládání protokolů pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="73688-120">Select the option to **Archive to a storage account** to store logs to an Azure Storage account.</span></span> <span data-ttu-id="73688-121">Tuto možnost použijte, pokud chcete archivovat data, která bude zpracování dávky později.</span><span class="sxs-lookup"><span data-stu-id="73688-121">You use this option if you want to archive the data that will be batch-processed at a later date.</span></span> <span data-ttu-id="73688-122">Pokud vyberete tuto možnost je nutné zadat účet služby Azure Storage k ukládání protokolů.</span><span class="sxs-lookup"><span data-stu-id="73688-122">If you select this option you must provide an Azure Storage account to save the logs to.</span></span>
        
        * <span data-ttu-id="73688-123">Vyberte možnost **datový proud do centra událostí** na datový proud protokolu data do centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="73688-123">Select the option to **Stream to an event hub** to stream log data to an Azure Event Hub.</span></span> <span data-ttu-id="73688-124">Bude s největší pravděpodobností používat tuto možnost, pokud máte kanálu zpracování příjmu dat pro analýzu příchozích protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="73688-124">Most likely you will use this option if you have a downstream processing pipeline to analyze incoming logs at real time.</span></span> <span data-ttu-id="73688-125">Pokud vyberete tuto možnost, je nutné zadat podrobnosti pro centra událostí Azure, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="73688-125">If you select this option, you must provide the details for the Azure Event Hub you want to use.</span></span>

        * <span data-ttu-id="73688-126">Vyberte možnost **odeslat k analýze protokolů** používat službu Azure Log Analytics k analýze dat generovaný protokolu.</span><span class="sxs-lookup"><span data-stu-id="73688-126">Select the option to **Send to Log Analytics** to use the Azure Log Analytics service to analyze the generated log data.</span></span> <span data-ttu-id="73688-127">Pokud vyberete tuto možnost, je nutné zadat podrobnosti pro pracovní prostor služby Operations Management Suite se, že používáte analýzy protokolů provést.</span><span class="sxs-lookup"><span data-stu-id="73688-127">If you select this option, you must provide the details for the Operations Management Suite workspace that you would use the perform log analysis.</span></span>
     
   * <span data-ttu-id="73688-128">Určete, jestli chcete získat protokoly auditu nebo protokoly požadavku nebo obě.</span><span class="sxs-lookup"><span data-stu-id="73688-128">Specify whether you want to get audit logs or request logs or both.</span></span>
   * <span data-ttu-id="73688-129">Zadejte počet dnů, pro které se musí uchovávat data.</span><span class="sxs-lookup"><span data-stu-id="73688-129">Specify the number of days for which the data must be retained.</span></span> <span data-ttu-id="73688-130">Uchování platí pouze pokud používáte k archivaci dat protokolu účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="73688-130">Retention is only applicable if you are using Azure storage account to archive log data.</span></span>
   * <span data-ttu-id="73688-131">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="73688-131">Click **Save**.</span></span>

<span data-ttu-id="73688-132">Jakmile povolíte nastavení diagnostiky, můžete sledovat v protokolech **diagnostické protokoly** kartě.</span><span class="sxs-lookup"><span data-stu-id="73688-132">Once you have enabled diagnostic settings, you can watch the logs in the **Diagnostic Logs** tab.</span></span>

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a><span data-ttu-id="73688-133">Zobrazení diagnostických protokolů pro účet Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73688-133">View diagnostic logs for your Data Lake Store account</span></span>
<span data-ttu-id="73688-134">Existují dva způsoby, jak zobrazit data protokolu pro váš účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="73688-134">There are two ways to view the log data for your Data Lake Store account.</span></span>

* <span data-ttu-id="73688-135">V zobrazení nastavení účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73688-135">From the Data Lake Store account settings view</span></span>
* <span data-ttu-id="73688-136">Z účtu úložiště Azure, kde jsou data uložená</span><span class="sxs-lookup"><span data-stu-id="73688-136">From the Azure Storage account where the data is stored</span></span>

### <a name="using-the-data-lake-store-settings-view"></a><span data-ttu-id="73688-137">Pomocí Data Lake Store nastavení zobrazení</span><span class="sxs-lookup"><span data-stu-id="73688-137">Using the Data Lake Store Settings view</span></span>
1. <span data-ttu-id="73688-138">Z účtu Data Lake Store **nastavení** okně klikněte na tlačítko **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="73688-138">From your Data Lake Store account **Settings** blade, click **Diagnostic Logs**.</span></span>
   
    <span data-ttu-id="73688-139">![Protokolování diagnostiky zobrazení](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="73688-139">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span> 
2. <span data-ttu-id="73688-140">V **diagnostické protokoly** okno, měli byste vidět protokoly zařazený do kategorie službou **protokoly auditu** a **požadavku protokoly**.</span><span class="sxs-lookup"><span data-stu-id="73688-140">In the **Diagnostic Logs** blade, you should see the logs categorized by **Audit Logs** and **Request Logs**.</span></span>
   
   * <span data-ttu-id="73688-141">Protokoly žádosti o zachycení každý API požadavek na účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="73688-141">Request logs capture every API request made on the Data Lake Store account.</span></span>
   * <span data-ttu-id="73688-142">Protokoly auditu jsou podobná žádosti protokoly, ale poskytují mnohem podrobnější rozpis operací týkajících se účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="73688-142">Audit Logs are similar to request Logs but provide a much more detailed breakdown of the operations being performed on the Data Lake Store account.</span></span> <span data-ttu-id="73688-143">Například volání rozhraní API jednoho uložení v protokolech žádost může způsobit více operací "Připojit" v protokolech auditu.</span><span class="sxs-lookup"><span data-stu-id="73688-143">For example, a single upload API call in request logs might result in multiple "Append" operations in the audit logs.</span></span>
3. <span data-ttu-id="73688-144">Klikněte **Stáhnout** odkaz u každé položky protokolu pro stahování protokolů.</span><span class="sxs-lookup"><span data-stu-id="73688-144">Click the **Download** link against each log entry to download the logs.</span></span>

### <a name="from-the-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="73688-145">Z účtu úložiště Azure, který obsahuje data protokolu</span><span class="sxs-lookup"><span data-stu-id="73688-145">From the Azure Storage account that contains log data</span></span>
1. <span data-ttu-id="73688-146">Otevřete okno účtu úložiště Azure spojené s Data Lake Store pro protokolování a pak klikněte na objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="73688-146">Open the Azure Storage account blade associated with Data Lake Store for logging, and then click Blobs.</span></span> <span data-ttu-id="73688-147">**Služba objektů Blob** okno uvádí dva kontejnery.</span><span class="sxs-lookup"><span data-stu-id="73688-147">The **Blob service** blade lists two containers.</span></span>
   
    <span data-ttu-id="73688-148">![Protokolování diagnostiky zobrazení](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="73688-148">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>
   
   * <span data-ttu-id="73688-149">Kontejner **přehledy. protokoly auditu** obsahující protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="73688-149">The container **insights-logs-audit** contains the audit logs.</span></span>
   * <span data-ttu-id="73688-150">Kontejner **přehledy. protokoly žádosti** obsahující protokoly požadavku.</span><span class="sxs-lookup"><span data-stu-id="73688-150">The container **insights-logs-requests** contains the request logs.</span></span>
2. <span data-ttu-id="73688-151">V rámci těchto kontejnerů protokoly jsou uloženy v rámci následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="73688-151">Within these containers, the logs are stored under the following structure.</span></span>
   
    <span data-ttu-id="73688-152">![Protokolování diagnostiky zobrazení](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="73688-152">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "View diagnostic logs")</span></span>
   
    <span data-ttu-id="73688-153">Jako příklad může být úplná cesta k protokolu auditování`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="73688-153">As an example, the complete path to an audit log could be `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span></span>
   
    <span data-ttu-id="73688-154">Obdobně, může být úplná cesta k požadavku protokolu`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="73688-154">Similary, the complete path to a request log could be `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span></span>

## <a name="understand-the-structure-of-the-log-data"></a><span data-ttu-id="73688-155">Pochopení struktury data protokolu</span><span class="sxs-lookup"><span data-stu-id="73688-155">Understand the structure of the log data</span></span>
<span data-ttu-id="73688-156">Protokoly auditu a požadavku jsou ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="73688-156">The audit and request logs are in a JSON format.</span></span> <span data-ttu-id="73688-157">V této části jsme zobrazit strukturu JSON pro požadavek a protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="73688-157">In this section, we look at the structure of JSON for request and audit logs.</span></span>

### <a name="request-logs"></a><span data-ttu-id="73688-158">Žádost o protokoly</span><span class="sxs-lookup"><span data-stu-id="73688-158">Request logs</span></span>
<span data-ttu-id="73688-159">Zde je vzorového vstupu do formátu JSON žádost protokolu.</span><span class="sxs-lookup"><span data-stu-id="73688-159">Here's a sample entry in the JSON-formatted request log.</span></span> <span data-ttu-id="73688-160">Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.</span><span class="sxs-lookup"><span data-stu-id="73688-160">Each blob has one root object called **records** that contains an array of log objects.</span></span>

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a><span data-ttu-id="73688-161">Schéma požadavku protokolu</span><span class="sxs-lookup"><span data-stu-id="73688-161">Request log schema</span></span>
| <span data-ttu-id="73688-162">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="73688-162">Name</span></span> | <span data-ttu-id="73688-163">Typ</span><span class="sxs-lookup"><span data-stu-id="73688-163">Type</span></span> | <span data-ttu-id="73688-164">Popis</span><span class="sxs-lookup"><span data-stu-id="73688-164">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73688-165">time</span><span class="sxs-lookup"><span data-stu-id="73688-165">time</span></span> |<span data-ttu-id="73688-166">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-166">String</span></span> |<span data-ttu-id="73688-167">Časové razítko (ve formátu UTC) v protokolu</span><span class="sxs-lookup"><span data-stu-id="73688-167">The timestamp (in UTC) of the log</span></span> |
| <span data-ttu-id="73688-168">resourceId</span><span class="sxs-lookup"><span data-stu-id="73688-168">resourceId</span></span> |<span data-ttu-id="73688-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-169">String</span></span> |<span data-ttu-id="73688-170">ID prostředku, který operace trvalo umístit na</span><span class="sxs-lookup"><span data-stu-id="73688-170">The ID of the resource that operation took place on</span></span> |
| <span data-ttu-id="73688-171">category</span><span class="sxs-lookup"><span data-stu-id="73688-171">category</span></span> |<span data-ttu-id="73688-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-172">String</span></span> |<span data-ttu-id="73688-173">Kategorie protokolu.</span><span class="sxs-lookup"><span data-stu-id="73688-173">The log category.</span></span> <span data-ttu-id="73688-174">Například **požadavky**.</span><span class="sxs-lookup"><span data-stu-id="73688-174">For example, **Requests**.</span></span> |
| <span data-ttu-id="73688-175">operationName</span><span class="sxs-lookup"><span data-stu-id="73688-175">operationName</span></span> |<span data-ttu-id="73688-176">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-176">String</span></span> |<span data-ttu-id="73688-177">Název operace, která je zaznamenána.</span><span class="sxs-lookup"><span data-stu-id="73688-177">Name of the operation that is logged.</span></span> <span data-ttu-id="73688-178">Například getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="73688-178">For example, getfilestatus.</span></span> |
| <span data-ttu-id="73688-179">resultType</span><span class="sxs-lookup"><span data-stu-id="73688-179">resultType</span></span> |<span data-ttu-id="73688-180">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-180">String</span></span> |<span data-ttu-id="73688-181">Stav operace, například 200.</span><span class="sxs-lookup"><span data-stu-id="73688-181">The status of the operation, For example, 200.</span></span> |
| <span data-ttu-id="73688-182">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="73688-182">callerIpAddress</span></span> |<span data-ttu-id="73688-183">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-183">String</span></span> |<span data-ttu-id="73688-184">IP adresa klienta vytvoření požadavku</span><span class="sxs-lookup"><span data-stu-id="73688-184">The IP address of the client making the request</span></span> |
| <span data-ttu-id="73688-185">correlationId</span><span class="sxs-lookup"><span data-stu-id="73688-185">correlationId</span></span> |<span data-ttu-id="73688-186">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-186">String</span></span> |<span data-ttu-id="73688-187">Id protokolu, který můžete použít k seskupení sady položek protokolu související</span><span class="sxs-lookup"><span data-stu-id="73688-187">The id of the log that can used to group together a set of related log entries</span></span> |
| <span data-ttu-id="73688-188">identity</span><span class="sxs-lookup"><span data-stu-id="73688-188">identity</span></span> |<span data-ttu-id="73688-189">Objekt</span><span class="sxs-lookup"><span data-stu-id="73688-189">Object</span></span> |<span data-ttu-id="73688-190">Identity, která generuje protokol</span><span class="sxs-lookup"><span data-stu-id="73688-190">The identity that generated the log</span></span> |
| <span data-ttu-id="73688-191">properties</span><span class="sxs-lookup"><span data-stu-id="73688-191">properties</span></span> |<span data-ttu-id="73688-192">JSON</span><span class="sxs-lookup"><span data-stu-id="73688-192">JSON</span></span> |<span data-ttu-id="73688-193">Níže naleznete podrobnosti</span><span class="sxs-lookup"><span data-stu-id="73688-193">See below for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="73688-194">Schéma vlastnosti požadavku protokolu</span><span class="sxs-lookup"><span data-stu-id="73688-194">Request log properties schema</span></span>
| <span data-ttu-id="73688-195">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="73688-195">Name</span></span> | <span data-ttu-id="73688-196">Typ</span><span class="sxs-lookup"><span data-stu-id="73688-196">Type</span></span> | <span data-ttu-id="73688-197">Popis</span><span class="sxs-lookup"><span data-stu-id="73688-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73688-198">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="73688-198">HttpMethod</span></span> |<span data-ttu-id="73688-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-199">String</span></span> |<span data-ttu-id="73688-200">Metoda HTTP se používá pro operaci.</span><span class="sxs-lookup"><span data-stu-id="73688-200">The HTTP Method used for the operation.</span></span> <span data-ttu-id="73688-201">Například získáte.</span><span class="sxs-lookup"><span data-stu-id="73688-201">For example, GET.</span></span> |
| <span data-ttu-id="73688-202">Cesta</span><span class="sxs-lookup"><span data-stu-id="73688-202">Path</span></span> |<span data-ttu-id="73688-203">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-203">String</span></span> |<span data-ttu-id="73688-204">Cesta k operaci byla provedena na</span><span class="sxs-lookup"><span data-stu-id="73688-204">The path the operation was performed on</span></span> |
| <span data-ttu-id="73688-205">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="73688-205">RequestContentLength</span></span> |<span data-ttu-id="73688-206">celá čísla</span><span class="sxs-lookup"><span data-stu-id="73688-206">int</span></span> |<span data-ttu-id="73688-207">Délka obsahu požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="73688-207">The content length of the HTTP request</span></span> |
| <span data-ttu-id="73688-208">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="73688-208">ClientRequestId</span></span> |<span data-ttu-id="73688-209">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-209">String</span></span> |<span data-ttu-id="73688-210">Identifikátor, který jedinečně identifikuje tuto žádost</span><span class="sxs-lookup"><span data-stu-id="73688-210">The Id that uniquely identifies this request</span></span> |
| <span data-ttu-id="73688-211">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="73688-211">StartTime</span></span> |<span data-ttu-id="73688-212">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-212">String</span></span> |<span data-ttu-id="73688-213">Čas, na které server přijal žádost</span><span class="sxs-lookup"><span data-stu-id="73688-213">The time at which the server received the request</span></span> |
| <span data-ttu-id="73688-214">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="73688-214">EndTime</span></span> |<span data-ttu-id="73688-215">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-215">String</span></span> |<span data-ttu-id="73688-216">Čas, kdy server odeslal odpověď</span><span class="sxs-lookup"><span data-stu-id="73688-216">The time at which the server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="73688-217">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="73688-217">Audit logs</span></span>
<span data-ttu-id="73688-218">Zde je vzorového vstupu v protokolu auditování formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="73688-218">Here's a sample entry in the JSON-formatted audit log.</span></span> <span data-ttu-id="73688-219">Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu</span><span class="sxs-lookup"><span data-stu-id="73688-219">Each blob has one root object called **records** that contains an array of log objects</span></span>

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a><span data-ttu-id="73688-220">Schéma protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="73688-220">Audit log schema</span></span>
| <span data-ttu-id="73688-221">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="73688-221">Name</span></span> | <span data-ttu-id="73688-222">Typ</span><span class="sxs-lookup"><span data-stu-id="73688-222">Type</span></span> | <span data-ttu-id="73688-223">Popis</span><span class="sxs-lookup"><span data-stu-id="73688-223">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73688-224">time</span><span class="sxs-lookup"><span data-stu-id="73688-224">time</span></span> |<span data-ttu-id="73688-225">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-225">String</span></span> |<span data-ttu-id="73688-226">Časové razítko (ve formátu UTC) v protokolu</span><span class="sxs-lookup"><span data-stu-id="73688-226">The timestamp (in UTC) of the log</span></span> |
| <span data-ttu-id="73688-227">resourceId</span><span class="sxs-lookup"><span data-stu-id="73688-227">resourceId</span></span> |<span data-ttu-id="73688-228">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-228">String</span></span> |<span data-ttu-id="73688-229">ID prostředku, který operace trvalo umístit na</span><span class="sxs-lookup"><span data-stu-id="73688-229">The ID of the resource that operation took place on</span></span> |
| <span data-ttu-id="73688-230">category</span><span class="sxs-lookup"><span data-stu-id="73688-230">category</span></span> |<span data-ttu-id="73688-231">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-231">String</span></span> |<span data-ttu-id="73688-232">Kategorie protokolu.</span><span class="sxs-lookup"><span data-stu-id="73688-232">The log category.</span></span> <span data-ttu-id="73688-233">Například **auditu**.</span><span class="sxs-lookup"><span data-stu-id="73688-233">For example, **Audit**.</span></span> |
| <span data-ttu-id="73688-234">operationName</span><span class="sxs-lookup"><span data-stu-id="73688-234">operationName</span></span> |<span data-ttu-id="73688-235">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-235">String</span></span> |<span data-ttu-id="73688-236">Název operace, která je zaznamenána.</span><span class="sxs-lookup"><span data-stu-id="73688-236">Name of the operation that is logged.</span></span> <span data-ttu-id="73688-237">Například getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="73688-237">For example, getfilestatus.</span></span> |
| <span data-ttu-id="73688-238">resultType</span><span class="sxs-lookup"><span data-stu-id="73688-238">resultType</span></span> |<span data-ttu-id="73688-239">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-239">String</span></span> |<span data-ttu-id="73688-240">Stav operace, například 200.</span><span class="sxs-lookup"><span data-stu-id="73688-240">The status of the operation, For example, 200.</span></span> |
| <span data-ttu-id="73688-241">correlationId</span><span class="sxs-lookup"><span data-stu-id="73688-241">correlationId</span></span> |<span data-ttu-id="73688-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-242">String</span></span> |<span data-ttu-id="73688-243">Id protokolu, který můžete použít k seskupení sady položek protokolu související</span><span class="sxs-lookup"><span data-stu-id="73688-243">The id of the log that can used to group together a set of related log entries</span></span> |
| <span data-ttu-id="73688-244">identity</span><span class="sxs-lookup"><span data-stu-id="73688-244">identity</span></span> |<span data-ttu-id="73688-245">Objekt</span><span class="sxs-lookup"><span data-stu-id="73688-245">Object</span></span> |<span data-ttu-id="73688-246">Identity, která generuje protokol</span><span class="sxs-lookup"><span data-stu-id="73688-246">The identity that generated the log</span></span> |
| <span data-ttu-id="73688-247">properties</span><span class="sxs-lookup"><span data-stu-id="73688-247">properties</span></span> |<span data-ttu-id="73688-248">JSON</span><span class="sxs-lookup"><span data-stu-id="73688-248">JSON</span></span> |<span data-ttu-id="73688-249">Níže naleznete podrobnosti</span><span class="sxs-lookup"><span data-stu-id="73688-249">See below for details</span></span> |

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="73688-250">Schéma vlastnosti protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="73688-250">Audit log properties schema</span></span>
| <span data-ttu-id="73688-251">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="73688-251">Name</span></span> | <span data-ttu-id="73688-252">Typ</span><span class="sxs-lookup"><span data-stu-id="73688-252">Type</span></span> | <span data-ttu-id="73688-253">Popis</span><span class="sxs-lookup"><span data-stu-id="73688-253">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73688-254">StreamName</span><span class="sxs-lookup"><span data-stu-id="73688-254">StreamName</span></span> |<span data-ttu-id="73688-255">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73688-255">String</span></span> |<span data-ttu-id="73688-256">Cesta k operaci byla provedena na</span><span class="sxs-lookup"><span data-stu-id="73688-256">The path the operation was performed on</span></span> |

## <a name="samples-to-process-the-log-data"></a><span data-ttu-id="73688-257">Ukázky ke zpracování dat protokolu</span><span class="sxs-lookup"><span data-stu-id="73688-257">Samples to process the log data</span></span>
<span data-ttu-id="73688-258">Azure Data Lake Store poskytuje vzorku o tom, jak zpracovávat a analyzovat data protokolu.</span><span class="sxs-lookup"><span data-stu-id="73688-258">Azure Data Lake Store provides a sample on how to process and analyze the log data.</span></span> <span data-ttu-id="73688-259">Můžete najít ukázku najdete na adrese [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="73688-259">You can find the sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span> 

## <a name="see-also"></a><span data-ttu-id="73688-260">Viz také</span><span class="sxs-lookup"><span data-stu-id="73688-260">See also</span></span>
* [<span data-ttu-id="73688-261">Přehled Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73688-261">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="73688-262">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73688-262">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)

