---
title: "diagnostické protokoly aaaViewing pro Azure Data Lake Store | Microsoft Docs"
description: "Pochopit, jak toosetup a přístupu k diagnostickým protokolům pro Azure Data Lake Store "
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
ms.openlocfilehash: 11fbf7f517f97abdcaf809c1ebeeb51424ab2c1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a><span data-ttu-id="61926-103">Přístup k diagnostickým protokolům pro Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="61926-103">Accessing diagnostic logs for Azure Data Lake Store</span></span>
<span data-ttu-id="61926-104">Informace o tom, jak tooenable protokolování diagnostiky pro váš účet Data Lake Store a jak tooview hello protokoly shromážděné pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="61926-104">Learn about how tooenable diagnostic logging for your Data Lake Store account and how tooview hello logs collected for your account.</span></span>

<span data-ttu-id="61926-105">Organizace může povolit protokolování diagnostiky pro jejich Azure Data Lake Store účet toocollect data přístup kontrolní záznamy, které poskytuje informace, jako je seznam uživatele, kteří používají hello data, jak často se hello data používají, kolik dat je uložen v hello účet, atd.</span><span class="sxs-lookup"><span data-stu-id="61926-105">Organizations can enable diagnostic logging for their Azure Data Lake Store account toocollect data access audit trails that provides information such as list of users accessing hello data, how frequently hello data is accessed, how much data is stored in hello account, etc.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61926-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="61926-106">Prerequisites</span></span>
* <span data-ttu-id="61926-107">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="61926-107">**An Azure subscription**.</span></span> <span data-ttu-id="61926-108">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="61926-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="61926-109">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="61926-109">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="61926-110">Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="61926-110">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span>

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a><span data-ttu-id="61926-111">Povolit protokolování diagnostiky pro váš účet Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="61926-111">Enable diagnostic logging for your Data Lake Store account</span></span>
1. <span data-ttu-id="61926-112">Přihlášení toohello nové [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="61926-112">Sign on toohello new [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="61926-113">Otevřete účet Data Lake Store a z vaší okně účtu Data Lake Store klikněte na tlačítko **nastavení**a potom klikněte na **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="61926-113">Open your Data Lake Store account, and from your Data Lake Store account blade, click **Settings**, and then click **Diagnostic logs**.</span></span>
3. <span data-ttu-id="61926-114">V hello **protokolů diagnostiky** okně klikněte na tlačítko **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="61926-114">In hello **Diagnostics logs** blade, click **Turn on diagnostics**.</span></span>

    <span data-ttu-id="61926-115">![Povolit protokolování diagnostiky](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "povolení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="61926-115">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "Enable diagnostic logs")</span></span>

3. <span data-ttu-id="61926-116">V hello **diagnostiky** okno, proveďte následující změny protokolování diagnostiky tooconfigure hello.</span><span class="sxs-lookup"><span data-stu-id="61926-116">In hello **Diagnostic** blade, make hello following changes tooconfigure diagnostic logging.</span></span>
   
    <span data-ttu-id="61926-117">![Povolit protokolování diagnostiky](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "povolení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="61926-117">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>
   
   * <span data-ttu-id="61926-118">Nastavit **stav** příliš**na** tooenable protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="61926-118">Set **Status** too**On** tooenable diagnostic logging.</span></span>
   * <span data-ttu-id="61926-119">Můžete data toostore/procesu hello různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="61926-119">You can choose toostore/process hello data in different ways.</span></span>
     
        * <span data-ttu-id="61926-120">Vyberte možnost hello příliš**archivu účet úložiště tooa** toostore protokoly tooan účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="61926-120">Select hello option too**Archive tooa storage account** toostore logs tooan Azure Storage account.</span></span> <span data-ttu-id="61926-121">Tuto možnost použijte, pokud chcete tooarchive hello data, která bude zpracování dávky později.</span><span class="sxs-lookup"><span data-stu-id="61926-121">You use this option if you want tooarchive hello data that will be batch-processed at a later date.</span></span> <span data-ttu-id="61926-122">Pokud vyberete tuto možnost je nutné zadat účet služby Azure Storage toosave hello protokolů.</span><span class="sxs-lookup"><span data-stu-id="61926-122">If you select this option you must provide an Azure Storage account toosave hello logs to.</span></span>
        
        * <span data-ttu-id="61926-123">Vyberte možnost hello příliš**centra událostí tooan datového proudu** toostream protokolu data tooan centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="61926-123">Select hello option too**Stream tooan event hub** toostream log data tooan Azure Event Hub.</span></span> <span data-ttu-id="61926-124">S největší pravděpodobností použijete tuto možnost, pokud mají podřízené zpracování kanálu tooanalyze příchozí protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="61926-124">Most likely you will use this option if you have a downstream processing pipeline tooanalyze incoming logs at real time.</span></span> <span data-ttu-id="61926-125">Pokud vyberete tuto možnost, je nutné zadat hello podrobnosti pro hello chcete toouse centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="61926-125">If you select this option, you must provide hello details for hello Azure Event Hub you want toouse.</span></span>

        * <span data-ttu-id="61926-126">Vyberte možnost hello příliš**odeslat tooLog Analytics** toouse hello data protokolu hello generované tooanalyze služba Analýza protokolů Azure.</span><span class="sxs-lookup"><span data-stu-id="61926-126">Select hello option too**Send tooLog Analytics** toouse hello Azure Log Analytics service tooanalyze hello generated log data.</span></span> <span data-ttu-id="61926-127">Pokud vyberete tuto možnost, je nutné zadat, že hello podrobnosti o hello prostoru služby Operations Management Suite, které by použití hello provádět analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="61926-127">If you select this option, you must provide hello details for hello Operations Management Suite workspace that you would use hello perform log analysis.</span></span>
     
   * <span data-ttu-id="61926-128">Zadejte, zda chcete protokoly auditu tooget nebo protokoly požadavku nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="61926-128">Specify whether you want tooget audit logs or request logs or both.</span></span>
   * <span data-ttu-id="61926-129">Zadejte hello počet dnů, pro které se musí uchovávat hello data.</span><span class="sxs-lookup"><span data-stu-id="61926-129">Specify hello number of days for which hello data must be retained.</span></span> <span data-ttu-id="61926-130">Uchování platí pouze pokud používáte data protokolu tooarchive účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="61926-130">Retention is only applicable if you are using Azure storage account tooarchive log data.</span></span>
   * <span data-ttu-id="61926-131">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="61926-131">Click **Save**.</span></span>

<span data-ttu-id="61926-132">Jakmile povolíte nastavení diagnostiky, můžete sledovat hello přihlásí hello **diagnostické protokoly** kartě.</span><span class="sxs-lookup"><span data-stu-id="61926-132">Once you have enabled diagnostic settings, you can watch hello logs in hello **Diagnostic Logs** tab.</span></span>

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a><span data-ttu-id="61926-133">Zobrazení diagnostických protokolů pro účet Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="61926-133">View diagnostic logs for your Data Lake Store account</span></span>
<span data-ttu-id="61926-134">Existují dva způsoby tooview hello data protokolu pro váš účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="61926-134">There are two ways tooview hello log data for your Data Lake Store account.</span></span>

* <span data-ttu-id="61926-135">Z účtu Data Lake Store hello nastavení zobrazení</span><span class="sxs-lookup"><span data-stu-id="61926-135">From hello Data Lake Store account settings view</span></span>
* <span data-ttu-id="61926-136">Z účtu úložiště Azure hello ukládat hello data</span><span class="sxs-lookup"><span data-stu-id="61926-136">From hello Azure Storage account where hello data is stored</span></span>

### <a name="using-hello-data-lake-store-settings-view"></a><span data-ttu-id="61926-137">Pomocí hello zobrazit Data Lake Store nastavení</span><span class="sxs-lookup"><span data-stu-id="61926-137">Using hello Data Lake Store Settings view</span></span>
1. <span data-ttu-id="61926-138">Z účtu Data Lake Store **nastavení** okně klikněte na tlačítko **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="61926-138">From your Data Lake Store account **Settings** blade, click **Diagnostic Logs**.</span></span>
   
    <span data-ttu-id="61926-139">![Protokolování diagnostiky zobrazení](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="61926-139">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span> 
2. <span data-ttu-id="61926-140">V hello **diagnostické protokoly** okno, měli byste vidět hello protokoly zařazený do kategorie službou **protokoly auditu** a **požadavku protokoly**.</span><span class="sxs-lookup"><span data-stu-id="61926-140">In hello **Diagnostic Logs** blade, you should see hello logs categorized by **Audit Logs** and **Request Logs**.</span></span>
   
   * <span data-ttu-id="61926-141">Protokoly žádosti o zachycení každý API požadavek na hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="61926-141">Request logs capture every API request made on hello Data Lake Store account.</span></span>
   * <span data-ttu-id="61926-142">Protokoly auditu jsou podobné toorequest protokoly, ale poskytují mnohem podrobnější rozpis hello operací provádí na účtu Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="61926-142">Audit Logs are similar toorequest Logs but provide a much more detailed breakdown of hello operations being performed on hello Data Lake Store account.</span></span> <span data-ttu-id="61926-143">Například volání rozhraní API jednoho uložení v protokolech žádost může způsobit více operací "Připojit" v protokolech auditu hello.</span><span class="sxs-lookup"><span data-stu-id="61926-143">For example, a single upload API call in request logs might result in multiple "Append" operations in hello audit logs.</span></span>
3. <span data-ttu-id="61926-144">Klikněte na tlačítko hello **Stáhnout** odkaz proti jednotlivým protokol položka toodownload hello protokoly.</span><span class="sxs-lookup"><span data-stu-id="61926-144">Click hello **Download** link against each log entry toodownload hello logs.</span></span>

### <a name="from-hello-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="61926-145">Z hello účet služby Azure Storage, který obsahuje data protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-145">From hello Azure Storage account that contains log data</span></span>
1. <span data-ttu-id="61926-146">Otevřete okno účtu Azure Storage hello spojené s Data Lake Store pro protokolování a pak klikněte na objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="61926-146">Open hello Azure Storage account blade associated with Data Lake Store for logging, and then click Blobs.</span></span> <span data-ttu-id="61926-147">Hello **služba objektů Blob** okno uvádí dva kontejnery.</span><span class="sxs-lookup"><span data-stu-id="61926-147">hello **Blob service** blade lists two containers.</span></span>
   
    <span data-ttu-id="61926-148">![Protokolování diagnostiky zobrazení](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="61926-148">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>
   
   * <span data-ttu-id="61926-149">kontejner Hello **přehledy. protokoly auditu** obsahuje hello protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="61926-149">hello container **insights-logs-audit** contains hello audit logs.</span></span>
   * <span data-ttu-id="61926-150">kontejner Hello **přehledy. protokoly žádosti** obsahuje hello požadavek protokoly.</span><span class="sxs-lookup"><span data-stu-id="61926-150">hello container **insights-logs-requests** contains hello request logs.</span></span>
2. <span data-ttu-id="61926-151">V rámci těchto kontejnerů hello protokoly jsou uloženy v rámci hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="61926-151">Within these containers, hello logs are stored under hello following structure.</span></span>
   
    <span data-ttu-id="61926-152">![Protokolování diagnostiky zobrazení](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "zobrazení diagnostických protokolů")</span><span class="sxs-lookup"><span data-stu-id="61926-152">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "View diagnostic logs")</span></span>
   
    <span data-ttu-id="61926-153">Jako příklad může být protokolu auditování tooan kompletní cesta hello`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="61926-153">As an example, hello complete path tooan audit log could be `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span></span>
   
    <span data-ttu-id="61926-154">Obdobně, může být hello kompletní cesta tooa žádost protokolu`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="61926-154">Similary, hello complete path tooa request log could be `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span></span>

## <a name="understand-hello-structure-of-hello-log-data"></a><span data-ttu-id="61926-155">Pochopení hello struktura dat protokolu hello</span><span class="sxs-lookup"><span data-stu-id="61926-155">Understand hello structure of hello log data</span></span>
<span data-ttu-id="61926-156">Hello protokoly auditu a požadavku jsou ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="61926-156">hello audit and request logs are in a JSON format.</span></span> <span data-ttu-id="61926-157">V této části jsme podívejte se na hello struktura JSON pro požadavek a protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="61926-157">In this section, we look at hello structure of JSON for request and audit logs.</span></span>

### <a name="request-logs"></a><span data-ttu-id="61926-158">Žádost o protokoly</span><span class="sxs-lookup"><span data-stu-id="61926-158">Request logs</span></span>
<span data-ttu-id="61926-159">Zde je vzorového vstupu v protokolu hello požadavků formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="61926-159">Here's a sample entry in hello JSON-formatted request log.</span></span> <span data-ttu-id="61926-160">Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.</span><span class="sxs-lookup"><span data-stu-id="61926-160">Each blob has one root object called **records** that contains an array of log objects.</span></span>

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

#### <a name="request-log-schema"></a><span data-ttu-id="61926-161">Schéma požadavku protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-161">Request log schema</span></span>
| <span data-ttu-id="61926-162">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="61926-162">Name</span></span> | <span data-ttu-id="61926-163">Typ</span><span class="sxs-lookup"><span data-stu-id="61926-163">Type</span></span> | <span data-ttu-id="61926-164">Popis</span><span class="sxs-lookup"><span data-stu-id="61926-164">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="61926-165">time</span><span class="sxs-lookup"><span data-stu-id="61926-165">time</span></span> |<span data-ttu-id="61926-166">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-166">String</span></span> |<span data-ttu-id="61926-167">časové razítko Hello (ve formátu UTC) hello protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-167">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="61926-168">resourceId</span><span class="sxs-lookup"><span data-stu-id="61926-168">resourceId</span></span> |<span data-ttu-id="61926-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-169">String</span></span> |<span data-ttu-id="61926-170">ID Hello hello prostředku, který operace trvalo umístit na</span><span class="sxs-lookup"><span data-stu-id="61926-170">hello ID of hello resource that operation took place on</span></span> |
| <span data-ttu-id="61926-171">category</span><span class="sxs-lookup"><span data-stu-id="61926-171">category</span></span> |<span data-ttu-id="61926-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-172">String</span></span> |<span data-ttu-id="61926-173">kategorie Hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="61926-173">hello log category.</span></span> <span data-ttu-id="61926-174">Například **požadavky**.</span><span class="sxs-lookup"><span data-stu-id="61926-174">For example, **Requests**.</span></span> |
| <span data-ttu-id="61926-175">operationName</span><span class="sxs-lookup"><span data-stu-id="61926-175">operationName</span></span> |<span data-ttu-id="61926-176">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-176">String</span></span> |<span data-ttu-id="61926-177">Název hello operace, která je zaznamenána.</span><span class="sxs-lookup"><span data-stu-id="61926-177">Name of hello operation that is logged.</span></span> <span data-ttu-id="61926-178">Například getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="61926-178">For example, getfilestatus.</span></span> |
| <span data-ttu-id="61926-179">resultType</span><span class="sxs-lookup"><span data-stu-id="61926-179">resultType</span></span> |<span data-ttu-id="61926-180">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-180">String</span></span> |<span data-ttu-id="61926-181">Stav Hello hello operace, například 200.</span><span class="sxs-lookup"><span data-stu-id="61926-181">hello status of hello operation, For example, 200.</span></span> |
| <span data-ttu-id="61926-182">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="61926-182">callerIpAddress</span></span> |<span data-ttu-id="61926-183">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-183">String</span></span> |<span data-ttu-id="61926-184">Hello IP adresa klienta hello vytváření hello požadavku</span><span class="sxs-lookup"><span data-stu-id="61926-184">hello IP address of hello client making hello request</span></span> |
| <span data-ttu-id="61926-185">correlationId</span><span class="sxs-lookup"><span data-stu-id="61926-185">correlationId</span></span> |<span data-ttu-id="61926-186">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-186">String</span></span> |<span data-ttu-id="61926-187">id Hello hello protokolu, který můžete použít toogroup společně sada položek související protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-187">hello id of hello log that can used toogroup together a set of related log entries</span></span> |
| <span data-ttu-id="61926-188">identity</span><span class="sxs-lookup"><span data-stu-id="61926-188">identity</span></span> |<span data-ttu-id="61926-189">Objekt</span><span class="sxs-lookup"><span data-stu-id="61926-189">Object</span></span> |<span data-ttu-id="61926-190">Hello identity, která vygenerovala hello protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-190">hello identity that generated hello log</span></span> |
| <span data-ttu-id="61926-191">properties</span><span class="sxs-lookup"><span data-stu-id="61926-191">properties</span></span> |<span data-ttu-id="61926-192">JSON</span><span class="sxs-lookup"><span data-stu-id="61926-192">JSON</span></span> |<span data-ttu-id="61926-193">Níže naleznete podrobnosti</span><span class="sxs-lookup"><span data-stu-id="61926-193">See below for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="61926-194">Schéma vlastnosti požadavku protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-194">Request log properties schema</span></span>
| <span data-ttu-id="61926-195">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="61926-195">Name</span></span> | <span data-ttu-id="61926-196">Typ</span><span class="sxs-lookup"><span data-stu-id="61926-196">Type</span></span> | <span data-ttu-id="61926-197">Popis</span><span class="sxs-lookup"><span data-stu-id="61926-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="61926-198">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="61926-198">HttpMethod</span></span> |<span data-ttu-id="61926-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-199">String</span></span> |<span data-ttu-id="61926-200">Hello metoda HTTP pro hello operaci použít.</span><span class="sxs-lookup"><span data-stu-id="61926-200">hello HTTP Method used for hello operation.</span></span> <span data-ttu-id="61926-201">Například získáte.</span><span class="sxs-lookup"><span data-stu-id="61926-201">For example, GET.</span></span> |
| <span data-ttu-id="61926-202">Cesta</span><span class="sxs-lookup"><span data-stu-id="61926-202">Path</span></span> |<span data-ttu-id="61926-203">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-203">String</span></span> |<span data-ttu-id="61926-204">operace hello Hello cesta byla provedena na</span><span class="sxs-lookup"><span data-stu-id="61926-204">hello path hello operation was performed on</span></span> |
| <span data-ttu-id="61926-205">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="61926-205">RequestContentLength</span></span> |<span data-ttu-id="61926-206">celá čísla</span><span class="sxs-lookup"><span data-stu-id="61926-206">int</span></span> |<span data-ttu-id="61926-207">Délka obsahu Hello hello HTTP požadavku</span><span class="sxs-lookup"><span data-stu-id="61926-207">hello content length of hello HTTP request</span></span> |
| <span data-ttu-id="61926-208">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="61926-208">ClientRequestId</span></span> |<span data-ttu-id="61926-209">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-209">String</span></span> |<span data-ttu-id="61926-210">Hello Id, který jedinečně identifikuje tuto žádost</span><span class="sxs-lookup"><span data-stu-id="61926-210">hello Id that uniquely identifies this request</span></span> |
| <span data-ttu-id="61926-211">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="61926-211">StartTime</span></span> |<span data-ttu-id="61926-212">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-212">String</span></span> |<span data-ttu-id="61926-213">Hello čas, který hello server obdržel hello požadavku</span><span class="sxs-lookup"><span data-stu-id="61926-213">hello time at which hello server received hello request</span></span> |
| <span data-ttu-id="61926-214">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="61926-214">EndTime</span></span> |<span data-ttu-id="61926-215">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-215">String</span></span> |<span data-ttu-id="61926-216">Hello čas, který hello serveru odeslání odpovědi</span><span class="sxs-lookup"><span data-stu-id="61926-216">hello time at which hello server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="61926-217">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="61926-217">Audit logs</span></span>
<span data-ttu-id="61926-218">Zde je vstup vzorového ve formátu JSON auditní protokol hello.</span><span class="sxs-lookup"><span data-stu-id="61926-218">Here's a sample entry in hello JSON-formatted audit log.</span></span> <span data-ttu-id="61926-219">Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-219">Each blob has one root object called **records** that contains an array of log objects</span></span>

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

#### <a name="audit-log-schema"></a><span data-ttu-id="61926-220">Schéma protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="61926-220">Audit log schema</span></span>
| <span data-ttu-id="61926-221">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="61926-221">Name</span></span> | <span data-ttu-id="61926-222">Typ</span><span class="sxs-lookup"><span data-stu-id="61926-222">Type</span></span> | <span data-ttu-id="61926-223">Popis</span><span class="sxs-lookup"><span data-stu-id="61926-223">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="61926-224">time</span><span class="sxs-lookup"><span data-stu-id="61926-224">time</span></span> |<span data-ttu-id="61926-225">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-225">String</span></span> |<span data-ttu-id="61926-226">časové razítko Hello (ve formátu UTC) hello protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-226">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="61926-227">resourceId</span><span class="sxs-lookup"><span data-stu-id="61926-227">resourceId</span></span> |<span data-ttu-id="61926-228">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-228">String</span></span> |<span data-ttu-id="61926-229">ID Hello hello prostředku, který operace trvalo umístit na</span><span class="sxs-lookup"><span data-stu-id="61926-229">hello ID of hello resource that operation took place on</span></span> |
| <span data-ttu-id="61926-230">category</span><span class="sxs-lookup"><span data-stu-id="61926-230">category</span></span> |<span data-ttu-id="61926-231">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-231">String</span></span> |<span data-ttu-id="61926-232">kategorie Hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="61926-232">hello log category.</span></span> <span data-ttu-id="61926-233">Například **auditu**.</span><span class="sxs-lookup"><span data-stu-id="61926-233">For example, **Audit**.</span></span> |
| <span data-ttu-id="61926-234">operationName</span><span class="sxs-lookup"><span data-stu-id="61926-234">operationName</span></span> |<span data-ttu-id="61926-235">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-235">String</span></span> |<span data-ttu-id="61926-236">Název hello operace, která je zaznamenána.</span><span class="sxs-lookup"><span data-stu-id="61926-236">Name of hello operation that is logged.</span></span> <span data-ttu-id="61926-237">Například getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="61926-237">For example, getfilestatus.</span></span> |
| <span data-ttu-id="61926-238">resultType</span><span class="sxs-lookup"><span data-stu-id="61926-238">resultType</span></span> |<span data-ttu-id="61926-239">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-239">String</span></span> |<span data-ttu-id="61926-240">Stav Hello hello operace, například 200.</span><span class="sxs-lookup"><span data-stu-id="61926-240">hello status of hello operation, For example, 200.</span></span> |
| <span data-ttu-id="61926-241">correlationId</span><span class="sxs-lookup"><span data-stu-id="61926-241">correlationId</span></span> |<span data-ttu-id="61926-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-242">String</span></span> |<span data-ttu-id="61926-243">id Hello hello protokolu, který můžete použít toogroup společně sada položek související protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-243">hello id of hello log that can used toogroup together a set of related log entries</span></span> |
| <span data-ttu-id="61926-244">identity</span><span class="sxs-lookup"><span data-stu-id="61926-244">identity</span></span> |<span data-ttu-id="61926-245">Objekt</span><span class="sxs-lookup"><span data-stu-id="61926-245">Object</span></span> |<span data-ttu-id="61926-246">Hello identity, která vygenerovala hello protokolu</span><span class="sxs-lookup"><span data-stu-id="61926-246">hello identity that generated hello log</span></span> |
| <span data-ttu-id="61926-247">properties</span><span class="sxs-lookup"><span data-stu-id="61926-247">properties</span></span> |<span data-ttu-id="61926-248">JSON</span><span class="sxs-lookup"><span data-stu-id="61926-248">JSON</span></span> |<span data-ttu-id="61926-249">Níže naleznete podrobnosti</span><span class="sxs-lookup"><span data-stu-id="61926-249">See below for details</span></span> |

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="61926-250">Schéma vlastnosti protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="61926-250">Audit log properties schema</span></span>
| <span data-ttu-id="61926-251">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="61926-251">Name</span></span> | <span data-ttu-id="61926-252">Typ</span><span class="sxs-lookup"><span data-stu-id="61926-252">Type</span></span> | <span data-ttu-id="61926-253">Popis</span><span class="sxs-lookup"><span data-stu-id="61926-253">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="61926-254">StreamName</span><span class="sxs-lookup"><span data-stu-id="61926-254">StreamName</span></span> |<span data-ttu-id="61926-255">Řetězec</span><span class="sxs-lookup"><span data-stu-id="61926-255">String</span></span> |<span data-ttu-id="61926-256">operace hello Hello cesta byla provedena na</span><span class="sxs-lookup"><span data-stu-id="61926-256">hello path hello operation was performed on</span></span> |

## <a name="samples-tooprocess-hello-log-data"></a><span data-ttu-id="61926-257">Data protokolu hello tooprocess ukázky</span><span class="sxs-lookup"><span data-stu-id="61926-257">Samples tooprocess hello log data</span></span>
<span data-ttu-id="61926-258">Azure Data Lake Store obsahuje ukázku tooprocess a analýze dat protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="61926-258">Azure Data Lake Store provides a sample on how tooprocess and analyze hello log data.</span></span> <span data-ttu-id="61926-259">Můžete najít hello ukázku najdete na adrese [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="61926-259">You can find hello sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span> 

## <a name="see-also"></a><span data-ttu-id="61926-260">Viz také</span><span class="sxs-lookup"><span data-stu-id="61926-260">See also</span></span>
* [<span data-ttu-id="61926-261">Přehled Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="61926-261">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="61926-262">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="61926-262">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)

