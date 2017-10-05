---
title: "Import dat k analýze ve službě Azure Application Insights | Microsoft Docs"
description: "Importovat statických dat pro připojení s telemetrií aplikace, nebo importovat samostatné datový proud do dotazu s Analytics."
services: application-insights
keywords: "Otevřete schéma, import dat"
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bwren
ms.openlocfilehash: aa855a9050ec4e5e7c5db88b7209b8bb48bdba51
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="import-data-into-analytics"></a><span data-ttu-id="8d1e0-104">Importovat data do Analytics</span><span class="sxs-lookup"><span data-stu-id="8d1e0-104">Import data into Analytics</span></span>

<span data-ttu-id="8d1e0-105">Importovat žádná tabulková data do [Analytics](app-insights-analytics.md), buď propojit je s [Application Insights](app-insights-overview.md) telemetrie z vaší aplikace, nebo tak, aby je bylo možné analyzovat jako samostatné datový proud.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-105">Import any tabular data into [Analytics](app-insights-analytics.md), either to join it with [Application Insights](app-insights-overview.md) telemetry from your app, or so that you can analyze it as a separate stream.</span></span> <span data-ttu-id="8d1e0-106">Analytics je účinný dotazovací jazyk, který je vhodné řešení pro analýzu proudů označen časovým razítkem vysoký počet telemetrie.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-106">Analytics is a powerful query language well-suited to analyzing high-volume timestamped streams of telemetry.</span></span>

<span data-ttu-id="8d1e0-107">Data můžete importovat do analýzy pomocí vlastního schématu.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-107">You can import data into Analytics using your own schema.</span></span> <span data-ttu-id="8d1e0-108">Nemá používat standardní schémata Application Insights, jako je například požadavek nebo trasování.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-108">It doesn't have to use the standard Application Insights schemas such as request or trace.</span></span>

<span data-ttu-id="8d1e0-109">Můžete importovat JSON nebo DSV (oddělovač oddělovači - čárku, středník nebo kartě) soubory.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-109">You can import JSON or DSV (delimiter-separated values - comma, semicolon or tab) files.</span></span>

<span data-ttu-id="8d1e0-110">Existují tři situacích výhodné import do Analytics:</span><span class="sxs-lookup"><span data-stu-id="8d1e0-110">There are three situations where importing to Analytics is useful:</span></span>

* <span data-ttu-id="8d1e0-111">**Připojte se telemetrie aplikace.**</span><span class="sxs-lookup"><span data-stu-id="8d1e0-111">**Join with app telemetry.**</span></span> <span data-ttu-id="8d1e0-112">Mohli byste například importovat tabulku, která mapuje adresy URL vašeho webu na titulů srozumitelnější.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-112">For example, you could import a table that maps URLs from your website to more readable page titles.</span></span> <span data-ttu-id="8d1e0-113">V analýzy můžete vytvořit sestavu grafu řídicího panelu, která obsahuje deset nejoblíbenější stránky ve vašem webu.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-113">In Analytics, you can create a dashboard chart report that shows the ten most popular pages in your website.</span></span> <span data-ttu-id="8d1e0-114">Nyní ji můžete zobrazují názvy stránky místo adresy URL.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-114">Now it can show the page titles instead of the URLs.</span></span>
* <span data-ttu-id="8d1e0-115">**Korelovat telemetrii aplikace** s jinými zdroji například síťových přenosů dat serveru nebo CDN soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-115">**Correlate your application telemetry** with other sources such as network traffic, server data, or CDN log files.</span></span>
* <span data-ttu-id="8d1e0-116">**Analýza platí pro samostatné datového proudu.**</span><span class="sxs-lookup"><span data-stu-id="8d1e0-116">**Apply Analytics to a separate data stream.**</span></span> <span data-ttu-id="8d1e0-117">Application Insights Analytics je výkonný nástroj, který pracuje s zhuštěné, označen časovým razítkem datové proudy - lépe než SQL v mnoha případech.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-117">Application Insights Analytics is a powerful tool, that works well with sparse, timestamped streams - much better than SQL in many cases.</span></span> <span data-ttu-id="8d1e0-118">Pokud máte datový proud z z jiného zdroje, můžete ho s Analytics analyzovat.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-118">If you have such a stream from some other source, you can analyze it with Analytics.</span></span>

