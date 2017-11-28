---
title: "aaaImport tooAnalytics vaše data ve službě Azure Application Insights | Microsoft Docs"
description: "Importovat toojoin statických dat s telemetrií aplikace, nebo importovat datového proudu tooquery samostatné dat s Analytics."
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
ms.openlocfilehash: 7a3a3c9155adc1885dd366ddb13dda80bb894adb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-into-analytics"></a><span data-ttu-id="19333-104">Importovat data do Analytics</span><span class="sxs-lookup"><span data-stu-id="19333-104">Import data into Analytics</span></span>

<span data-ttu-id="19333-105">Importovat žádná tabulková data do [Analytics](app-insights-analytics.md), buď toojoin její [Application Insights](app-insights-overview.md) telemetrie z vaší aplikace, nebo tak, aby je bylo možné analyzovat jako samostatné datový proud.</span><span class="sxs-lookup"><span data-stu-id="19333-105">Import any tabular data into [Analytics](app-insights-analytics.md), either toojoin it with [Application Insights](app-insights-overview.md) telemetry from your app, or so that you can analyze it as a separate stream.</span></span> <span data-ttu-id="19333-106">Analytics je účinný dotazovací jazyk vhodným tooanalyzing označen časovým razítkem vysoký počet datových proudů telemetrie.</span><span class="sxs-lookup"><span data-stu-id="19333-106">Analytics is a powerful query language well-suited tooanalyzing high-volume timestamped streams of telemetry.</span></span>

<span data-ttu-id="19333-107">Data můžete importovat do analýzy pomocí vlastního schématu.</span><span class="sxs-lookup"><span data-stu-id="19333-107">You can import data into Analytics using your own schema.</span></span> <span data-ttu-id="19333-108">Nemá toouse hello standardní Application Insights schémata například požadavek nebo trasování.</span><span class="sxs-lookup"><span data-stu-id="19333-108">It doesn't have toouse hello standard Application Insights schemas such as request or trace.</span></span>

<span data-ttu-id="19333-109">Můžete importovat JSON nebo DSV (oddělovač oddělovači - čárku, středník nebo kartě) soubory.</span><span class="sxs-lookup"><span data-stu-id="19333-109">You can import JSON or DSV (delimiter-separated values - comma, semicolon or tab) files.</span></span>

<span data-ttu-id="19333-110">Existují tři situacích výhodné import tooAnalytics:</span><span class="sxs-lookup"><span data-stu-id="19333-110">There are three situations where importing tooAnalytics is useful:</span></span>

* <span data-ttu-id="19333-111">**Připojte se telemetrie aplikace.**</span><span class="sxs-lookup"><span data-stu-id="19333-111">**Join with app telemetry.**</span></span> <span data-ttu-id="19333-112">Mohli byste například importovat tabulku, která mapuje adresy URL z vašeho webu toomore čitelný titulů.</span><span class="sxs-lookup"><span data-stu-id="19333-112">For example, you could import a table that maps URLs from your website toomore readable page titles.</span></span> <span data-ttu-id="19333-113">V analýzy můžete vytvořit sestavu grafu řídicího panelu, která obsahuje hello deset nejoblíbenější stránky ve vašem webu.</span><span class="sxs-lookup"><span data-stu-id="19333-113">In Analytics, you can create a dashboard chart report that shows hello ten most popular pages in your website.</span></span> <span data-ttu-id="19333-114">Nyní ji můžete zobrazit hello místo adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="19333-114">Now it can show hello page titles instead of hello URLs.</span></span>
* <span data-ttu-id="19333-115">**Korelovat telemetrii aplikace** s jinými zdroji například síťových přenosů dat serveru nebo CDN soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="19333-115">**Correlate your application telemetry** with other sources such as network traffic, server data, or CDN log files.</span></span>
* <span data-ttu-id="19333-116">**Použijte Analytics tooa samostatné datového proudu.**</span><span class="sxs-lookup"><span data-stu-id="19333-116">**Apply Analytics tooa separate data stream.**</span></span> <span data-ttu-id="19333-117">Application Insights Analytics je výkonný nástroj, který pracuje s zhuštěné, označen časovým razítkem datové proudy - lépe než SQL v mnoha případech.</span><span class="sxs-lookup"><span data-stu-id="19333-117">Application Insights Analytics is a powerful tool, that works well with sparse, timestamped streams - much better than SQL in many cases.</span></span> <span data-ttu-id="19333-118">Pokud máte datový proud z z jiného zdroje, můžete ho s Analytics analyzovat.</span><span class="sxs-lookup"><span data-stu-id="19333-118">If you have such a stream from some other source, you can analyze it with Analytics.</span></span>

