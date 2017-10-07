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
# <a name="import-data-into-analytics"></a>Importovat data do Analytics

Importovat žádná tabulková data do [Analytics](app-insights-analytics.md), buď toojoin její [Application Insights](app-insights-overview.md) telemetrie z vaší aplikace, nebo tak, aby je bylo možné analyzovat jako samostatné datový proud. Analytics je účinný dotazovací jazyk vhodným tooanalyzing označen časovým razítkem vysoký počet datových proudů telemetrie.

Data můžete importovat do analýzy pomocí vlastního schématu. Nemá toouse hello standardní Application Insights schémata například požadavek nebo trasování.

Můžete importovat JSON nebo DSV (oddělovač oddělovači - čárku, středník nebo kartě) soubory.

Existují tři situacích výhodné import tooAnalytics:

* **Připojte se telemetrie aplikace.** Mohli byste například importovat tabulku, která mapuje adresy URL z vašeho webu toomore čitelný titulů. V analýzy můžete vytvořit sestavu grafu řídicího panelu, která obsahuje hello deset nejoblíbenější stránky ve vašem webu. Nyní ji můžete zobrazit hello místo adresy URL hello.
* **Korelovat telemetrii aplikace** s jinými zdroji například síťových přenosů dat serveru nebo CDN soubory protokolu.
* **Použijte Analytics tooa samostatné datového proudu.** Application Insights Analytics je výkonný nástroj, který pracuje s zhuštěné, označen časovým razítkem datové proudy - lépe než SQL v mnoha případech. Pokud máte datový proud z z jiného zdroje, můžete ho s Analytics analyzovat.

Odesílání zdroj dat datového tooyour je snadné. 

1. (Jednou) Definujte hello schématu dat v datovém zdroji.
2. (Pravidelně) Nahrání dat tooAzure úložiště a volání rozhraní REST API toonotify hello nám, které nová data se čeká na přijímání. Během několika minut je k dispozici pro dotaz v analýzy dat hello.

Hello frekvence odesílání hello je definované uživatelem a jak rychle chcete toobe vaše data, k dispozici pro dotazy. Je efektivnější tooupload data v větší bloky dat, ale není větší než 1GB.