<span data-ttu-id="8d1e0-119">Odesílání dat do zdroje dat je snadné.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-119">Sending data to your data source is easy.</span></span> 

1. <span data-ttu-id="8d1e0-120">(Jednou) Definování schématu dat v datovém zdroji.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-120">(One time) Define the schema of your data in a 'data source'.</span></span>
2. <span data-ttu-id="8d1e0-121">(Pravidelně) Nahrání dat do úložiště Azure a volání rozhraní REST API nám, které nová data se čeká na přijímání oznámení.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-121">(Periodically) Upload your data to Azure storage, and call the REST API to notify us that new data is waiting for ingestion.</span></span> <span data-ttu-id="8d1e0-122">Během několika minut je k dispozici pro dotaz v Analytics data.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-122">Within a few minutes, the data is available for query in Analytics.</span></span>

<span data-ttu-id="8d1e0-123">Frekvence odesílání je definována vy a jak rychlý chcete, aby vaše data k dispozici pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-123">The frequency of the upload is defined by you and how fast would you like your data to be available for queries.</span></span> <span data-ttu-id="8d1e0-124">Je efektivnější odesílat data v větší bloky dat, ale není větší než 1GB.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-124">It is more efficient to upload data in larger chunks, but not larger than 1GB.</span></span>

> [!NOTE]
> <span data-ttu-id="8d1e0-125">*Máte velký počet zdrojů dat k analýze?*</span><span class="sxs-lookup"><span data-stu-id="8d1e0-125">*Got lots of data sources to analyze?*</span></span> [<span data-ttu-id="8d1e0-126">*Zvažte použití* logstash *pro odeslání dat do služby Application Insights.*</span><span class="sxs-lookup"><span data-stu-id="8d1e0-126">*Consider using* logstash *to ship your data into Application Insights.*</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a><span data-ttu-id="8d1e0-127">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8d1e0-127">Before you start</span></span>

<span data-ttu-id="8d1e0-128">Budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="8d1e0-128">You need:</span></span>

1. <span data-ttu-id="8d1e0-129">Prostředek Application Insights v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-129">An Application Insights resource in Microsoft Azure.</span></span>

 * <span data-ttu-id="8d1e0-130">Pokud chcete analyzovat data odděleně od jiných telemetrie [vytvořte nový prostředek Application Insights](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="8d1e0-130">If you want to analyze your data separately from any other telemetry, [create a new Application Insights resource](app-insights-create-new-resource.md).</span></span>
 * <span data-ttu-id="8d1e0-131">Pokud chcete propojení nebo porovnávání svá data pomocí telemetrie z aplikace, která je již nastavena pomocí služby Application Insights, můžete použít na prostředek pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-131">If you're joining or comparing your data with telemetry from an app that is already set up with Application Insights, then you can use the resource for that app.</span></span>
 * <span data-ttu-id="8d1e0-132">Přispěvatel nebo vlastníka přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-132">Contributor or owner access to that resource.</span></span>
 
2. <span data-ttu-id="8d1e0-133">Úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-133">Azure storage.</span></span> <span data-ttu-id="8d1e0-134">Nahrát do úložiště Azure a analýzy získá vaše data z ní.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-134">You upload to Azure storage, and Analytics gets your data from there.</span></span> 

 * <span data-ttu-id="8d1e0-135">Doporučujeme že vytvořit účet vyhrazeného úložiště pro objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-135">We recommend you create a dedicated storage account for your blobs.</span></span> <span data-ttu-id="8d1e0-136">Pokud objektů BLOB jsou sdíleny s jinými procesy, trvá déle pro naše procesy ke čtení objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-136">If your blobs are shared with other processes, it takes longer for our processes to read your blobs.</span></span>


## <a name="define-your-schema"></a><span data-ttu-id="8d1e0-137">Definování schématu</span><span class="sxs-lookup"><span data-stu-id="8d1e0-137">Define your schema</span></span>

<span data-ttu-id="8d1e0-138">Před importem dat, je nutné definovat *zdroje dat,* který určuje schéma vaše data.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-138">Before you can import data, you must define a *data source,* which specifies the schema of your data.</span></span>
<span data-ttu-id="8d1e0-139">Může mít až 50 zdroje dat v jediném prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="8d1e0-139">You can have up to 50 data sources in a single Application Insights resource</span></span>

1. <span data-ttu-id="8d1e0-140">Spuštění Průvodce zdrojem dat.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-140">Start the data source wizard.</span></span> <span data-ttu-id="8d1e0-141">Použijte tlačítko "Přidat nový zdroj dat".</span><span class="sxs-lookup"><span data-stu-id="8d1e0-141">Use "Add new data source" button.</span></span> <span data-ttu-id="8d1e0-142">Případně – klikněte na tlačítko nastavení v pravém horním rohu a v rozevírací nabídce vyberte "Zdroje dat".</span><span class="sxs-lookup"><span data-stu-id="8d1e0-142">Alternatively - click on settings button in right upper corner and choose "Data Sources" in dropdown menu.</span></span>

    ![Přidat nový zdroj dat](./media/app-insights-analytics-import/add-new-data-source.png)

    <span data-ttu-id="8d1e0-144">Zadejte název nového zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-144">Provide a name for your new data source.</span></span>

2. <span data-ttu-id="8d1e0-145">Definování formátu souborů, které odešlete.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-145">Define format of the files that you will upload.</span></span>

    <span data-ttu-id="8d1e0-146">Můžete definovat formát ručně, nebo odeslání ukázkového souboru.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-146">You can either define the format manually, or upload a sample file.</span></span>

    <span data-ttu-id="8d1e0-147">Pokud jsou data ve formátu CSV, může být první řádek v ukázkovém záhlaví sloupců.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-147">If the data is in CSV format, the first row of the sample can be column headers.</span></span> <span data-ttu-id="8d1e0-148">Můžete změnit názvy polí v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-148">You can change the field names in the next step.</span></span>

    <span data-ttu-id="8d1e0-149">Ukázka by měla obsahovat alespoň 10 řádků nebo záznamů dat s.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-149">The sample should include at least 10 rows or records of data.</span></span>

    <span data-ttu-id="8d1e0-150">Sloupec nebo pole názvy by měly mít alfanumerické názvy (bez mezer a interpunkce).</span><span class="sxs-lookup"><span data-stu-id="8d1e0-150">Column or field names should have alphanumeric names (without spaces or punctuation).</span></span>

    ![Odeslání ukázkového souboru](./media/app-insights-analytics-import/sample-data-file.png)


3. <span data-ttu-id="8d1e0-152">Zkontrolujte schéma, které má potom průvodce.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-152">Review the schema that the wizard has got.</span></span> <span data-ttu-id="8d1e0-153">Pokud ho odvodit typů z ukázku, můžete upravit odvozené typy sloupců.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-153">If it inferred the types from a sample, you might need to adjust the inferred types of the columns.</span></span>

    ![Zkontrolujte odvozené schématu](./media/app-insights-analytics-import/data-source-review-schema.png)

 * <span data-ttu-id="8d1e0-155">(Volitelné.) Nahrajte definici schématu.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-155">(Optional.) Upload a schema definition.</span></span> <span data-ttu-id="8d1e0-156">Viz níže uvedeným formátem.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-156">See the format below.</span></span>

 * <span data-ttu-id="8d1e0-157">Vyberte časové razítko.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-157">Select a Timestamp.</span></span> <span data-ttu-id="8d1e0-158">Všechna data v Analytics musí mít pole časového razítka.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-158">All data in Analytics must have a timestamp field.</span></span> <span data-ttu-id="8d1e0-159">Musí mít typ `datetime`, ale nemá s názvem 'časové razítko'.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-159">It must have type `datetime`, but it doesn't have to be named 'timestamp'.</span></span> <span data-ttu-id="8d1e0-160">Pokud data obsahují sloupec obsahující datum a čas ve formátu ISO, zvolte to jako sloupec časového razítka.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-160">If your data has a column containing a date and time in ISO format, choose this as the timestamp column.</span></span> <span data-ttu-id="8d1e0-161">Jinak, vyberte "jako data doručen" a proces importu přidá pole časového razítka.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-161">Otherwise, choose "as data arrived", and the import process will add a timestamp field.</span></span>

5. <span data-ttu-id="8d1e0-162">Vytvoření zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-162">Create the data source.</span></span>

### <a name="schema-definition-file-format"></a><span data-ttu-id="8d1e0-163">Formát souboru definice schématu</span><span class="sxs-lookup"><span data-stu-id="8d1e0-163">Schema definition file format</span></span>

<span data-ttu-id="8d1e0-164">Neupravujte schématu v uživatelském rozhraní, můžete načíst definici schématu ze souboru.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-164">Instead of editing the schema in UI, you can load the schema definition from a file.</span></span> <span data-ttu-id="8d1e0-165">Formát definice schématu je následující:</span><span class="sxs-lookup"><span data-stu-id="8d1e0-165">The schema definition format is as follows:</span></span> 

<span data-ttu-id="8d1e0-166">Formát s oddělovačem</span><span class="sxs-lookup"><span data-stu-id="8d1e0-166">Delimited format</span></span> 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

<span data-ttu-id="8d1e0-167">Formát JSON</span><span class="sxs-lookup"><span data-stu-id="8d1e0-167">JSON format</span></span> 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
<span data-ttu-id="8d1e0-168">Každý sloupec je určený podle umístění, název a typ.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-168">Each column is identified by the location, name and type.</span></span> 

* <span data-ttu-id="8d1e0-169">Umístění – souboru s oddělovači naformátujte ho je umístění namapované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-169">Location – For delimited file format it is the position of the mapped value.</span></span> <span data-ttu-id="8d1e0-170">Pro formát JSON je jpath namapované klíče.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-170">For JSON format, it is the jpath of the mapped key.</span></span>
* <span data-ttu-id="8d1e0-171">Název – zobrazovaný název sloupce.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-171">Name – the displayed name of the column.</span></span>
* <span data-ttu-id="8d1e0-172">Typ – datový typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-172">Type – the data type of that column.</span></span>
 
<span data-ttu-id="8d1e0-173">V případě, že byl použit ukázková data a jsou odděleny formát souboru, musí definice schématu mapovat všechny sloupce a přidejte nové sloupce na konci.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-173">In case a sample data was used and file format is delimited, the schema definition must map all columns and add new columns at the end.</span></span> 

<span data-ttu-id="8d1e0-174">JSON umožňuje částečné mapování dat, proto definice schématu formátu JSON nemá pro každý klíč, který se nachází v ukázková data mapování.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-174">JSON allows partial mapping of the data, therefore the schema definition of JSON format doesn’t have to map every key which is found in a sample data.</span></span> <span data-ttu-id="8d1e0-175">Také ho můžete namapovat sloupce, které nejsou součástí ukázková data.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-175">It can also map columns which are not part of the sample data.</span></span> 

## <a name="import-data"></a><span data-ttu-id="8d1e0-176">Import dat</span><span class="sxs-lookup"><span data-stu-id="8d1e0-176">Import data</span></span>

<span data-ttu-id="8d1e0-177">K importu dat, nahrajte ho do úložiště Azure, vytvořte přístupový klíč pro tuto a pak proveďte volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-177">To import data, you upload it to Azure storage, create an access key for it, and then make a REST API call.</span></span>

![Přidat nový zdroj dat](./media/app-insights-analytics-import/analytics-upload-process.png)

<span data-ttu-id="8d1e0-179">Můžete provést následující proces ručně, nebo nastavit automatizovaný systém to udělat v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-179">You can perform the following process manually, or set up an automated system to do it at regular intervals.</span></span> <span data-ttu-id="8d1e0-180">Musíte provést následující kroky pro každý blok dat, které chcete importovat.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-180">You need to follow these steps for each block of data you want to import.</span></span>

1. <span data-ttu-id="8d1e0-181">Nahrání dat do [úložiště objektů blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="8d1e0-181">Upload the data to [Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> 

 * <span data-ttu-id="8d1e0-182">Objekty BLOB může být libovolné velikosti až 1GB nekomprimovaným.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-182">Blobs can be any size up to 1GB uncompressed.</span></span> <span data-ttu-id="8d1e0-183">Velké objekty BLOB stovek MB jsou ideální z hlediska výkonu.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-183">Large blobs of hundreds of MB are ideal from a performance perspective.</span></span>
 * <span data-ttu-id="8d1e0-184">Můžete je komprimovat s Gzip ke zlepšení doba nahrávání a latence pro data, která mají být k dispozici pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-184">You can compress it with Gzip to improve upload time and latency for the data to be available for query.</span></span> <span data-ttu-id="8d1e0-185">Použití `.gz` příponu názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-185">Use the `.gz` filename extension.</span></span>
 * <span data-ttu-id="8d1e0-186">Je nejvhodnější použít účet samostatného úložiště pro tento účel, aby se zabránilo volání z jiné služby zpomalení výkonu.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-186">It's best to use a separate storage account for this purpose, to avoid calls from different services slowing performance.</span></span>
 * <span data-ttu-id="8d1e0-187">Při odesílání dat v vysoká frekvence, každých několik sekund, se doporučuje použít více než jeden účet úložiště, z důvodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-187">When sending data in high frequency, every few seconds, it is recommended to use more than one storage account, for performance reasons.</span></span>

 
2. <span data-ttu-id="8d1e0-188">[Vytvoření klíče sdíleného přístupového podpisu pro tento objekt blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="8d1e0-188">[Create a Shared Access Signature key for the blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span></span> <span data-ttu-id="8d1e0-189">Klíč by měl mít jeden den po dobu vypršení platnosti a poskytovat přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-189">The key should have an expiration period of one day and provide read access.</span></span>
3. <span data-ttu-id="8d1e0-190">Volání REST oznámit Application Insights, čekající data.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-190">Make a REST call to notify Application Insights that data is waiting.</span></span>

 * <span data-ttu-id="8d1e0-191">Koncový bod:`https://dc.services.visualstudio.com/v2/track`</span><span class="sxs-lookup"><span data-stu-id="8d1e0-191">Endpoint: `https://dc.services.visualstudio.com/v2/track`</span></span>
 * <span data-ttu-id="8d1e0-192">Metoda HTTP: POST</span><span class="sxs-lookup"><span data-stu-id="8d1e0-192">HTTP method: POST</span></span>
 * <span data-ttu-id="8d1e0-193">Datové části:</span><span class="sxs-lookup"><span data-stu-id="8d1e0-193">Payload:</span></span>

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

<span data-ttu-id="8d1e0-194">Zástupné symboly jsou:</span><span class="sxs-lookup"><span data-stu-id="8d1e0-194">The placeholders are:</span></span>

* <span data-ttu-id="8d1e0-195">`Blob URI with Shared Access Key`: Zobrazí to v postupu pro vytváření klíčů.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-195">`Blob URI with Shared Access Key`: You get this from the procedure for creating a key.</span></span> <span data-ttu-id="8d1e0-196">Je specifická pro objekt blob.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-196">It is specific to the blob.</span></span>
* <span data-ttu-id="8d1e0-197">`Schema ID`: ID schématu vygenerované definované schéma.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-197">`Schema ID`: The schema ID generated for your defined schema.</span></span> <span data-ttu-id="8d1e0-198">Data v tohoto objektu blob musí být v souladu schématu.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-198">The data in this blob should conform to the schema.</span></span>
* <span data-ttu-id="8d1e0-199">`DateTime`: Čas, kdy je odeslána žádost, UTC.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-199">`DateTime`: The time at which the request is submitted, UTC.</span></span> <span data-ttu-id="8d1e0-200">Můžeme přijmout tyto formáty: ISO8601 (jako je "2016-01-01 13:45:01"); Rfc822 ("ST, 14 16 DEC – 14:57:01 + 0000"); RFC850 ("středa, 14. prosince 16 UTC 14:57:00"); RFC1123 ("st 14 Dec 2016 14:57:00 + 0000").</span><span class="sxs-lookup"><span data-stu-id="8d1e0-200">We accept these formats: ISO8601 (like "2016-01-01 13:45:01"); RFC822 ("Wed, 14 Dec 16 14:57:01 +0000"); RFC850 ("Wednesday, 14-Dec-16 14:57:00 UTC"); RFC1123 ("Wed, 14 Dec 2016 14:57:00 +0000").</span></span>
* <span data-ttu-id="8d1e0-201">`Instrumentation key`z prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-201">`Instrumentation key` of your Application Insights resource.</span></span>

<span data-ttu-id="8d1e0-202">Data jsou k dispozici v Analytics po několika minutách.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-202">The data is available in Analytics after a few minutes.</span></span>

## <a name="error-responses"></a><span data-ttu-id="8d1e0-203">Chybové odpovědi</span><span class="sxs-lookup"><span data-stu-id="8d1e0-203">Error responses</span></span>

* <span data-ttu-id="8d1e0-204">**požadavek je 400 nesprávný**: označuje, že datová část požadavku je neplatné.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-204">**400 bad request**: indicates that the request payload is invalid.</span></span> <span data-ttu-id="8d1e0-205">Kontrola:</span><span class="sxs-lookup"><span data-stu-id="8d1e0-205">Check:</span></span>
 * <span data-ttu-id="8d1e0-206">Klíč instrumentace správné.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-206">Correct instrumentation key.</span></span>
 * <span data-ttu-id="8d1e0-207">Hodnota doby platnosti.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-207">Valid time value.</span></span> <span data-ttu-id="8d1e0-208">Mělo by být čas nyní ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-208">It should be the time now in UTC.</span></span>
 * <span data-ttu-id="8d1e0-209">JSON události odpovídá schématu.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-209">JSON of the event conforms to the schema.</span></span>
* <span data-ttu-id="8d1e0-210">**403 Zakázáno**: Objekt blob, které jste odeslali není dostupný.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-210">**403 Forbidden**: The blob you've sent is not accessible.</span></span> <span data-ttu-id="8d1e0-211">Ujistěte se, že je sdílený přístupový klíč platný a zda nevypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-211">Make sure that the shared access key is valid and has not expired.</span></span>
* <span data-ttu-id="8d1e0-212">**404 nebyl nalezen**:</span><span class="sxs-lookup"><span data-stu-id="8d1e0-212">**404 Not Found**:</span></span>
 * <span data-ttu-id="8d1e0-213">Objekt blob neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-213">The blob doesn't exist.</span></span>
 * <span data-ttu-id="8d1e0-214">ID zdroje je nesprávné.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-214">The sourceId is wrong.</span></span>

<span data-ttu-id="8d1e0-215">Podrobnější informace najdete v chybové zprávě odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-215">More detailed information is available in the response error message.</span></span>


## <a name="sample-code"></a><span data-ttu-id="8d1e0-216">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="8d1e0-216">Sample code</span></span>

<span data-ttu-id="8d1e0-217">Tento kód používá [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-217">This code uses the [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet package.</span></span>

### <a name="classes"></a><span data-ttu-id="8d1e0-218">Třídy</span><span class="sxs-lookup"><span data-stu-id="8d1e0-218">Classes</span></span>

```C#
namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 
```

### <a name="ingest-data"></a><span data-ttu-id="8d1e0-219">Příjem dat</span><span class="sxs-lookup"><span data-stu-id="8d1e0-219">Ingest data</span></span>

<span data-ttu-id="8d1e0-220">Pomocí tohoto kódu pro každý objekt blob.</span><span class="sxs-lookup"><span data-stu-id="8d1e0-220">Use this code for each blob.</span></span> 

```C#
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a><span data-ttu-id="8d1e0-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8d1e0-221">Next steps</span></span>

* [<span data-ttu-id="8d1e0-222">Prohlídka dotazovací jazyk analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="8d1e0-222">Tour of the Log Analytics query language</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="8d1e0-223">Použití *Logstash* k odesílání dat do služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="8d1e0-223">Use *Logstash* to send data to Application Insights</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