<span data-ttu-id="19333-119">Odesílání zdroj dat datového tooyour je snadné.</span><span class="sxs-lookup"><span data-stu-id="19333-119">Sending data tooyour data source is easy.</span></span> 

1. <span data-ttu-id="19333-120">(Jednou) Definujte hello schématu dat v datovém zdroji.</span><span class="sxs-lookup"><span data-stu-id="19333-120">(One time) Define hello schema of your data in a 'data source'.</span></span>
2. <span data-ttu-id="19333-121">(Pravidelně) Nahrání dat tooAzure úložiště a volání rozhraní REST API toonotify hello nám, které nová data se čeká na přijímání.</span><span class="sxs-lookup"><span data-stu-id="19333-121">(Periodically) Upload your data tooAzure storage, and call hello REST API toonotify us that new data is waiting for ingestion.</span></span> <span data-ttu-id="19333-122">Během několika minut je k dispozici pro dotaz v analýzy dat hello.</span><span class="sxs-lookup"><span data-stu-id="19333-122">Within a few minutes, hello data is available for query in Analytics.</span></span>

<span data-ttu-id="19333-123">Hello frekvence odesílání hello je definované uživatelem a jak rychle chcete toobe vaše data, k dispozici pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="19333-123">hello frequency of hello upload is defined by you and how fast would you like your data toobe available for queries.</span></span> <span data-ttu-id="19333-124">Je efektivnější tooupload data v větší bloky dat, ale není větší než 1GB.</span><span class="sxs-lookup"><span data-stu-id="19333-124">It is more efficient tooupload data in larger chunks, but not larger than 1GB.</span></span>