> [!NOTE]
> *Máte spoustu tooanalyze zdroje dat?* [*Zvažte použití* logstash *tooship dat do služby Application Insights.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Než začnete

Budete potřebovat:

1. Prostředek Application Insights v Microsoft Azure.

 * Pokud chcete tooanalyze data odděleně od jiných telemetrie [vytvořte nový prostředek Application Insights](app-insights-create-new-resource.md).
 * Pokud chcete propojení nebo porovnávání svá data pomocí telemetrie z aplikace, která je již nastavena pomocí služby Application Insights, můžete použít hello prostředků pro tuto aplikaci.
 * Přispěvatel nebo vlastníka přístup toothat prostředek.
 
2. Úložiště Azure. Nahrát tooAzure úložiště a analýzy získá vaše data z ní. 

 * Doporučujeme že vytvořit účet vyhrazeného úložiště pro objektů BLOB. Pokud objektů BLOB jsou sdíleny s jinými procesy, trvá déle pro naše tooread procesy objektů BLOB.


## <a name="define-your-schema"></a>Definování schématu

Aby bylo možné naimportovat data, je nutné definovat *zdroje dat,* který určuje hello schématu dat.
Může mít až too50 zdroje dat v jediném prostředku Application Insights

1. Spuštění Průvodce zdrojem dat hello. Použijte tlačítko "Přidat nový zdroj dat". Případně – klikněte na tlačítko nastavení v pravém horním rohu a v rozevírací nabídce vyberte "Zdroje dat".

    ![Přidat nový zdroj dat](./media/app-insights-analytics-import/add-new-data-source.png)

    Zadejte název nového zdroje dat.

2. Definování formátu hello souborů, které odešlete.

    Můžete definovat hello formátu ručně, nebo odeslání ukázkového souboru.

    Pokud hello data jsou ve formátu CSV, může být první řádek hello hello ukázky záhlaví sloupců. Můžete změnit názvy polí hello v dalším kroku hello.

    Ukázka Hello by měla obsahovat alespoň 10 řádků nebo záznamů dat s.

    Sloupec nebo pole názvy by měly mít alfanumerické názvy (bez mezer a interpunkce).

    ![Odeslání ukázkového souboru](./media/app-insights-analytics-import/sample-data-file.png)


3. Je tu zkontrolujte hello schématu, které hello průvodce. Pokud ho odvodit hello typů z ukázku, bude pravděpodobně nutné tooadjust hello odvozené typy sloupců hello.

    ![Zkontrolujte hello odvodit schématu](./media/app-insights-analytics-import/data-source-review-schema.png)

 * (Volitelné.) Nahrajte definici schématu. Viz níže uvedeným formátem hello.

 * Vyberte časové razítko. Všechna data v Analytics musí mít pole časového razítka. Musí mít typ `datetime`, ale nemá toobe s názvem 'časové razítko'. Pokud data obsahují sloupec obsahující datum a čas ve formátu ISO, zvolte to jako sloupec časového razítka hello. Jinak, vyberte "jako data doručen" a proces importu hello přidá pole časového razítka.

5. Vytvoření zdroje dat hello.

### <a name="schema-definition-file-format"></a>Formát souboru definice schématu

Neupravujte hello schématu v uživatelském rozhraní, můžete načíst definice schématu hello ze souboru. Formát definice schématu Hello vypadá takto: 

Formát s oddělovačem 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

Formát JSON 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
Každý sloupec je identifikována hello umístění, název a typ. 

* Umístění – souboru s oddělovači naformátujte ho je pozice hello hello namapované hodnoty. Pro formát JSON je hello jpath hello namapované klíče.
* Název – hello zobrazí název sloupce hello.
* Typ – hello datový typ sloupce.
 
V případě, že byl použit ukázková data a jsou odděleny formát souboru, musí definice schématu hello mapovat všechny sloupce a přidejte nové sloupce na konci hello. 

JSON umožňuje částečné mapování hello dat, proto hello definice schématu formátu JSON nemá toomap každým klíčem, který se nachází v ukázková data. Také ho můžete namapovat sloupce, které nejsou součástí hello ukázková data. 

## <a name="import-data"></a>Import dat

tooimport data, nahrajte ho tooAzure úložiště, vytvořte přístupový klíč pro něj a potom proveďte volání rozhraní REST API.

![Přidat nový zdroj dat](./media/app-insights-analytics-import/analytics-upload-process.png)

Můžete provádět následující proces ručně hello nebo nastavení automatizované systémové toodo ho v pravidelných intervalech. Je nutné toofollow tyto kroky pro každý datový blok chcete tooimport.

1. Nahrání dat hello příliš[úložiště objektů blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md). 

 * Objekty BLOB může mít libovolnou velikost až too1GB nekomprimovaným. Velké objekty BLOB stovek MB jsou ideální z hlediska výkonu.
 * Můžete je komprimovat doba nahrávání tooimprove Gzip a latenci pro toobe data hello k dispozici pro dotaz. Použití hello `.gz` příponu názvu souboru.
 * Nejlepší toouse účet samostatného úložiště je pro tento účel tooavoid volání z jiné služby zpomalení výkonu.
 * Při odesílání dat v vysoká frekvence, každých několik sekund, se doporučuje toouse více než jeden účet úložiště, z důvodů výkonu.

 
2. [Vytvoření klíče sdíleného přístupového podpisu pro objekt blob hello](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). Hello klíč by měl mít jeden den po dobu vypršení platnosti a poskytovat přístup pro čtení.
3. Zkontrolujte toonotify volání REST Application Insights, čekající data.

 * Koncový bod:`https://dc.services.visualstudio.com/v2/track`
 * Metoda HTTP: POST
 * Datové části:

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

Hello zástupné znaky jsou:

* `Blob URI with Shared Access Key`: Zobrazí to hello postupu pro vytváření klíčů. Je konkrétní toohello objektů blob.
* `Schema ID`: ID schéma definované schéma vygenerované hello. Hello dat v této objektu blob musí být v souladu toohello schématu.
* `DateTime`: hello čas na které hello je odeslána žádost, UTC. Můžeme přijmout tyto formáty: ISO8601 (jako je "2016-01-01 13:45:01"); Rfc822 ("ST, 14 16 DEC – 14:57:01 + 0000"); RFC850 ("středa, 14. prosince 16 UTC 14:57:00"); RFC1123 ("st 14 Dec 2016 14:57:00 + 0000").
* `Instrumentation key`z prostředku Application Insights.

Hello dat je k dispozici v Analytics po několika minutách.

## <a name="error-responses"></a>Chybové odpovědi

* **požadavek je 400 nesprávný**: Určuje, že datová část požadavku hello je neplatný. Kontrola:
 * Klíč instrumentace správné.
 * Hodnota doby platnosti. Mělo by být hello čas nyní ve standardu UTC.
 * JSON hello události vyhovuje toohello schématu.
* **403 Zakázáno**: hello blob, které jste odeslali není dostupný. Ujistěte se, že hello sdílený přístupový klíč je platný a jestli nevypršela platnost.
* **404 nebyl nalezen**:
 * Hello blob neexistuje.
 * Hello sourceId je nesprávná.

Podrobnější informace jsou k dispozici v hello odpovědi chybová zpráva.


## <a name="sample-code"></a>Ukázka kódu

Tento kód používá hello [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) balíček NuGet.

### <a name="classes"></a>Třídy

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

### <a name="ingest-data"></a>Příjem dat

Pomocí tohoto kódu pro každý objekt blob. 

```C#
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a>Další kroky

* [Prohlídka hello dotazovacího jazyka pro analýzy protokolů](app-insights-analytics-tour.md)
* [Použití *Logstash* toosend data tooApplication statistiky](https://github.com/Microsoft/logstash-output-application-insights)
