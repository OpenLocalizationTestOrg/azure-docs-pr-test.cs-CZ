---
title: "Rychlý start: Azure Machine Learning doporučení API (verze 1) | Microsoft Docs"
description: "Azure Machine Learning doporučení – úvodní příručka"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 5bce1a4a-1ad6-473f-812b-84f800fdc09a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 0a9d0b6aa1ef734a857ecc16777ba6250909b38d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="quick-start-guide-for-the-machine-learning-recommendations-api-version-1"></a>Úvodní příručka pro Machine Learning doporučení API (verze 1)

> [!NOTE]
> Měli byste začít používat [kognitivní služby API doporučení](https://www.microsoft.com/cognitive-services/recommendations-api) místo tuto verzi. Službu kognitivní doporučení budou nahrazení této služby, a všechny nové funkce bude vyvinutý existuje. Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.
>
> Další informace o [migraci na novou službu kognitivní](http://aka.ms/recomigrate).
> 
> 

Tento dokument popisuje, jak se budou registrovat službou nebo aplikací používat Microsoft Azure Machine Learning doporučení. Další informace naleznete na volání rozhraní API doporučení v [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="general-overview"></a>Obecné – přehled
Pokud chcete používat Azure Machine Learning doporučení, budete muset provést následující kroky:

* Vytvoření modelu – model je kontejner pro data o využití, data katalogu a model doporučení.
* Data katalogu import - katalog obsahuje informace o metadatech na položky. 
* Data o využití import - data o využití je možné uložit v jedné ze dvou způsobů (nebo obě):
  * Tím, že nahrajete soubor, který obsahuje data o využití.
  * Odesláním dat pořízení události.
    Obvykle nahrát soubor využití aby mohl vytvořit model počáteční doporučení (zavedení) a jeho použití, dokud systém shromažďuje dostatek dat pomocí formátu data pořízení.
* Vytvoření modelu doporučení – to je asynchronní operace, ve kterém systém doporučení trvá všech dat o využití a vytvoří model doporučení. Tato operace může trvat několik minut nebo i několik hodin v závislosti na velikosti dat a parametry konfigurace sestavení. Při spuštění sestavení, zobrazí se o ID sestavení. Použijte ho ke kontrole při ukončení procesu sestavení před zahájením využívat doporučení.
* Využívat doporučení - Get doporučení pro určitou položku nebo seznam položek.

Všechny výše uvedené kroky se provádějí pomocí rozhraní API služby Azure Machine Learning doporučení.  Si můžete stáhnout ukázkovou aplikaci, která implementuje každý z těchto kroků z [také galerie.](http://1drv.ms/1xeO2F3)

## <a name="limitations"></a>Omezení
* Maximální počet modelů podle předplatného je 10.
* Maximální počet položek, které mohou být uloženy katalog je 100 000.
* Maximální počet bodů využití, které jsou zachovány je ~ 5 000 000. Nejstarší budou odstraněna, pokud nové se nahrál nebo nahlásí.
* Maximální velikost dat, který může odeslat v BLOGU (například import katalogu dat, data o využití import) je 200MB.
* Počet transakcí za sekundu pro sestavení modelu doporučení, která není aktivní je ~ 2TPS. Sestavení modelu doporučení, která je aktivní mohou být uloženy do 20TPS.

## <a name="integration"></a>Integrace
### <a name="authentication"></a>Authentication
Microsoft Azure Marketplace podporuje základní nebo OAuth metodu ověřování. Klíče účtu budete moci snadno najít tak, že přejdete na klíče ve marketplace v oddíle [nastavení svého účtu](https://datamarket.azure.com/account/keys). 

#### <a name="basic-authentication"></a>Základní ověřování
Přidáte hlavičku autorizace:

    Authorization: Basic <creds>

    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  

Převést na Base64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);

Převést na Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);




### <a name="service-uri"></a>URI služby
Kořenový adresář identifikátor URI pro rozhraní API služby Azure Machine Learning doporučení je [sem.](https://api.datamarket.azure.com/amla/recommendations/v2/)

Službu úplný identifikátor URI je vyjádřit pomocí elementů specifikace prostředí OData.

### <a name="api-version"></a>Verze rozhraní API
Každé volání rozhraní API bude mít na konci, parametr dotazu s názvem apiVersion, který musí být nastavena na "1.0".

### <a name="ids-are-case-sensitive"></a>ID jsou malá a velká písmena
ID, podle rozhraní API, vrátil malých a velkých písmen a by měl být použit jako takový, při předány jako parametry při následných voláních rozhraní API. ID modelu a ID katalogu pro instanci, jsou malá a velká písmena.

### <a name="create-a-model"></a>Vytvoření modelu
Vytváří se žádost o "Vytvoření modelu":

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelName/ |Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.<br>Maximální délka: 20 |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

* `feed/entry/content/properties/id`-Obsahuje ID modelu.
  Všimněte si, že ID modelu je malá a velká písmena.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-catalog-data"></a>Umožňuje importovat catalog data
Pokud nahrajete několik katalog souborů do stejného modelu s několika volání, jsme vloží nové položky katalogu. Existující položky zůstane s původními hodnotami.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu (malá a velká písmena) |
| Název souboru |Textové identifikátor katalogu.<br>Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.<br>Maximální délka: 50 |
| apiVersion |1.0 |
|  | |
| Text žádosti |Data katalogu. Formát:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Name (Název)</th><th>Povinné</th><th>Typ</th><th>Popis</th></tr><tr><td>Id položky</td><td>Ano</td><td>Alfanumerické znaky, maximální délka 50</td><td>Jedinečný identifikátor položky</td></tr><tr><td>Název položky</td><td>Ano</td><td>Alfanumerické znaky, maximální délka 255</td><td>Název položky</td></tr><tr><td>Kategorie položky</td><td>Ano</td><td>Alfanumerické znaky, maximální délka 255</td><td>Kategorie, do které patří tato položka (například vaření knihy, obrázkům...)</td></tr><tr><td>Popis</td><td>Ne</td><td>Alfanumerické znaky, maximální délka je 4000</td><td>Popis této položky</td></tr></table><br>Maximální velikost souboru je 200MB.<br><br>Příklad:<br><code>2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book<br>21bf8088-b6c0-4509-870c-e1c7ac78304a,The Forgetting Room: A Fiction (Byzantium Book),Book<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book<br>552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book</code> |

**Odpověď**:

Kód stavu HTTP: 200

* `Feed\entry\content\properties\LineCount`-Přijaté počet řádků.
* `Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
          <subtitle type="text">Import catalog file</subtitle>
          <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:58:04Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-usage-data"></a>Importovat data o využití
#### <a name="uploading-a-file"></a>Nahrání souboru
V této části ukazuje, jak odeslat data o využití pomocí souboru. Můžete volat toto rozhraní API se data o využití. Všechna data o využití se uloží pro všechna volání.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu (malá a velká písmena) |
| Název souboru |Textové identifikátor katalogu.<br>Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.<br>Maximální délka: 50 |
| apiVersion |1.0 |
|  | |
| Text žádosti |Data o využití. Formát:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Name (Název)</th><th>Povinné</th><th>Typ</th><th>Popis</th></tr><tr><td>Id uživatele</td><td>Ano</td><td>Alfanumerické</td><td>Jedinečný identifikátor uživatele</td></tr><tr><td>Id položky</td><td>Ano</td><td>Alfanumerické znaky, maximální délka 50</td><td>Jedinečný identifikátor položky</td></tr><tr><td>Čas</td><td>Ne</td><td>Datum ve formátu: rrrr/MM/ddTHH (například 2013/06/20T10:00:00)</td><td>Čas dat</td></tr><tr><td>Událost</td><td>Ne, pokud zadaný musí taky datum</td><td>Jeden z následujících:<br>• Klikněte na tlačítko<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Nákupu</td><td></td></tr></table><br>Maximální velikost souboru je 200MB.<br><br>Příklad:<br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Odpověď**:

Kód stavu HTTP: 200

* `Feed\entry\content\properties\LineCount`-Přijaté počet řádků.
* `Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby.
* `Feed\entry\content\properties\FileId`-Souboru identifikátor.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="using-data-acquisition"></a>Pomocí získávání dat
V této části ukazuje, jak k odesílání událostí v reálném čase na Azure Machine Learning doporučení, obvykle z vašeho webu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| Text žádosti |Vkládání dat události pro všechny události, který chcete poslat. Byste měli odeslat pro stejné relace uživatele nebo prohlížeče stejné ID v poli ID relace. (Viz ukázka textu události, které jsou níže). |

* Příklad pro události, klikněte na tlačítko'.
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>
* Příklad pro událost 'RecommendationClick':
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RecommendationClick</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* Příklad pro událost 'AddShopCart':
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* Příklad pro událost 'RemoveShopCart':
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RemoveShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* Příklad pro události, nákupu'.
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>
* Příklad odesílání 2 události, klikněte na' a 'AddShopCart':
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>Click</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
          <ItemName>itemName</ItemName>
          <ItemDescription>item description</ItemDescription>
          <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
          </EventData>
        </Event>

**Odpověď**: kód stavu HTTP: 200

### <a name="build-a-recommendation-model"></a>Vytvoření modelu doporučení
| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu (malá a velká písmena) |
| userDescription |Textové identifikátor katalogu. Poznámka: Pokud použijete prostory musí zakódovat ho s % 20 místo. Najdete v předchozím příkladu.<br>Maximální délka: 50 |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Toto je asynchronní rozhraní API. Zobrazí se ID sestavení jako odpověď. Vědět, pokud sestavení skončila, by měly volat rozhraní API "Získat sestavení stavu systému Model" a vyhledejte toto ID sestavení v odpovědi. Všimněte si, že sestavení může trvat minut až hodin v závislosti na velikosti dat.

Nemůžete spotřebovat doporučení do sestavení ukončí.

Stav platný sestavení:

* Vytvoření – Model byl vytvořen.
* Ve frontě – sestavení modelu byla spuštěna a je ve frontě.
* Sestavení – Model sestavuje.
* Úspěch – sestavení skončila úspěšně.
* Chyba – sestavení skončila s chybou.
* Došlo ke zrušení – sestavení bylo zrušeno.
* Zrušení – sestavení probíhá její zrušení.

Všimněte si, že ID sestavení naleznete v následující cestě:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="get-build-status-of-a-model"></a>Získat stav sestavení modelu
| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu (malá a velká písmena) |
| onlyLastBuild |Označuje, zda vrátit veškerá historie sestavení modelu nebo pouze stav poslední sestavení. |
| apiVersion |1.0 |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za sestavení. Každá položka má následující data:

* `feed/entry/content/properties/UserName`– Název uživatele.
* `feed/entry/content/properties/ModelName`– Název modelu.
* `feed/entry/content/properties/ModelId`– Model jedinečný identifikátor.
* `feed/entry/content/properties/IsDeployed`– Jestli (také známa jako nasazení sestavení aktivní sestavení).
* `feed/entry/content/properties/BuildId`– Vytvořte jedinečný identifikátor.
* `feed/entry/content/properties/BuildType`-Typ sestavení.
* `feed/entry/content/properties/Status`– Stav sestavení. Může být jedna z následujících akcí: Chyba, sestavování, zařazeno do fronty, zrušení, zrušení, úspěch
* `feed/entry/content/properties/StatusMessage`– Zpráva podrobné informace o stavu (platí pouze pro konkrétní stavy).
* `feed/entry/content/properties/Progress`– Sestavení průběh (%).
* `feed/entry/content/properties/StartTime`– Sestavení čas spuštění.
* `feed/entry/content/properties/EndTime`– Sestavení koncový čas.
* `feed/entry/content/properties/ExecutionTime`– Sestavení doba trvání.
* `feed/entry/content/properties/ProgressStep`– Podrobnosti o aktuální fázi, který je v průběhu sestavení v.

Stav platný sestavení:

* Vytvořeno – položka sestavení žádost byla vytvořena.
* Ve frontě – sestavení požadavku byla spuštěna a je ve frontě.
* Probíhá vytváření – sestavení.
* Úspěch – sestavení skončila úspěšně.
* Chyba – sestavení skončila s chybou.
* Došlo ke zrušení – sestavení bylo zrušeno.
* Zrušení – sestavení probíhá její zrušení.

Platné hodnoty pro typ sestavení:

* Pořadí - pořadí sestavení. (Pro pořadí sestavení podrobnosti naleznete v dokumentu "Doporučení Machine Learning API dokumentace".)
* Doporučení - doporučení sestavení.
* Společně fbt - často koupila sestavení.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


### <a name="get-recommendations"></a>Získejte doporučení
| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu (malá a velká písmena) |
| položky ItemID |Textový soubor s oddělovači seznam položek, doporučujeme pro.<br>Maximální délka: 1024 |
| numberOfResults |Počet požadovaných výsledků |
| includeMetatadata |Budoucí použití, vždy hodnotu false. |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za doporučené položky. Každá položka má následující data:

* `Feed\entry\content\properties\Id`– ID Doporučené položky.
* `Feed\entry\content\properties\Name`-Název položky.
* `Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.
* `Feed\entry\content\properties\Reasoning`-Doporučení reasoning (například vysvětlení doporučení).

OData XML

Následující příklad odpověď obsahuje 10 doporučené položek:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="update-model"></a>Aktualizace modelu
Můžete aktualizovat popis modelu nebo ID aktivního sestavení.
*ID aktivního sestavení* -každé sestavení pro každý model má sestavení ID. ID aktivního sestavení je prvním úspěšném sestavení každý nový model. Jakmile máte ID aktivního sestavení a proveďte další sestavení pro stejný model, je nutné explicitně nastavit jako výchozí ID sestavení Pokud chcete. Když budete používat doporučení, pokud nezadáte ID sestavení, které chcete použít, výchozí nastavení se použije automaticky.

Tento mechanismus umožňuje – až budete mít model doporučení v produkčním prostředí - pro vytvoření nových modelů a testování je před zvýšením úrovně je do produkčního prostředí.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| PUT |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| id |Jedinečný identifikátor modelu (malá a velká písmena) |
| apiVersion |1.0 |
|  | |
| Text žádosti |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Všimněte si, že značky XML, popis a ActiveBuildId jsou volitelné. Pokud nechcete nastavit popis nebo ActiveBuildId, odeberte celý značky. |

**Odpověď**:

Kód stavu HTTP: 200

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Update an Existing Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T10:27:17Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

## <a name="legal"></a>Právní informace
Tento dokument je poskytován "jako-je". Informace a názory vyjádřené v tomto dokumentu včetně adres URL a dalších odkazů na internetové weby mohou změnit bez předchozího upozornění. Některé příklady použité v ukázkách jsou jenom ilustrativní a smyšlené. Žádný skutečný vztah nebo připojení je určený nebo událostmi. Tento dokument neposkytuje jste žádná zákonná práva duševního vlastnictví produktů společnosti Microsoft. Můžete kopírovat a tento dokument použít pro interní referenční účely. © 2014 Microsoft. Všechna práva vyhrazena. 

