---
title: "aaaHow tooencode prostředek Azure pomocí kodéru Media Encoder Standard | Microsoft Docs"
description: "Zjistěte, jak toouse Media Encoder Standard tooencode média obsahu na službu Azure Media Services. Ukázky kódu pomocí rozhraní REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: b766bafded7ee98eda3e6ef149c31d5d8fe406fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencode-an-asset-by-using-media-encoder-standard"></a>Jak tooencode assetu pomocí kodéru Media Encoder Standard
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [REST](media-services-rest-encode-asset.md)
> * [Azure Portal](media-services-portal-encode.md)
>
>

## <a name="overview"></a>Přehled
toodeliver digitální video přes hello Internetu, musí komprese hello médií. Digitální video soubory jsou velké, může být příliš velký toodeliver prostřednictvím hello Internet nebo pro vaše zákazníky zařízení toodisplay správně. Kódování je proces hello komprimace videa a zvuku, takže vaši zákazníci mohou zobrazit médiu.

Kódování úlohy jsou některé z nejběžnějších operací zpracování hello ve službě Azure Media Services. Z jednoho kódování tooanother vytvoříte kódování úlohy tooconvert mediálních souborů. Při kódování, můžete použít hello Media Services předdefinované kodér (Media Encoder Standard). Můžete také použít kodér poskytovanými partnerem Media Services. Jsou k dispozici prostřednictvím Azure Marketplace hello kodéry třetích stran. Můžete zadat podrobnosti hello úlohy kódování, pomocí přednastavení definované pro kodér nebo pomocí přednastavené konfigurační soubory. typy hello toosee přednastavení, které jsou k dispozici, najdete v části [přednastavení úloh pro Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).

Každá úloha může mít jeden nebo více úloh v závislosti na typu hello zpracování, které chcete tooaccomplish. Prostřednictvím hello REST API můžete vytvořit úlohy a jejich související úlohy v jedné ze dvou způsobů:

* Úlohy může být definována vložením prostřednictvím hello úlohy navigační vlastnost u entity úlohy.
* Použijte dávkovým zpracováním OData.

Doporučujeme vždy zakódovat zdrojové soubory do sady souborů MP4 adaptivní přenosovou rychlostí a pak převést hello sadu toohello požadovaný formát pomocí [dynamické balení](media-services-dynamic-packaging-overview.md).

Pokud výstupní asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu hello. Další informace najdete v tématu [konfigurace zásad doručení assetu](media-services-rest-configure-asset-delivery-policy.md).

## <a name="considerations"></a>Požadavky

Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP. Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).

Než začnete, odkazující na procesory médií, ověřte, že mají hello ID správná média procesoru. Další informace najdete v tématu [získat procesory médií](media-services-rest-get-media-processor.md).

## <a name="connect-toomedia-services"></a>Připojení služby tooMedia

Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services. Je nutné provést následující volání toohello nový identifikátor URI.

