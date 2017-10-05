---
title: "Úvod do služby Stream Analytics | Dokumentace Microsoftu"
description: "Přečtěte si o službě Stream Analytics, která umožňuje v reálném čase analyzovat data streamovaná z platformy Internet věcí (IOT)."
keywords: "analytics jako služba, spravované služby, zpracování datového proudu, streamování analytics, co je datový proud analytics"
services: stream-analytics
documentationcenter: 
author: jenniehubbard
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/08/2017
ms.author: jhubbard
ms.openlocfilehash: daaec2b986af8b3f2fc020e01d8fb0f47ffb2df0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="what-is-stream-analytics"></a><span data-ttu-id="ff264-104">Co je služba Stream Analytics?</span><span class="sxs-lookup"><span data-stu-id="ff264-104">What is Stream Analytics?</span></span>

<span data-ttu-id="ff264-105">Azure Stream Analytics je plně spravovaný modul na zpracování událostí, který umožňuje nastavit a provádět analytické výpočty streamovaných dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="ff264-105">Azure Stream Analytics is a fully managed event-processing engine that lets you set up real-time analytic computations on streaming data.</span></span> <span data-ttu-id="ff264-106">Data můžou pocházet ze zařízení, senzorů, webů, informačních kanálů sociálních médií, aplikací, systémů infrastruktury a dalších zdrojů.</span><span class="sxs-lookup"><span data-stu-id="ff264-106">The data can come from devices, sensors, web sites, social media feeds, applications, infrastructure systems, and more.</span></span> 

## <a name="what-can-i-do-with-stream-analytics"></a><span data-ttu-id="ff264-107">Co se dá se službou Stream Analytics dělat?</span><span class="sxs-lookup"><span data-stu-id="ff264-107">What can I do with Stream Analytics?</span></span>

<span data-ttu-id="ff264-108">Pomocí Stream Analytics můžete zkoumat velké objemy dat odesílaných ze zařízení nebo procesů, extrahovat informace z datových proudů a vyhledávat vzory, trendy a vztahy.</span><span class="sxs-lookup"><span data-stu-id="ff264-108">Use Stream Analytics to examine high volumes of data flowing from devices or processes, extract information from the data stream, and look for patterns, trends, and relationships.</span></span> <span data-ttu-id="ff264-109">Podle povahy zkoumaných dat pak můžete provádět různé úlohy aplikací.</span><span class="sxs-lookup"><span data-stu-id="ff264-109">Based on what's in the data, you can then perform application tasks.</span></span> <span data-ttu-id="ff264-110">Můžete například vyvolávat výstrahy, spouštět automatizované pracovní postupy, odesílat informace do nástrojů na vytváření sestav, jako je Power BI, nebo ukládat data pro pozdější analýzu.</span><span class="sxs-lookup"><span data-stu-id="ff264-110">For example, you might raise alerts, kick off automation workflows, feed information to a reporting tool such as Power BI, or store data for later investigation.</span></span> 

<span data-ttu-id="ff264-111">Příklady:</span><span class="sxs-lookup"><span data-stu-id="ff264-111">Examples:</span></span>

* <span data-ttu-id="ff264-112">Personalizované analýzy obchodování na burze a výstrahy v reálném čase nabízené finančními institucemi</span><span class="sxs-lookup"><span data-stu-id="ff264-112">Personalized, real-time stock-trading analysis and alerts offered by financial services companies.</span></span>
* <span data-ttu-id="ff264-113">Odhalování podvodů v reálném čase prověřováním dat transakcí</span><span class="sxs-lookup"><span data-stu-id="ff264-113">Real-time fraud detection based on examining transaction data.</span></span> 
* <span data-ttu-id="ff264-114">Služby ochrany dat a identity</span><span class="sxs-lookup"><span data-stu-id="ff264-114">Data and identity protection services.</span></span>
* <span data-ttu-id="ff264-115">Analýza dat generovaných senzory a poháněcími zařízeními ve fyzických objektech (Internet věcí nebo IoT)</span><span class="sxs-lookup"><span data-stu-id="ff264-115">Analysis of data generated by sensors and actuators embedded in physical objects (Internet of Things, or IoT).</span></span>
* <span data-ttu-id="ff264-116">Analýza navštívených webových stránek</span><span class="sxs-lookup"><span data-stu-id="ff264-116">Web clickstream analytics.</span></span>
* <span data-ttu-id="ff264-117">Aplikace řízení vztahů se zákazníky (CRM), například vydání výstrahy, když se v určitém časovém rámci sníží kvalita zákaznických služeb</span><span class="sxs-lookup"><span data-stu-id="ff264-117">Customer relationship management (CRM) applications, such as issuing alerts when customer experience within a time frame is degraded.</span></span>