> [!NOTE]
> <span data-ttu-id="19333-125">*Máte spoustu tooanalyze zdroje dat?*</span><span class="sxs-lookup"><span data-stu-id="19333-125">*Got lots of data sources tooanalyze?*</span></span> [<span data-ttu-id="19333-126">*Zvažte použití* logstash *tooship dat do služby Application Insights.*</span><span class="sxs-lookup"><span data-stu-id="19333-126">*Consider using* logstash *tooship your data into Application Insights.*</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a><span data-ttu-id="19333-127">Než začnete</span><span class="sxs-lookup"><span data-stu-id="19333-127">Before you start</span></span>

<span data-ttu-id="19333-128">Budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="19333-128">You need:</span></span>

1. <span data-ttu-id="19333-129">Prostředek Application Insights v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="19333-129">An Application Insights resource in Microsoft Azure.</span></span>

 * <span data-ttu-id="19333-130">Pokud chcete tooanalyze data odděleně od jiných telemetrie [vytvořte nový prostředek Application Insights](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="19333-130">If you want tooanalyze your data separately from any other telemetry, [create a new Application Insights resource](app-insights-create-new-resource.md).</span></span>
 * <span data-ttu-id="19333-131">Pokud chcete propojení nebo porovnávání svá data pomocí telemetrie z aplikace, která je již nastavena pomocí služby Application Insights, můžete použít hello prostředků pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="19333-131">If you're joining or comparing your data with telemetry from an app that is already set up with Application Insights, then you can use hello resource for that app.</span></span>
 * <span data-ttu-id="19333-132">Přispěvatel nebo vlastníka přístup toothat prostředek.</span><span class="sxs-lookup"><span data-stu-id="19333-132">Contributor or owner access toothat resource.</span></span>
 
2. <span data-ttu-id="19333-133">Úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="19333-133">Azure storage.</span></span> <span data-ttu-id="19333-134">Nahrát tooAzure úložiště a analýzy získá vaše data z ní.</span><span class="sxs-lookup"><span data-stu-id="19333-134">You upload tooAzure storage, and Analytics gets your data from there.</span></span> 

 * <span data-ttu-id="19333-135">Doporučujeme že vytvořit účet vyhrazeného úložiště pro objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="19333-135">We recommend you create a dedicated storage account for your blobs.</span></span> <span data-ttu-id="19333-136">Pokud objektů BLOB jsou sdíleny s jinými procesy, trvá déle pro naše tooread procesy objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="19333-136">If your blobs are shared with other processes, it takes longer for our processes tooread your blobs.</span></span>


## <a name="define-your-schema"></a><span data-ttu-id="19333-137">Definování schématu</span><span class="sxs-lookup"><span data-stu-id="19333-137">Define your schema</span></span>

<span data-ttu-id="19333-138">Aby bylo možné naimportovat data, je nutné definovat *zdroje dat,* který určuje hello schématu dat.</span><span class="sxs-lookup"><span data-stu-id="19333-138">Before you can import data, you must define a *data source,* which specifies hello schema of your data.</span></span>
<span data-ttu-id="19333-139">Může mít až too50 zdroje dat v jediném prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="19333-139">You can have up too50 data sources in a single Application Insights resource</span></span>

1. <span data-ttu-id="19333-140">Spuštění Průvodce zdrojem dat hello.</span><span class="sxs-lookup"><span data-stu-id="19333-140">Start hello data source wizard.</span></span> <span data-ttu-id="19333-141">Použijte tlačítko "Přidat nový zdroj dat".</span><span class="sxs-lookup"><span data-stu-id="19333-141">Use "Add new data source" button.</span></span> <span data-ttu-id="19333-142">Případně – klikněte na tlačítko nastavení v pravém horním rohu a v rozevírací nabídce vyberte "Zdroje dat".</span><span class="sxs-lookup"><span data-stu-id="19333-142">Alternatively - click on settings button in right upper corner and choose "Data Sources" in dropdown menu.</span></span>

    ![Přidat nový zdroj dat](./media/app-insights-analytics-import/add-new-data-source.png)

    <span data-ttu-id="19333-144">Zadejte název nového zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="19333-144">Provide a name for your new data source.</span></span>

2. <span data-ttu-id="19333-145">Definování formátu hello souborů, které odešlete.</span><span class="sxs-lookup"><span data-stu-id="19333-145">Define format of hello files that you will upload.</span></span>

    <span data-ttu-id="19333-146">Můžete definovat hello formátu ručně, nebo odeslání ukázkového souboru.</span><span class="sxs-lookup"><span data-stu-id="19333-146">You can either define hello format manually, or upload a sample file.</span></span>

    <span data-ttu-id="19333-147">Pokud hello data jsou ve formátu CSV, může být první řádek hello hello ukázky záhlaví sloupců.</span><span class="sxs-lookup"><span data-stu-id="19333-147">If hello data is in CSV format, hello first row of hello sample can be column headers.</span></span> <span data-ttu-id="19333-148">Můžete změnit názvy polí hello v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="19333-148">You can change hello field names in hello next step.</span></span>

    <span data-ttu-id="19333-149">Ukázka Hello by měla obsahovat alespoň 10 řádků nebo záznamů dat s.</span><span class="sxs-lookup"><span data-stu-id="19333-149">hello sample should include at least 10 rows or records of data.</span></span>

    <span data-ttu-id="19333-150">Sloupec nebo pole názvy by měly mít alfanumerické názvy (bez mezer a interpunkce).</span><span class="sxs-lookup"><span data-stu-id="19333-150">Column or field names should have alphanumeric names (without spaces or punctuation).</span></span>

    ![Odeslání ukázkového souboru](./media/app-insights-analytics-import/sample-data-file.png)


3. <span data-ttu-id="19333-152">Je tu zkontrolujte hello schématu, které hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="19333-152">Review hello schema that hello wizard has got.</span></span> <span data-ttu-id="19333-153">Pokud ho odvodit hello typů z ukázku, bude pravděpodobně nutné tooadjust hello odvozené typy sloupců hello.</span><span class="sxs-lookup"><span data-stu-id="19333-153">If it inferred hello types from a sample, you might need tooadjust hello inferred types of hello columns.</span></span>

    ![Zkontrolujte hello odvodit schématu](./media/app-insights-analytics-import/data-source-review-schema.png)

 * <span data-ttu-id="19333-155">(Volitelné.) Nahrajte definici schématu.</span><span class="sxs-lookup"><span data-stu-id="19333-155">(Optional.) Upload a schema definition.</span></span> <span data-ttu-id="19333-156">Viz níže uvedeným formátem hello.</span><span class="sxs-lookup"><span data-stu-id="19333-156">See hello format below.</span></span>

 * <span data-ttu-id="19333-157">Vyberte časové razítko.</span><span class="sxs-lookup"><span data-stu-id="19333-157">Select a Timestamp.</span></span> <span data-ttu-id="19333-158">Všechna data v Analytics musí mít pole časového razítka.</span><span class="sxs-lookup"><span data-stu-id="19333-158">All data in Analytics must have a timestamp field.</span></span> <span data-ttu-id="19333-159">Musí mít typ `datetime`, ale nemá toobe s názvem 'časové razítko'.</span><span class="sxs-lookup"><span data-stu-id="19333-159">It must have type `datetime`, but it doesn't have toobe named 'timestamp'.</span></span> <span data-ttu-id="19333-160">Pokud data obsahují sloupec obsahující datum a čas ve formátu ISO, zvolte to jako sloupec časového razítka hello.</span><span class="sxs-lookup"><span data-stu-id="19333-160">If your data has a column containing a date and time in ISO format, choose this as hello timestamp column.</span></span> <span data-ttu-id="19333-161">Jinak, vyberte "jako data doručen" a proces importu hello přidá pole časového razítka.</span><span class="sxs-lookup"><span data-stu-id="19333-161">Otherwise, choose "as data arrived", and hello import process will add a timestamp field.</span></span>

5. <span data-ttu-id="19333-162">Vytvoření zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="19333-162">Create hello data source.</span></span>

### <a name="schema-definition-file-format"></a><span data-ttu-id="19333-163">Formát souboru definice schématu</span><span class="sxs-lookup"><span data-stu-id="19333-163">Schema definition file format</span></span>

<span data-ttu-id="19333-164">Neupravujte hello schématu v uživatelském rozhraní, můžete načíst definice schématu hello ze souboru.</span><span class="sxs-lookup"><span data-stu-id="19333-164">Instead of editing hello schema in UI, you can load hello schema definition from a file.</span></span> <span data-ttu-id="19333-165">Formát definice schématu Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="19333-165">hello schema definition format is as follows:</span></span> 

<span data-ttu-id="19333-166">Formát s oddělovačem</span><span class="sxs-lookup"><span data-stu-id="19333-166">Delimited format</span></span> 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

<span data-ttu-id="19333-167">Formát JSON</span><span class="sxs-lookup"><span data-stu-id="19333-167">JSON format</span></span> 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
<span data-ttu-id="19333-168">Každý sloupec je identifikována hello umístění, název a typ.</span><span class="sxs-lookup"><span data-stu-id="19333-168">Each column is identified by hello location, name and type.</span></span> 

* <span data-ttu-id="19333-169">Umístění – souboru s oddělovači naformátujte ho je pozice hello hello namapované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="19333-169">Location – For delimited file format it is hello position of hello mapped value.</span></span> <span data-ttu-id="19333-170">Pro formát JSON je hello jpath hello namapované klíče.</span><span class="sxs-lookup"><span data-stu-id="19333-170">For JSON format, it is hello jpath of hello mapped key.</span></span>
* <span data-ttu-id="19333-171">Název – hello zobrazí název sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="19333-171">Name – hello displayed name of hello column.</span></span>
* <span data-ttu-id="19333-172">Typ – hello datový typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="19333-172">Type – hello data type of that column.</span></span>
 
<span data-ttu-id="19333-173">V případě, že byl použit ukázková data a jsou odděleny formát souboru, musí definice schématu hello mapovat všechny sloupce a přidejte nové sloupce na konci hello.</span><span class="sxs-lookup"><span data-stu-id="19333-173">In case a sample data was used and file format is delimited, hello schema definition must map all columns and add new columns at hello end.</span></span> 

<span data-ttu-id="19333-174">JSON umožňuje částečné mapování hello dat, proto hello definice schématu formátu JSON nemá toomap každým klíčem, který se nachází v ukázková data.</span><span class="sxs-lookup"><span data-stu-id="19333-174">JSON allows partial mapping of hello data, therefore hello schema definition of JSON format doesn’t have toomap every key which is found in a sample data.</span></span> <span data-ttu-id="19333-175">Také ho můžete namapovat sloupce, které nejsou součástí hello ukázková data.</span><span class="sxs-lookup"><span data-stu-id="19333-175">It can also map columns which are not part of hello sample data.</span></span> 

## <a name="import-data"></a><span data-ttu-id="19333-176">Import dat</span><span class="sxs-lookup"><span data-stu-id="19333-176">Import data</span></span>

<span data-ttu-id="19333-177">tooimport data, nahrajte ho tooAzure úložiště, vytvořte přístupový klíč pro něj a potom proveďte volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="19333-177">tooimport data, you upload it tooAzure storage, create an access key for it, and then make a REST API call.</span></span>

![Přidat nový zdroj dat](./media/app-insights-analytics-import/analytics-upload-process.png)

<span data-ttu-id="19333-179">Můžete provádět následující proces ručně hello nebo nastavení automatizované systémové toodo ho v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="19333-179">You can perform hello following process manually, or set up an automated system toodo it at regular intervals.</span></span> <span data-ttu-id="19333-180">Je nutné toofollow tyto kroky pro každý datový blok chcete tooimport.</span><span class="sxs-lookup"><span data-stu-id="19333-180">You need toofollow these steps for each block of data you want tooimport.</span></span>

1. <span data-ttu-id="19333-181">Nahrání dat hello příliš[úložiště objektů blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="19333-181">Upload hello data too[Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> 

 * <span data-ttu-id="19333-182">Objekty BLOB může mít libovolnou velikost až too1GB nekomprimovaným.</span><span class="sxs-lookup"><span data-stu-id="19333-182">Blobs can be any size up too1GB uncompressed.</span></span> <span data-ttu-id="19333-183">Velké objekty BLOB stovek MB jsou ideální z hlediska výkonu.</span><span class="sxs-lookup"><span data-stu-id="19333-183">Large blobs of hundreds of MB are ideal from a performance perspective.</span></span>
 * <span data-ttu-id="19333-184">Můžete je komprimovat doba nahrávání tooimprove Gzip a latenci pro toobe data hello k dispozici pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="19333-184">You can compress it with Gzip tooimprove upload time and latency for hello data toobe available for query.</span></span> <span data-ttu-id="19333-185">Použití hello `.gz` příponu názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="19333-185">Use hello `.gz` filename extension.</span></span>
 * <span data-ttu-id="19333-186">Nejlepší toouse účet samostatného úložiště je pro tento účel tooavoid volání z jiné služby zpomalení výkonu.</span><span class="sxs-lookup"><span data-stu-id="19333-186">It's best toouse a separate storage account for this purpose, tooavoid calls from different services slowing performance.</span></span>
 * <span data-ttu-id="19333-187">Při odesílání dat v vysoká frekvence, každých několik sekund, se doporučuje toouse více než jeden účet úložiště, z důvodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="19333-187">When sending data in high frequency, every few seconds, it is recommended toouse more than one storage account, for performance reasons.</span></span>

 
2. <span data-ttu-id="19333-188">[Vytvoření klíče sdíleného přístupového podpisu pro objekt blob hello](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="19333-188">[Create a Shared Access Signature key for hello blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span></span> <span data-ttu-id="19333-189">Hello klíč by měl mít jeden den po dobu vypršení platnosti a poskytovat přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="19333-189">hello key should have an expiration period of one day and provide read access.</span></span>
3. <span data-ttu-id="19333-190">Zkontrolujte toonotify volání REST Application Insights, čekající data.</span><span class="sxs-lookup"><span data-stu-id="19333-190">Make a REST call toonotify Application Insights that data is waiting.</span></span>

 * <span data-ttu-id="19333-191">Koncový bod:`https://dc.services.visualstudio.com/v2/track`</span><span class="sxs-lookup"><span data-stu-id="19333-191">Endpoint: `https://dc.services.visualstudio.com/v2/track`</span></span>
 * <span data-ttu-id="19333-192">Metoda HTTP: POST</span><span class="sxs-lookup"><span data-stu-id="19333-192">HTTP method: POST</span></span>
 * <span data-ttu-id="19333-193">Datové části:</span><span class="sxs-lookup"><span data-stu-id="19333-193">Payload:</span></span>

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

<span data-ttu-id="19333-194">Hello zástupné znaky jsou:</span><span class="sxs-lookup"><span data-stu-id="19333-194">hello placeholders are:</span></span>

* <span data-ttu-id="19333-195">`Blob URI with Shared Access Key`: Zobrazí to hello postupu pro vytváření klíčů.</span><span class="sxs-lookup"><span data-stu-id="19333-195">`Blob URI with Shared Access Key`: You get this from hello procedure for creating a key.</span></span> <span data-ttu-id="19333-196">Je konkrétní toohello objektů blob.</span><span class="sxs-lookup"><span data-stu-id="19333-196">It is specific toohello blob.</span></span>
* <span data-ttu-id="19333-197">`Schema ID`: ID schéma definované schéma vygenerované hello.</span><span class="sxs-lookup"><span data-stu-id="19333-197">`Schema ID`: hello schema ID generated for your defined schema.</span></span> <span data-ttu-id="19333-198">Hello dat v této objektu blob musí být v souladu toohello schématu.</span><span class="sxs-lookup"><span data-stu-id="19333-198">hello data in this blob should conform toohello schema.</span></span>
* <span data-ttu-id="19333-199">`DateTime`: hello čas na které hello je odeslána žádost, UTC.</span><span class="sxs-lookup"><span data-stu-id="19333-199">`DateTime`: hello time at which hello request is submitted, UTC.</span></span> <span data-ttu-id="19333-200">Můžeme přijmout tyto formáty: ISO8601 (jako je "2016-01-01 13:45:01"); Rfc822 ("ST, 14 16 DEC – 14:57:01 + 0000"); RFC850 ("středa, 14. prosince 16 UTC 14:57:00"); RFC1123 ("st 14 Dec 2016 14:57:00 + 0000").</span><span class="sxs-lookup"><span data-stu-id="19333-200">We accept these formats: ISO8601 (like "2016-01-01 13:45:01"); RFC822 ("Wed, 14 Dec 16 14:57:01 +0000"); RFC850 ("Wednesday, 14-Dec-16 14:57:00 UTC"); RFC1123 ("Wed, 14 Dec 2016 14:57:00 +0000").</span></span>
* <span data-ttu-id="19333-201">`Instrumentation key`z prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="19333-201">`Instrumentation key` of your Application Insights resource.</span></span>

<span data-ttu-id="19333-202">Hello dat je k dispozici v Analytics po několika minutách.</span><span class="sxs-lookup"><span data-stu-id="19333-202">hello data is available in Analytics after a few minutes.</span></span>

## <a name="error-responses"></a><span data-ttu-id="19333-203">Chybové odpovědi</span><span class="sxs-lookup"><span data-stu-id="19333-203">Error responses</span></span>

* <span data-ttu-id="19333-204">**požadavek je 400 nesprávný**: Určuje, že datová část požadavku hello je neplatný.</span><span class="sxs-lookup"><span data-stu-id="19333-204">**400 bad request**: indicates that hello request payload is invalid.</span></span> <span data-ttu-id="19333-205">Kontrola:</span><span class="sxs-lookup"><span data-stu-id="19333-205">Check:</span></span>
 * <span data-ttu-id="19333-206">Klíč instrumentace správné.</span><span class="sxs-lookup"><span data-stu-id="19333-206">Correct instrumentation key.</span></span>
 * <span data-ttu-id="19333-207">Hodnota doby platnosti.</span><span class="sxs-lookup"><span data-stu-id="19333-207">Valid time value.</span></span> <span data-ttu-id="19333-208">Mělo by být hello čas nyní ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="19333-208">It should be hello time now in UTC.</span></span>
 * <span data-ttu-id="19333-209">JSON hello události vyhovuje toohello schématu.</span><span class="sxs-lookup"><span data-stu-id="19333-209">JSON of hello event conforms toohello schema.</span></span>
* <span data-ttu-id="19333-210">**403 Zakázáno**: hello blob, které jste odeslali není dostupný.</span><span class="sxs-lookup"><span data-stu-id="19333-210">**403 Forbidden**: hello blob you've sent is not accessible.</span></span> <span data-ttu-id="19333-211">Ujistěte se, že hello sdílený přístupový klíč je platný a jestli nevypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="19333-211">Make sure that hello shared access key is valid and has not expired.</span></span>
* <span data-ttu-id="19333-212">**404 nebyl nalezen**:</span><span class="sxs-lookup"><span data-stu-id="19333-212">**404 Not Found**:</span></span>
 * <span data-ttu-id="19333-213">Hello blob neexistuje.</span><span class="sxs-lookup"><span data-stu-id="19333-213">hello blob doesn't exist.</span></span>
 * <span data-ttu-id="19333-214">Hello sourceId je nesprávná.</span><span class="sxs-lookup"><span data-stu-id="19333-214">hello sourceId is wrong.</span></span>

<span data-ttu-id="19333-215">Podrobnější informace jsou k dispozici v hello odpovědi chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="19333-215">More detailed information is available in hello response error message.</span></span>


## <a name="sample-code"></a><span data-ttu-id="19333-216">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="19333-216">Sample code</span></span>

<span data-ttu-id="19333-217">Tento kód používá hello [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="19333-217">This code uses hello [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet package.</span></span>

### <a name="classes"></a><span data-ttu-id="19333-218">Třídy</span><span class="sxs-lookup"><span data-stu-id="19333-218">Classes</span></span>

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

### <a name="ingest-data"></a><span data-ttu-id="19333-219">Příjem dat</span><span class="sxs-lookup"><span data-stu-id="19333-219">Ingest data</span></span>

<span data-ttu-id="19333-220">Pomocí tohoto kódu pro každý objekt blob.</span><span class="sxs-lookup"><span data-stu-id="19333-220">Use this code for each blob.</span></span> 

```C#
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a><span data-ttu-id="19333-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19333-221">Next steps</span></span>

* [<span data-ttu-id="19333-222">Prohlídka hello dotazovacího jazyka pro analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="19333-222">Tour of hello Log Analytics query language</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="19333-223">Použití *Logstash* toosend data tooApplication statistiky</span><span class="sxs-lookup"><span data-stu-id="19333-223">Use *Logstash* toosend data tooApplication Insights</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