## <a name="create-a-job-with-a-single-encoding-task"></a>Vytvoření úlohy pomocí jednoho úkolu kódování
> [!NOTE]
> Při práci s hello Media Services REST API, hello platí následující aspekty:
>
> Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP. Další informace najdete v tématu [nastavení pro vývoj pro Media Services REST API](media-services-rest-how-to-use.md).
>
> Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services. Je nutné provést následující volání toohello nový identifikátor URI. Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).
>
> Při použití formátu JSON a zadání toouse hello **__metadata** – klíčové slovo v požadavku hello (například tooreferences propojeného objektu), je nutné nastavit hello **přijmout** záhlaví příliš[JSON podrobný formát ](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Přijměte: application/json; odata = verbose.
>
>

Hello následující ukázka ukazuje, jak toocreate a post úlohu s jeden úkol nastavit tooencode video na konkrétní řešení a kvality. Při kódování pomocí procesoru Media Encoder Standard, můžete použít přednastavení úloh konfigurace zadané [zde](http://msdn.microsoft.com/library/mt269960).

Žádost:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Odpověď:

    HTTP/1.1 201 Created

    . . .

### <a name="set-hello-output-assets-name"></a>Nastavte název prostředku výstup hello
Hello následující příklad ukazuje, jak tooset hello assetName atribut:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a>Požadavky
* Taskbody – vlastnosti musí používat literál XML toodefine hello počet vstup nebo výstup prostředky, které jsou používány hello úloh. Hello úloh téma obsahuje hello definice schématu XML pro hello XML.
* V hello taskbody – definice, každý vnitřní hodnota <inputAsset> a <outputAsset> musí být nastavena jako JobInputAsset(value) nebo JobOutputAsset(value).
* Úloha může mít více prostředků výstup. Jeden JobOutputAsset(x) lze použít jako výstup úlohy pro úlohu pouze jednou.
* Můžete zadat JobInputAsset nebo JobOutputAsset jako vstupní datový zdroj úlohy.
* Úlohy nesmí tvoří cyklus.
* Parametr Hodnota Hello předáte tooJobInputAsset nebo JobOutputAsset představuje hodnotu hello index pro určitý prostředek. Hello skutečné prostředky jsou definovány v hello InputMediaAssets a OutputMediaAssets navigačních vlastností v definici entity hello úlohy.
* Služba Media Services je integrovaná v OData v3, hello jednotlivé prostředky v hello InputMediaAssets a OutputMediaAssets navigační vlastnost kolekce se odkazuje prostřednictvím "__metadata: identifikátor uri" dvojice název hodnota.
* InputMediaAssets mapuje tooone nebo další prostředky, které jste vytvořili ve službě Media Services. OutputMediaAssets jsou vytvořené pomocí systému hello. Nemusíte se odkazovat na existující prostředek.
* OutputMediaAssets může mít název pomocí atributu assetName hello. Pokud tento atribut není k dispozici, je název hello hello OutputMediaAsset libovolnou hodnotu vnitřní text hello hello <outputAsset> element je s příponou hello název úlohy hodnotu nebo hodnotu Id úlohy hello (v případě hello, kde není definován název vlastnosti hello). Například pokud nastavíte hodnotu assetName příliš "Ukázkový", pak hello OutputMediaAsset název je nastavena příliš "ukázkové." Pokud nebylo nastavit hodnotu assetName, ale nastavit název úlohy hello příliš "NewJob", pak hello název OutputMediaAsset by však bylo "_NewJob JobOutputAsset (hodnota)."

## <a name="create-a-job-with-chained-tasks"></a>Vytvořit úlohu zřetězené úlohy
V mnoha scénářích aplikace mají vývojáři toocreate řadu zpracování úlohy. Ve službě Media Services můžete vytvořit řadu zřetězené úlohy. Každý úkol provádí různé zpracování kroky a můžete použít různé média procesory. Hello zřetězené úlohy můžete z tooanother jeden úkol, provádění lineární pořadí úloh na hello asset přebírají prostředek. Hello úlohy prováděné v rámci úlohy však nejsou požadované toobe v pořadí. Když vytvoříte úlohu zřetězené, hello zřetězené **ITask** objekty jsou vytvořené v jedné **IJob** objektu.

> [!NOTE]
> Je aktuálně omezeno na 30 úloh na úlohu. Pokud potřebujete více než 30 úloh toochain, vytvořte více než jednu úlohu toocontain hello úlohy.
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


### <a name="considerations"></a>Požadavky
řetězení úloh tooenable:

* Úloha musí mít alespoň dvě úlohy.
* Musí existovat alespoň jeden úkol, jejichž vstup je výstup hello jiná úloha v úloze hello.

## <a name="use-odata-batch-processing"></a>Použít dávkovým zpracováním OData
Hello následující příklad ukazuje, jak toouse OData batch toocreate zpracování úlohy a úlohy. Informace o zpracování dávky, najdete v části [Open Data Protocol (OData) dávkové zpracování](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net


    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a>Vytvořit úlohu pomocí JobTemplate
Když při zpracování více prostředků pomocí společnou sadu úloh, pomocí úlohu JobTemplate toospecify hello výchozí přednastavení nebo tooset hello pořadí úkolů.

Hello následující příklad ukazuje, jak toocreate JobTemplate s TaskTemplate, který je definovanými v řádku. Hello TaskTemplate používá hello Media Encoder Standard jako hello MediaProcessor tooencode hello asset soubor. Další MediaProcessors však lze použít také.

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> Na rozdíl od jinými entitami Media Services musí definovat nový identifikátor GUID pro každou TaskTemplate a umístěte jej v hello taskTemplateId a vlastnost Id v textu vaší žádosti. schéma obsahu identifikace Hello musí následovat hello schéma popsaných v identifikovat Azure Media Services entity. Navíc JobTemplates nelze aktualizovat. Místo toho musíte vytvořit novou s aktualizované změny.
>
>

V případě úspěchu se vrátí hello následující odpověď:

    HTTP/1.1 201 Created

    . . .


Následující příklad ukazuje, jak Hello toocreate úlohu, která odkazuje na JobTemplate Id:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


V případě úspěchu se vrátí hello následující odpověď:

    HTTP/1.1 201 Created

    . . .



## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Další kroky
Teď, když víte, jak toocreate tooencode úlohy na prostředek, najdete v části [jak toocheck úlohy průběh pomocí služby Media Services](media-services-rest-check-job-progress.md).

## <a name="see-also"></a>Viz také
[Získat procesory médií](media-services-rest-get-media-processor.md)