## <a name="how-does-stream-analytics-work"></a><span data-ttu-id="ff264-118">Jak funguje Stream Analytics?</span><span class="sxs-lookup"><span data-stu-id="ff264-118">How does Stream Analytics work?</span></span>

<span data-ttu-id="ff264-119">Tento diagram znázorňuje kanál Stream Analytics a ukazuje, jak jsou data ingestována, analyzována a následně odesílána k prezentování nebo provedení akce.</span><span class="sxs-lookup"><span data-stu-id="ff264-119">This diagram illustrates the Stream Analytics pipeline, showing how data is ingested, analyzed, and then sent for presentation or action.</span></span> 

![Kanál Stream Analytics](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

<span data-ttu-id="ff264-121">Stream Analytics začíná zdrojem streamovaných dat.</span><span class="sxs-lookup"><span data-stu-id="ff264-121">Stream Analytics starts with a source of streaming data.</span></span> <span data-ttu-id="ff264-122">Dat je možné ingestovat v Azure ze zařízení pomocí centra událostí Azure nebo centra IoT.</span><span class="sxs-lookup"><span data-stu-id="ff264-122">The data can be ingested into Azure from a device using an Azure event hub or IoT hub.</span></span> <span data-ttu-id="ff264-123">Data je také možné získávat z úložišť dat, například z Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ff264-123">The data can also be pulled from a data store like Azure Blob Storage.</span></span> 

<span data-ttu-id="ff264-124">Pokud chcete prozkoumat datový proud, vytvořte *úlohu* Stream Analytics, která bude určovat, odkud data pocházejí.</span><span class="sxs-lookup"><span data-stu-id="ff264-124">To examine the stream, you create a Stream Analytics *job* that specifies where the data is coming from.</span></span> <span data-ttu-id="ff264-125">Úloha také definuje *transformaci*&mdash; – to, jak se mají vyhledávat data, vzory nebo vztahy.</span><span class="sxs-lookup"><span data-stu-id="ff264-125">The job also specifies a *transformation*&mdash;how to look for data, patterns, or relationships.</span></span> <span data-ttu-id="ff264-126">Stream Analytics podporuje pro tuto úlohu dotazovací jazyk typu SQL, který umožňuje filtrovat, řadit, agregovat a spojovat streamovaná data za určité časové období.</span><span class="sxs-lookup"><span data-stu-id="ff264-126">For this task, Stream Analytics supports a SQL-like query language that lets you filter, sort, aggregate, and join streaming data over a time period.</span></span>

<span data-ttu-id="ff264-127">Úloha pak také definuje výstup, kam se mají transformovaná data odesílat.</span><span class="sxs-lookup"><span data-stu-id="ff264-127">Finally, the job specifies an output to send the transformed data to.</span></span> <span data-ttu-id="ff264-128">Tak můžete řídit, jaké akce se mají provádět v návaznosti na informace, které jste analyzovali.</span><span class="sxs-lookup"><span data-stu-id="ff264-128">This lets you control what to do in response to the information you've analyzed.</span></span> <span data-ttu-id="ff264-129">V návaznosti na analýzu můžete například:</span><span class="sxs-lookup"><span data-stu-id="ff264-129">For example, in response to analysis, you might:</span></span>

* <span data-ttu-id="ff264-130">Poslat příkaz, který změní nastavení zařízení</span><span class="sxs-lookup"><span data-stu-id="ff264-130">Send a command to change a device's settings.</span></span> 
* <span data-ttu-id="ff264-131">Poslat data do fronty monitorované procesem, který provede akci závislou na zjištěných informacích</span><span class="sxs-lookup"><span data-stu-id="ff264-131">Send data to a queue that's monitored by a process that takes action based on what it finds.</span></span> 
* <span data-ttu-id="ff264-132">Poslat data na řídicí panel Power BI za účelem generování sestav</span><span class="sxs-lookup"><span data-stu-id="ff264-132">Send data to a Power BI dashboard for reporting.</span></span>
* <span data-ttu-id="ff264-133">Poslat data do úložiště, jako je Data Lake Store, databáze SQL Serveru nebo úložiště objektů blob nebo tabulek Azure</span><span class="sxs-lookup"><span data-stu-id="ff264-133">Send data to storage like Data Lake Store, SQL Server database, or Azure Blob or Table storage.</span></span>

<span data-ttu-id="ff264-134">Probíhající úlohu můžete monitorovat a upravovat počet událostí, které zpracovává za sekundu.</span><span class="sxs-lookup"><span data-stu-id="ff264-134">You can monitor it and adjust how many events it processes per second while a job is running.</span></span> <span data-ttu-id="ff264-135">Úlohy můžou také vytvářet diagnostické protokoly k řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="ff264-135">You can also have jobs produce diagnostic logs for troubleshooting.</span></span>

## <a name="key-capabilities-and-benefits"></a><span data-ttu-id="ff264-136">Klíčové funkce a výhody</span><span class="sxs-lookup"><span data-stu-id="ff264-136">Key capabilities and benefits</span></span>

<span data-ttu-id="ff264-137">Služba Stream Analytics byla navržena jako snadno použitelný, flexibilní a ekonomický nástroj, škálovatelný na libovolnou velikost úlohy.</span><span class="sxs-lookup"><span data-stu-id="ff264-137">Stream Analytics is designed to be easy to use, flexible, scalable to any job size, and economical.</span></span>

### <a name="connectivity-to-many-inputs-and-outputs"></a><span data-ttu-id="ff264-138">Připojení k mnoha vstupům a výstupům</span><span class="sxs-lookup"><span data-stu-id="ff264-138">Connectivity to many inputs and outputs</span></span>

<span data-ttu-id="ff264-139">Služba Stream Analytics se za účelem ingestování datových proudů připojuje přímo ke službám [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) a za účelem ingestování historických dat přímo ke [službě Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts).</span><span class="sxs-lookup"><span data-stu-id="ff264-139">Stream Analytics connects directly to [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) and [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) for stream ingestion, and the [Azure Blob storage service](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts) to ingest historical data.</span></span> <span data-ttu-id="ff264-140">Když získáte data z center událostí, můžete Stream Analytics zkombinovat s dalšími zdroji dat a moduly na zpracování událostí.</span><span class="sxs-lookup"><span data-stu-id="ff264-140">If you get data from event hubs, you can combine Stream Analytics with other data sources and processing engines.</span></span>

<span data-ttu-id="ff264-141">Vstup úlohy může také zahrnovat referenční data (statická nebo pomalu se měnící data).</span><span class="sxs-lookup"><span data-stu-id="ff264-141">Job input can also include reference data (static or slow-changing data).</span></span> <span data-ttu-id="ff264-142">K těmto referenčním datům můžete připojit streamovaná data, abyste mohli provádět operace vyhledávání stejně jako u databázových dotazů.</span><span class="sxs-lookup"><span data-stu-id="ff264-142">You can join streaming data to this reference data to perform lookup operations the same way you would with database queries.</span></span>

<span data-ttu-id="ff264-143">Výstup úlohy Stream Analytics můžete směrovat mnoha směry.</span><span class="sxs-lookup"><span data-stu-id="ff264-143">Route Stream Analytics job output in many directions.</span></span> <span data-ttu-id="ff264-144">Můžete ho zapsat do úložiště, například do objektů blob nebo tabulek ve službě Azure Storage, Azure SQL Database, Azure Data Lake Store nebo Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ff264-144">You can write to storage, such as Azure Storage blobs or tables, Azure SQL DB, Azure Data Lake Stores, or Azure Cosmos DB.</span></span> <span data-ttu-id="ff264-145">Odtud můžou být data předávána k analýze dávky prostřednictvím Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ff264-145">From there, the data might go for batch analytics via Azure HDInsight.</span></span> <span data-ttu-id="ff264-146">Výstupy můžete odesílat do jiných služeb ke zpracování jiným procesem, například do center událostí, front nebo témat Sběrnice Azure.</span><span class="sxs-lookup"><span data-stu-id="ff264-146">You might send the output to another service for consumption by another process, such as event hubs, Azure Service Bus topics, or queues.</span></span> <span data-ttu-id="ff264-147">Můžete je také posílat do Power BI, pokud je chcete vizualizovat.</span><span class="sxs-lookup"><span data-stu-id="ff264-147">You might send the output to Power BI for visualization.</span></span>

### <a name="ease-of-use"></a><span data-ttu-id="ff264-148">Snadné používání</span><span class="sxs-lookup"><span data-stu-id="ff264-148">Ease of use</span></span>

<span data-ttu-id="ff264-149">K definování transformací slouží jednoduchý deklarativní [dotazovací jazyk Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx), který umožňuje vytvářet sofistikované analýzy bez znalosti programování.</span><span class="sxs-lookup"><span data-stu-id="ff264-149">To define transformations, you use a simple, declarative [Stream Analytics query language](https://msdn.microsoft.com/library/azure/dn834998.aspx) that lets you create sophisticated analyses with no programming.</span></span> <span data-ttu-id="ff264-150">Vstupem pro dotazovací jazyk jsou streamovaná data.</span><span class="sxs-lookup"><span data-stu-id="ff264-150">The query language takes streaming data as its input.</span></span> <span data-ttu-id="ff264-151">Data pak můžete filtrovat, řadit a spojovat (v datovém proudu nebo s referenčními daty), můžete agregovat hodnoty, provádět výpočty a používat geoprostorové funkce.</span><span class="sxs-lookup"><span data-stu-id="ff264-151">You can then filter and sort the data, aggregate values, perform calculations, join data (within a stream or to reference data), and use geospatial functions.</span></span> <span data-ttu-id="ff264-152">Dotazy můžete na portálu upravovat pomocí IntelliSense a kontroly syntaxe a můžete je testovat pomocí ukázkových dat, která se dají extrahovat z živého datového proudu.</span><span class="sxs-lookup"><span data-stu-id="ff264-152">You can edit queries in the portal, using IntelliSense and syntax checking, and you can test queries using sample data that you can extract from the live stream.</span></span>

### <a name="extensible-query-language"></a><span data-ttu-id="ff264-153">Rozšiřitelný dotazovací jazyk</span><span class="sxs-lookup"><span data-stu-id="ff264-153">Extensible query language</span></span>

<span data-ttu-id="ff264-154">Možnosti dotazovacího jazyka můžete rozšířit definováním a vyvoláním dalších funkcí.</span><span class="sxs-lookup"><span data-stu-id="ff264-154">You can extend the capabilities of the query language by defining and invoking additional functions.</span></span> <span data-ttu-id="ff264-155">Můžete využít výhod řešení Azure Machine Learning a definovat v něm volání funkcí.</span><span class="sxs-lookup"><span data-stu-id="ff264-155">You can define function calls in the Azure Machine Learning service to take advantage of Azure Machine Learning solutions.</span></span> <span data-ttu-id="ff264-156">Pokud chcete provádět složitější výpočty v rámci dotazu Stream Analytics, můžete také integrovat uživatelem definované funkce jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff264-156">You can also integrate JavaScript user-defined functions (UDFs) in order to perform complex calculations as part a Stream Analytics query.</span></span>

### <a name="scalability"></a><span data-ttu-id="ff264-157">Škálovatelnost</span><span class="sxs-lookup"><span data-stu-id="ff264-157">Scalability</span></span>

<span data-ttu-id="ff264-158">Stream Analytics dokáže zpracovat až 1 GB příchozích dat za sekundu.</span><span class="sxs-lookup"><span data-stu-id="ff264-158">Stream Analytics can handle up to 1 GB of incoming data per second.</span></span> <span data-ttu-id="ff264-159">Integrace se službami [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) umožňuje úlohám ingestovat miliony událostí za sekundu, například událostí odesílaných z připojených zařízení, webů a souborů protokolů.</span><span class="sxs-lookup"><span data-stu-id="ff264-159">Integration with [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) and [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) allows jobs to ingest millions of events per second coming from connected devices, clickstreams, and log files, to name a few.</span></span> <span data-ttu-id="ff264-160">Centra událostí nabízejí funkci oddílů, pomocí které můžete výpočty dělit na logické kroky – ty pak můžete dělit ještě podrobněji, pokud chcete zvýšit škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="ff264-160">Using the partition feature of event hubs, you can partition computations into logical steps, each with the ability to be further partitioned to increase scalability.</span></span>

### <a name="low-cost"></a><span data-ttu-id="ff264-161">Nízké náklady</span><span class="sxs-lookup"><span data-stu-id="ff264-161">Low cost</span></span>

<span data-ttu-id="ff264-162">Stream Analytics je cloudová služba, takže umožňuje začít s minimálními náklady.</span><span class="sxs-lookup"><span data-stu-id="ff264-162">As a cloud service, Stream Analytics is optimized to let you get going at low cost.</span></span> <span data-ttu-id="ff264-163">Platíte na základě jednotek datových proudů a tedy míry svého používání ve smyslu objemu zpracovaných dat.</span><span class="sxs-lookup"><span data-stu-id="ff264-163">You pay as you go based on streaming-unit usage and the amount of data processed by the system.</span></span> <span data-ttu-id="ff264-164">Míra využití je založena na objemu zpracovaných událostí a výpočetním výkonu poskytnutém v rámci clusteru ke zpracování úlohy služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="ff264-164">Usage is derived based on the volume of events processed and the amount of compute power provisioned within the cluster to handle Stream Analytics jobs.</span></span>

### <a name="reliability-quick-recovery-and-repeatability"></a><span data-ttu-id="ff264-165">Spolehlivost, rychlé obnovení a opakovatelnost</span><span class="sxs-lookup"><span data-stu-id="ff264-165">Reliability, quick recovery, and repeatability</span></span>

<span data-ttu-id="ff264-166">Stream Analytics je spravovaná služba v cloudu, což pomáhá předcházet ztrátě dat a zajišťovat kontinuitu podnikových procesů.</span><span class="sxs-lookup"><span data-stu-id="ff264-166">As a managed service in the cloud, Stream Analytics helps prevent data loss and provides business continuity.</span></span> <span data-ttu-id="ff264-167">V případě selhání použije služba integrované funkce pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="ff264-167">If failures occur, the service provides built-in recovery capabilities.</span></span> <span data-ttu-id="ff264-168">Díky schopnosti interního zachování stavu služba poskytuje možnost archivace událostí a opakovaného zpracování dat v budoucnosti, které vždy vrátí stejné výsledky.</span><span class="sxs-lookup"><span data-stu-id="ff264-168">With the ability to internally maintain state, the service provides repeatable results ensuring it is possible to archive events and reapply processing in the future, always getting the same results.</span></span> <span data-ttu-id="ff264-169">To umožňuje vracet se v čase a při provádění analýz původních příčin a citlivostních analýz opakovaně zkoumat výpočty a jejich výsledky.</span><span class="sxs-lookup"><span data-stu-id="ff264-169">This enables you to go back in time and investigate computations when doing root-cause analysis, what-if analysis, and so on.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff264-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff264-170">Next steps</span></span>

* <span data-ttu-id="ff264-171">Začněte [experimentovat se vstupy a dotazy ze zařízení IoT](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).</span><span class="sxs-lookup"><span data-stu-id="ff264-171">Get started by [experimenting with inputs and queries from IoT devices](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).</span></span>
* <span data-ttu-id="ff264-172">Sestavte [ucelené řešení Stream Analytics](stream-analytics-real-time-fraud-detection.md), které bude zkoumat telefonní metadata a hledat podvodná volání.</span><span class="sxs-lookup"><span data-stu-id="ff264-172">Build an [end-to-end Stream Analytics solution](stream-analytics-real-time-fraud-detection.md) that examines telephone metadata to look for fraudulent calls.</span></span>
* <span data-ttu-id="ff264-173">Seznamte se s dotazovacím jazykem typu SQL pro Stream Analytics a s jedinečnými koncepty, jako jsou [oddílové funkce](stream-analytics-window-functions.md).</span><span class="sxs-lookup"><span data-stu-id="ff264-173">Learn about the SQL-like query language for Stream Analytics, and about unique concepts like [window functions](stream-analytics-window-functions.md).</span></span>
* <span data-ttu-id="ff264-174">Zjistěte, jak [škálovat úlohy Stream Analytics](stream-analytics-scale-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="ff264-174">Learn how to [scale Stream Analytics jobs](stream-analytics-scale-jobs.md).</span></span> 
* <span data-ttu-id="ff264-175">Zjistěte, jak [integrovat Stream Analytics a Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="ff264-175">Learn how to [integrate Stream Analytics and Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).</span></span>
* <span data-ttu-id="ff264-176">Najděte odpovědi na své dotazy ke službě Stream Analytics ve [fóru Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="ff264-176">Find answers to your Stream Analytics questions in the [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>
