---
title: "Strojového učení dokumentace doporučení rozhraní API | Microsoft Docs"
description: "Dokumentace Azure Machine Learning doporučení API pro modul doporučení k dispozici na webu Microsoft Azure Marketplace."
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 32c3ab2f-fdd7-48cc-b501-ad55c79b87dc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: LuisCa
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 1fba64d78d779344e2895b0d54419186b7584865
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations-api-documentation"></a>Dokumentace k rozhraní Azure Machine Learning Recommendations API
> [!NOTE]
> Měli byste začít používat službu doporučení rozhraní API kognitivní místo tuto verzi. Službu kognitivní doporučení budou nahrazení této služby, a všechny nové funkce bude vyvinutý existuje. Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.
> Další informace o [migraci na novou službu kognitivní](http://aka.ms/recomigrate)
> 
> 

Tento dokument znázorňuje rozhraní API Microsoft Azure Machine Learning doporučení.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Obecné – přehled
Tento dokument je referenční dokumentace rozhraní API. Měli byste začít s dokumentu "Azure Machine Learning doporučení – rychlý Start".

Rozhraní API služby Azure Machine Learning doporučení je možné rozdělit do následujících logických skupin:

* <ins>Omezení</ins> -omezení rozhraní API doporučení.
* <ins>Obecné informace</ins> -informace o ověřování, služba URI a správu verzí.
* <ins>Model Basic</ins> – rozhraní API, které vám umožňují provádět základní operace v modelu (například vytvářet, aktualizovat a odstraňovat model).
* <ins>Model Upřesnit</ins> – rozhraní API, která vám umožní získat pokročilé statistiky dat na modelu.
* <ins>Model obchodní pravidla</ins> – rozhraní API, která vám umožní spravovat obchodní pravidla o výsledcích doporučení modelu.
* <ins>Katalog</ins> – rozhraní API, které vám umožňují provádět základní operace ve model katalogu. Katalog obsahuje informace o metadatech na položky dat o využití.
* <ins>Funkce</ins> – rozhraní API umožňujících získáte přehledy na položku do katalogu a jak tyto informace slouží k vytvoření lepší doporučení.
* <ins>Data o využití</ins> – rozhraní API, které vám umožňují provádět základní operace s daty využití modelu. Ve formuláři Základní data o využití se skládá z řádků, které zahrnují dvojici & č. 60; userId & č. 62; & č. 60; itemId & č. 62;.
* <ins>Sestavení</ins> – rozhraní API, která vám umožní aktivovat build modelu a provádět základní operace, které se vztahují na tento build. Až budete mít data o využití hodí v situaci, můžete aktivovat sestavení modelu.
* <ins>Doporučení</ins> – rozhraní API, která vám umožní využívat doporučení až po skončení sestavení modelu.
* <ins>Uživatelská Data</ins> – rozhraní API, která vám umožní načíst informace o uživatelských dat využití.
* <ins>Oznámení</ins> – rozhraní API, která vám umožní přijímat oznámení o problémech souvisejících s vaše operace rozhraní API. (Například zasíláte data o využití přes získávání dat a většiny událostí dochází k selhání zpracování. Oznámení o chybě bude vyvolána.)

## <a name="2-limitations"></a>2. Omezení
* Maximální počet modelů podle předplatného je 10.
* Maximální počet sestavení za modelu je 20.
* Maximální počet položek, které mohou být uloženy katalog je 100 000.
* Maximální počet bodů využití, které jsou zachovány je ~ 5 000 000. Nejstarší budou odstraněna, pokud nové se nahrál nebo nahlásí.
* Maximální velikost dat, který může odeslat v BLOGU (například import data katalogu, data o využití import) je 200MB.
* Maximální počet položek, které můžete být požádáni o při získávání doporučení je 150.

## <a name="3-apis---general-information"></a>3. Rozhraní API – obecné informace
### <a name="31-authentication"></a>3.1. Authentication
Postupujte podle pokynů Microsoft Azure Marketplace týkající se ověřování. Na webu marketplace podporuje základní nebo OAuth metodu ověřování.

### <a name="32-service-uri"></a>3.2. URI služby
Kořenový adresář identifikátor URI pro rozhraní API služby Azure Machine Learning doporučení je [sem.](https://api.datamarket.azure.com/amla/recommendations/v3/)

Službu úplný identifikátor URI je vyjádřit pomocí elementů specifikace prostředí OData.  

### <a name="33-api-version"></a>3.3. Verze rozhraní API
Každé volání rozhraní API bude mít na konci, parametr dotazu s názvem apiVersion, který musí být nastavena na 1.0.

### <a name="34-ids-are-case-sensitive"></a>3.4. ID jsou malá a velká písmena
ID, vrácený některé z rozhraní API se velká a malá písmena a by měl být použit jako takový, při předány jako parametry při následných voláních rozhraní API. ID modelu a ID katalogu pro instanci, jsou velká a malá písmena.

## <a name="4-recommendations-quality-and-cold-items"></a>4. Doporučení kvality a Cold položky
### <a name="41-recommendation-quality"></a>4.1. Doporučení kvality
Vytvoření modelu doporučení je obvykle dostatek, aby umožňovalo systému poskytnout doporučení. Nicméně kvality doporučení se může lišit podle využití zpracovat a pokrytí katalogu. Například pokud máte spoustu cold položky (položek bez významné využití), systém bude mít problémy poskytuje doporučení pro tyto položky nebo pomocí takové položky jako doporučenou jeden. Překonat problém cold položky systému dovoluje metadata položky, které chcete vylepšit doporučení. Tato metadata se označuje jako funkce. Typické funkce jsou autor knihy nebo video z objektu actor. Funkce poskytované prostřednictvím katalogu ve formě řetězce klíč/hodnota. Úplný formát souboru katalogu, naleznete [importovat část katalogu](#81-import-catalog-data). 

### <a name="42-rank-build"></a>4.2. RANK sestavení
Funkce můžete vylepšit modelu doporučení, ale k tomu vyžaduje použití smysluplný funkcí. Pro tento účel, který byl zaveden nového sestavení - rank sestavení. Toto sestavení se zařadit užitečnost funkcí. Důležité funkce je funkce s skóre pořadí 2 nebo vyšší.
Po porozumět tomu, které funkce mají význam, aktivovat build doporučení s seznamu (nebo dílčí seznam) smysluplný funkce. Je možné použít tyto funkce pro zvýšení záložním položky a cold položky. Chcete-li je používat pro záložním položky `UseFeatureInModel` parametr sestavení by měly být nastavené. Aby bylo možné používat funkce pro studenou položky `AllowColdItemPlacement` by měl být povolen parametr sestavení.
Poznámka: Není možné povolit `AllowColdItemPlacement` bez povolení `UseFeatureInModel`.

### <a name="43-recommendation-reasoning"></a>4.3. Doporučení reasoning
Doporučení reasoning je další aspekt používání funkcí. Modul Azure Machine Learning doporučení skutečně, můžete použít funkce k vysvětlení doporučení (také známa jako důvody), což větší jistotou v položce doporučené od doporučení příjemce.
Chcete-li povolit reasoning, `AllowFeatureCorrelation` a `ReasoningFeatureList` instalační program před požaduje doporučení sestavení musí být parametry.

## <a name="5-model-basic"></a>5. Model Basic
### <a name="51-create-model"></a>5.1. Vytvoření modelu
Vytvoří žádost o "Vytvoření modelu".

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
  **Poznámka:**: ID modelu je malá a velká písmena.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="52-get-model"></a>5.2. Získat modelu
Vytvoří žádost o "get model".

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| id |Jedinečný identifikátor modelu (malá a velká písmena) |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Data modelu naleznete v části následující prvky:

* `feed/entry/content/properties/Id`-Jedinečné ID modelu.
* `feed/entry/content/properties/Name`-Název modelu.
* `feed/entry/content/properties/Date`-Datum vytvoření model.
* `feed/entry/content/properties/Status`-Modelu stav. Jeden z následujících:
  * Vytvořit - Model je vytvořený a neobsahuje katalogu a využití.
  * ReadyForBuild - Model se vytvoří a obsahuje katalogu a využití.
* `feed/entry/content/properties/HasActiveBuild`-Určuje, pokud byl úspěšně sestaven modelu.
* `feed/entry/content/properties/BuildId`-ID modelu active sestavení.
* `feed/entry/content/properties/Mpr`-Model střední percentilu hodnocení (MPR - Další informace naleznete v tématu ModelInsight).
* `feed/entry/content/properties/UserName`-Model interní uživatelské jméno.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="53----get-all-models"></a>5.3.    Získat všechny modely
Načte všechny modely aktuálního uživatele.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetAllModels?apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

* `feed/entry/content/properties/Id`-Jedinečné ID modelu.
* `feed/entry/content/properties/Name`-Název modelu.
* `feed/entry/content/properties/Date`-Datum vytvoření model.
* `feed/entry/content/properties/Status`-Modelu stav. Jeden z následujících:
  * Vytvořit - Model je vytvořený a neobsahuje katalogu a využití.
  * ReadyForBuild - Model se vytvoří a obsahuje katalogu a využití.
* `feed/entry/content/properties/HasActiveBuild`-Určuje, pokud byl úspěšně sestaven modelu.
* `feed/entry/content/properties/BuildId`-ID modelu active sestavení.
* `feed/entry/content/properties/Mpr`-Model MPR (Další informace naleznete v tématu ModelInsight).
* `feed/entry/content/properties/UserName`-Model interní uživatelské jméno.
* `feed/entry/content/properties/UsageFileNames`-Seznam souborů využívání modelu oddělených čárkami.
* `feed/entry/content/properties/CatalogId`-ID modelu katalogu.
* `feed/entry/content/properties/Description`-Modelu popis.
* `feed/entry/content/properties/CatalogFileName`-Název souboru katalogu model.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="54----update-model"></a>5.4.    Aktualizace modelu
Můžete aktualizovat popis modelu nebo ID aktivního sestavení.<br>
<ins>ID aktivního sestavení</ins> -každé sestavení pro každý model má sestavení ID. ID aktivního sestavení je prvním úspěšném sestavení každý nový model. Jakmile máte ID aktivního sestavení a proveďte další sestavení pro stejný model, je nutné explicitně nastavit jako výchozí ID sestavení Pokud chcete. Když budete používat doporučení, pokud nezadáte ID sestavení, které chcete použít, výchozí nastavení se použije automaticky.<br>
Tento mechanismus umožňuje – až budete mít model doporučení v produkčním prostředí - pro vytvoření nových modelů a testování je před zvýšením úrovně je do produkčního prostředí.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| PUT |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| id |Jedinečný identifikátor modelu (malá a velká písmena) |
| apiVersion |1.0 |
|  | |
| Text žádosti |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br>Všimněte si, že značky XML, popis a ActiveBuildId jsou volitelné. Pokud nechcete nastavit popis nebo ActiveBuildId, odeberte celý značky. |

**Odpověď**:

Kód stavu HTTP: 200

### <a name="55----delete-model"></a>5.5.    Odstranit Model
Odstraní existující model podle ID.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| ODSTRANIT |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| id |Jedinečný identifikátor modelu (malá a velká písmena) |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

## <a name="6-model-advanced"></a>6. Rozšířené modelu
### <a name="61----model-data-insight"></a>6.1.    Přehled modelu dat
Vrátí data o využití, která tento model byl sestaven s statistické údaje.

K dispozici pouze pro sestavení doporučení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Data se vrátí jako kolekci vlastností.

* `feed/entry/id/content/properties/key`-Obsahuje název vlastnosti.
* `feed/entry/id/content/properties/value`-Obsahuje hodnotu vlastnosti.

Následující tabulka znázorňuje hodnotu, která představuje každý klíč.

| Klíč | Popis |
|:--- |:--- |
| AvgItemLength |Průměrný počet jedinečných uživatelů na každou položku. |
| AvgUserLength |Průměrný počet jedinečných položek na uživatele. |
| DensificationNumberOfItems |Počet položek po vyřazení položky, které nelze modelována. |
| DensificationNumberOfUsers |Počet bodů využití po vyřazení uživatelů a položky, které nelze modelována. |
| DensificationNumberOfRecords |Počet bodů využití po vyřazení uživatelů a položky, které nelze modelována. |
| MaxItemLength |Počet jedinečných uživatelů pro nejoblíbenější položku. |
| MaxUserLength |Maximální počet jedinečných položek pro uživatele. |
| MinItemLength |Maximální počet jedinečných uživatelů pro položku. |
| MinUserLength |Minimální počet jedinečných položek pro uživatele. |
| RawNumberOfItems |Počet položek v souborech použití. |
| RawNumberOfUsers |Počet bodů využití před všechny vyřazení. |
| RawNumberOfRecords |Počet bodů využití před všechny vyřazení. |
| SamplingNumberOfItems |Není k dispozici |
| SamplingNumberOfRecords |Není k dispozici |
| SamplingNumberOfUsers |Není k dispozici |

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="62----model-insight"></a>6.2.    Přehled modelu
Vrátí model náhled na aktivní sestavení nebo (je-li zadána) na konkrétní sestavení.

K dispozici pouze pro sestavení doporučení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |S ID aktivního sestavení:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>S konkrétní sestavovací ID:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| buildId |Volitelné – číslo, které identifikuje úspěšném sestavení. |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Data se vrátí jako kolekci vlastností.

* `feed/entry/id/content/properties/key`
* `feed/entry/id/content/properties/value`

Následující tabulka znázorňuje hodnotu, která představuje každý klíč.

| Klíč | Popis |
|:--- |:--- |
| CatalogCoverage |Jaké součástí katalogu můžete modelována s vzorce používání. Zbývající položky potřebovat funkce založené na obsah. |
| Pravidlem zásad správy |Střední percentilu hodnocení modelu. Nižší je lepší. |
| NumberOfDimensions |Počet dimenzí používá algoritmus factorization matice. |

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="63----get-model-sample"></a>6.3.    Získat ukázky modelu
Získá ukázku modelu doporučení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>S konkrétní sestavovací ID:<br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

OData XML

Odpověď se vrátí ve formátu raw textu:

<pre>
Level 1
---------------
655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel
    655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel Rating: 0.5215
    3f471802-f84f-44a0-99c8-6d2e7418eec1, Black Hawk Down: A Story of Modern War Rating: 0.5151
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5148
    6afc18e4-8c2a-43d1-9021-57543d6b11d8, Imajica Rating: 0.5146
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.514
56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai
    56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai Rating: 0.5218
    53156702-cc0c-443d-b718-6fb74b2491d3, Son of \ Rating: 0.5212
    fb8cf7a6-8719-46ee-97d4-92f931d77a3a, Smoke and Mirrors: Short Fictions and Illusions Rating: 0.5188
    8f5fe006-79e4-4679-816b-950989d1db4b, A Place I've Never Been (Contemporary American Fiction) Rating: 0.5156
    d8db4583-cc0f-49ce-bc95-b7fa3491623f, Happiness: A Novel Rating: 0.5156
50471eec-9aeb-4900-84d7-21567ab18546, If the Buddha Dated: A Handbook for Finding Love on a Spiritual Path
    cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine Secrets of the Ya-Ya Sisterhood: A Novel Rating: 0.5266
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5252
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5244
    e2cbf7ad-0636-4117-8b30-298da6df7077, Animal Dreams Rating: 0.5227
    6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions of an Ugly Stepsister: A Novel Rating: 0.5222
5e97148f-defb-4d74-af2d-80f4763bf531, The Deep End of the Ocean (Oprah's Book Club)
    5e97148f-defb-4d74-af2d-80f4763bf531, The Deep End of the Ocean (Oprah's Book Club) Rating: 0.537
    5dcbac37-2946-4f2a-a0b3-bbe710f9409a, Up Island: A Novel Rating: 0.5277
    bc5b69db-733b-4346-adde-3927544258f7, Downtown Rating: 0.5275
    31fe5c63-3e5a-48d0-802b-d3b0f989a634, Have a Nice Day: A Tale of Blood and Sweatsocks Rating: 0.5252
    0adf981a-b65b-4c11-b36b-78aca2f948a2, The Perfect Storm: A True Story of Men Against the Sea Rating: 0.5238
68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True Story of the Greatest Elephant That Ever Lived
    68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True Story of the Greatest Elephant That Ever Lived Rating: 0.5379
    6724862e-e4e7-4022-9614-1468d8b902ff, Little House on the Prairie Rating: 0.5345
    cdedb837-1620-496d-94c4-6ccfed888320, Little House in the Big Woods Rating: 0.5325
    382164ba-406b-4187-b726-d7a54b9d790d, The Tao of Pooh Rating: 0.5309
    6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5, On the Banks of Plum Creek Rating: 0.5285
37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships
    37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars, Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships Rating: 0.5397
    f2be16d4-5faf-4d32-ab83-7ba74d29261e, Politically Correct Bedtime Stories: Modern Tales for Our Life and Times Rating: 0.5207
    ef732c5c-334b-4d6b-ab82-7255eb7286d0, Honor Among Thieves Rating: 0.5195
    0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, The Giving Tree Rating: 0.5194
    883b360f-8b42-407f-b977-2f44ad840877, Scary Stories to Tell in the Dark: Collected from American Folklore (Scary Stories) Rating: 0.5184
ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: The Craft of Baseball
    d008dae9-c73a-40a1-9a9b-96d5cf546f36, The Gulag Archipelago 1918-1956: An Experiment in Literary Investigation I-II Rating: 0.5416
    ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: The Craft of Baseball Rating: 0.5403
    49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland Rating: 0.5394
    cc7964fd-d30f-478e-a425-93ddbdf094ed, Magic the Gathering: Arena Vol. 1 Rating: 0.5379
    8a1e9f36-97af-4614-bed9-24e3940a05f3, More Sniglets: Any Word That Doesn't Appear in the Dictionary but Should Rating: 0.5377
12a6d988-be21-4a09-8143-9d5f4261ba16, A Dream of Eagles
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5417
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.5416
    1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, The Family Rating: 0.5371
    56daeffe-7d48-43cd-8ef8-7dffd0c103d3, Kilo Class Rating: 0.5366
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5366
df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish
    56d33036-dfda-46b9-8e2a-76cb03921bb0, The X-Files: Ground Zero Rating: 0.5417
    0780cde8-6529-4e1d-b6c6-082c1b80e596, Twelve Red Herrings Rating: 0.5416
    df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish Rating: 0.5408
    400fe331-2c35-490c-adbc-b28b4b73d56c, Shall We Tell the President? Rating: 0.5383
    f86ad7d0-5c03-42b3-aebf-13d44aec8b30, Shades of Grace Rating: 0.5358
de1f62a4-89e6-44d2-aaee-992a4bf093f1, The Map That Changed the World: William Smith and the Birth of Modern Geology
    de1f62a4-89e6-44d2-aaee-992a4bf093f1, The Map That Changed the World: William Smith and the Birth of Modern Geology Rating: 0.5422
    b303538f-e2c6-4a2c-b425-8d21e684fc3e, My Uncle Oswald Rating: 0.5385
    34b84627-48af-4a4c-96c4-b26fb3863f56, Midnight In the Garden of Good and Evil Rating: 0.5379
    306cbaa7-b1a8-4142-9d55-e11b5018a7a8, The Street Lawyer Rating: 0.5376
    e53b4baa-8c09-45c4-95c0-b6a26b98770b, Miss Smillas Feeling for Snow Rating: 0.5367

Level 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, The Sinister Pig (Hillerman Tony)
    352aaea1-6b12-454d-a3d5-46379d9e4eb2, The Sinister Pig (Hillerman Tony) Rating: 0.5425
    74c49398-bc10-4af5-a658-a996a1201254, Children of the Storm (Peters Elizabeth) Rating: 0.5387
    9ba80080-196e-43fd-8025-391d963f77e7, The Floating Girl Rating: 0.5372
    e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Smile (Scottoline Lisa) Rating: 0.5353
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5332
c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days
    0adf981a-b65b-4c11-b36b-78aca2f948a2, The Perfect Storm: A True Story of Men Against the Sea Rating: 0.5433
    c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days Rating: 0.543
    a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: Any Word That Doesn't Appear in the Dictionary But Should) Rating: 0.5327
    6f6e192e-0d64-49ca-9b63-f09413ea1ee6, Politically Correct Holiday Stories: For an Enlightened Yuletide Season Rating: 0.5307
    798051a8-147d-4d46-b0dc-e836325029e6, AGE OF INNOCENCE (MOVIE TIE-IN) Rating: 0.5301
73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics)
    cba8163f-6536-436b-8130-47b4a43c827f, Trust No One (The Official Guide to the X-Files Vol. 2) Rating: 0.5434
    5708e4cb-2492-49c0-94a8-cc413eec5d89, Small Gods (Discworld Novels (Paperback)) Rating: 0.5406
    73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics) Rating: 0.5403
    d885b0bd-ae4b-452d-bdf2-faa90197dbc9, The Color of Magic Rating: 0.539
    b133a9c4-4784-4db3-b100-d0d6dffb94d2, The Truth Is Out There (The Official Guide to the X-Files Vol. 1) Rating: 0.5367
271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why the Winged Whale Sings
    271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why the Winged Whale Sings Rating: 0.5445
    2de1c354-90ff-47c5-a0db-1bad7d88ef94, The Salaryman's Wife (Children of Violence Series) Rating: 0.5329
    d279416e-19c0-43f8-9ec9-a585947879ca, Zen Attitude Rating: 0.5316
    c8f854d7-3de3-4b23-8217-f4f851670fd4, Revenge of the Cootie Girls: A Robin Hudson Mystery (Robin Hudson Mysteries (Paperback)) Rating: 0.5305
    8ef4751c-7074-409e-a3ac-d49b222fc864, Where the Wild Things Are Rating: 0.5289
9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God
    9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God Rating: 0.5446
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, The Bean Trees Rating: 0.5389
    65ecbdd1-131c-40c3-a3d6-d86ca281377a, The God of Small Things Rating: 0.5387
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, The Stone Diaries Rating: 0.5355
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5344
5f17d90a-2604-4fe8-8977-1a280b9098b1, One for the Money (Stephanie Plum Novels (Paperback))
    5f17d90a-2604-4fe8-8977-1a280b9098b1, One for the Money (Stephanie Plum Novels (Paperback)) Rating: 0.5446
    57169b2b-9a8a-486b-9aac-1ed98ce57168, Final Appeal Rating: 0.5332
    efcb1bc4-7278-4a8f-b491-befde02070d6, Moment of Truth Rating: 0.5329
    1efa91a2-993b-4c43-9f5c-3454fc12612d, Burn Factor Rating: 0.5309
    24c59962-458a-4ec8-b95d-d694e861919c, At Home in Mitford (The Mitford Years) Rating: 0.5303
4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: The Boy Who Was Raised As a Girl
    4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: The Boy Who Was Raised As a Girl Rating: 0.5449
    cd5f2c03-20cb-43be-a1fb-3b4233e63222, Pigs in Heaven Rating: 0.5329
    19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, Love in the Time of Cholera (Penguin Great Books of the 20th Century) Rating: 0.5267
    15689d09-c711-4844-84d8-130a90237b26, Bel Canto Rating: 0.5245
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5235
98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories
    f874b5a3-5d40-4436-94ff-0fa1c090ddf5, The Sun Also Rises (A Scribner classic) Rating: 0.5451
    98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories Rating: 0.5442
    0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Unseen Rating: 0.5421
    15316ca6-1e38-425f-893d-691944a47000, More Scary Stories To Tell In The Dark Rating: 0.5409
    329d5682-3dc3-4206-8aa2-eef4b1032258, Letters from the Earth Rating: 0.54
5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover))
    5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover)) Rating: 0.5462
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5372
    604eb3bd-6026-4f51-bffd-9fb54f180400, Family Pictures: A Novel Rating: 0.5341
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5334
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, The Bean Trees Rating: 0.5319
d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven
    d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven Rating: 0.5491
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5401
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, The Stone Diaries Rating: 0.5393
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5382
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5367

</pre>


## <a name="7-model-business-rules"></a>7. Model obchodní pravidla
Toto jsou typy pravidel, které jsou podporovány:

* <strong>BlockList</strong> -BlockList umožňuje zadat seznam položek, které nechcete vracet ve výsledcích doporučení. 
* <strong>FeatureBlockList</strong> -BlockList funkce umožňuje blokovat položek na základě hodnot její funkce.

*Neodesílat více než 1 000 položek v jednom blocklist pravidlo nebo volání může časový limit. Pokud potřebujete blokovat více než 1 000 položek, můžete provést několik blocklist volání.*

* <strong>Upsale</strong> -Upsale umožňuje vynucovat položek k vrácení ve výsledcích doporučení.
* <strong>Seznam povolených adres</strong> -seznamu povolených vám umožní pouze navrhovat doporučení ze seznamu položek.
* <strong>FeatureWhiteList</strong> – seznam povolených funkcí umožňuje pouze doporučujeme položky, které mají hodnoty příslušné funkce.
* <strong>PerSeedBlockList</strong> -na seznam blokovaných počáteční hodnoty můžete zadat na položku seznam položek, které nemůže být vrácen jako doporučení výsledky.

### <a name="71----get-model-rules"></a>7.1.    Získat pravidla modelu
| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

* `feed/entry/content/properties/Id`-Jedinečný identifikátor tohoto pravidla.
* `feed/entry/content/properties/Type`-Typ pravidla.
* `feed/entry/content/properties/Parameter`-Parametr pravidla.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="72----add-rule"></a>7.2.    Přidat pravidlo
| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/AddRule?apiVersion=%271.0%27` |
| ZÁHLAVÍ |`"Content-Type", "text/xml"` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| Text žádosti | |

<ins>Vždy, když poskytnete ID položek obchodní pravidla, nezapomeňte použít externí Id položky (stejným Id, který jste použili v katalogu souboru)</ins><br>
<ins>Chcete-li přidat pravidlo BlockList:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><ins>
<ins>Chcete-li přidat pravidlo FeatureBlockList:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><ins>Chcete-li přidat pravidlo Upsale:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br>
<ins>Chcete-li přidat pravidlo seznamu povolených IP adres:</ins><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><ins>
<ins>Chcete-li přidat pravidlo FeatureWhiteList:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><ins>Chcete-li přidat pravidlo PerSeedBlockList:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|

**Odpověď**:

Kód stavu HTTP: 200

Rozhraní API vrátí nově vytvořeného pravidla s jeho podrobnostmi. Vlastnost pravidla lze načíst z následující cesty:

* `feed/entry/content/properties/Id`-Jedinečný identifikátor tohoto pravidla.
* `feed/entry/content/properties/Type`-Typ pravidla: BlockList nebo Upsale.
* `feed/entry/content/properties/Parameter`-Parametr pravidla.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="73----delete-rule"></a>7.3.    Odstranit pravidlo
| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| ODSTRANIT |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| filterId |Jedinečný identifikátor filtru |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

### <a name="74----delete-all-rules"></a>7.4.    Odstranit všechna pravidla
| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| ODSTRANIT |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

## <a name="8-catalog"></a>8. Katalogu
### <a name="81----import-catalog-data"></a>8.1.    Umožňuje importovat Data Catalog
Pokud nahrajete několik katalog souborů do stejného modelu s několika volání, jsme vloží nové položky katalogu. Existující položky zůstane s původními hodnotami. Data katalogu nelze aktualizovat pomocí této metody.

Data katalogu postupujte podle následujícího formátu:

* Bez funkce –`<Item Id>,<Item Name>,<Item Category>[,<Description>]`
* S funkcemi-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

Poznámka: Maximální velikost souboru je 200MB.

** Formát podrobností **

| Name (Název) | Povinné | Typ | Popis |
|:--- |:--- |:--- |:--- |
| Id položky |Ano |[A-z], [-z], [0-9] [_] &#40; Podtržítko &#41; [-] &#40; Dash &#41;<br> Maximální délka: 50 |Jedinečný identifikátor položky. |
| Název položky |Ano |Žádné alfanumerické znaky<br> Maximální délka: 255 |Název položky. |
| Kategorie položky |Ano |Žádné alfanumerické znaky <br> Maximální délka: 255 |Kategorie, do které patří tato položka (například vaření knihy, obrázkům...); nesmí být prázdné. |
| Popis |Ne, pokud funkce jsou k dispozici (ale nesmí být prázdné) |Žádné alfanumerické znaky <br> Maximální délka: 4000 |Popis této položky. |
| Seznam funkcí |Ne |Žádné alfanumerické znaky <br> Maximální délka: 4000; Maximální počet funkcí: 20 |Čárkami oddělený seznam název funkce = hodnota funkce, který slouží k vylepšení modelu doporučení; v tématu [Advanced témata](#2-advanced-topics) části. |

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |
| ZÁHLAVÍ |`"Content-Type", "text/xml"` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| Název souboru |Textové identifikátor katalogu.<br>Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.<br>Maximální délka: 50 |
| apiVersion |1.0 |
|  | |
| Text žádosti |Příklad (s funkcí):<br/>vytváření 2406e770-769c-4189-89de-1c9283f93a96, Clara Callan adresáře, popis knihy = Richard Wright vydavatele = Harper Flamingo Kanada, rok = 2001<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, místnosti zapomenutí: vytváření A Fiction (Byzantium kniha), kniha,, = Nick Bantock vydavatele = Harpercollins, rok = 1997<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, kniha,, vytvářet = Alexander Findley vydavatele = HarperFlamingo Kanada rok = 2001<br>552a1940-21e4-4399-82bb-594b46d7ed54, omezení zvířata, kniha, popis knihy vytvářet = Magnus lisoven vydavatele = plošinová publikování rok = 1998</pre> |

**Odpověď**:

Kód stavu HTTP: 200

Rozhraní API vrátí sestavy importu.

* `feed\entry\content\properties\LineCount`-Přijaté počet řádků.
* `feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
       <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="82----get-catalog"></a>8.2.    Získat katalogu
Načte všechny položky katalogu.
Katalog bude načtených v daný okamžik jednu stránku. Pokud chcete získat položky v konkrétním indexem, můžete použít parametr $skip odata. Například pokud chcete získat položky začínající na pozici 100, přidejte parametr $skip = 100 na požadavek.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za položka katalogu. Každá položka má následující data:

* `feed/entry/content/properties/ExternalId`-ID položky katalogu externí, zadanému zákazník.
* `feed/entry/content/properties/InternalId`-Katalogu interní ID položky, ten, který vygeneruje Azure Machine Learning doporučení.
* `feed/entry/content/properties/Name`-Název položky katalogu.
* `feed/entry/content/properties/Category`-Kategorie položky katalogu.
* `feed/entry/content/properties/Description`-Katalogu popis položky.
* `feed/entry/content/properties/Metadata`-Metadata položky katalogu.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">The Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="83----get-catalog-items-by-token"></a>8.3.    Získat položky katalogu pomocí tokenu
| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| Token |Token názvu položky katalogu. By mělo obsahovat aspoň 3 znaky. |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za položka katalogu. Každá položka má následující data:

* `feed/entry/content/properties/InternalId`-Katalogu interní ID položky, ten, který vygeneruje Azure Machine Learning doporučení.
* `feed/entry/content/properties/Name`-Název položky katalogu.
* `feed/entry/content/properties/Rating`-(pro budoucí použití)
* `feed/entry/content/properties/Reasoning`-(pro budoucí použití)
* `feed/entry/content/properties/Metadata`-(pro budoucí použití)
* `feed/entry/content/properties/FormattedRating`-(pro budoucí použití)

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                  <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                  </m:properties>
            </content>
        </entry>
    </feed>

## <a name="9-usage-data"></a>9. Údaje o využití
### <a name="91----import-usage-data"></a>9.1.    Importovat Data o využití
#### <a name="911-uploading-file"></a>9.1.1. Nahrání souboru
V této části ukazuje, jak odeslat data o využití pomocí souboru. Můžete volat toto rozhraní API se data o využití. Všechna data o využití se uloží pro všechna volání.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| Název souboru |Textové identifikátor katalogu.<br>Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.<br>Maximální délka: 50 |
| apiVersion |1.0 |
|  | |
| Text žádosti |Data o využití. Formát:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Name (Název)</th><th>Povinné</th><th>Typ</th><th>Popis</th></tr><tr><td>Id uživatele</td><td>Ano</td><td>[A-z], [-z], [0-9] [_] &#40; Podtržítko &#41; [-] &#40; Dash &#41;<br> Maximální délka: 255 </td><td>Jedinečný identifikátor uživatele.</td></tr><tr><td>Id položky</td><td>Ano</td><td>[A-z], [-z], [0-9] [&#95;] &#40; Podtržítko &#41; [-] &#40; Dash &#41;<br> Maximální délka: 50</td><td>Jedinečný identifikátor položky.</td></tr><tr><td>Čas</td><td>Ne</td><td>Datum ve formátu: rrrr/MM/ddTHH (například 2013/06/20T10:00:00)</td><td>Čas data.</td></tr><tr><td>Událost</td><td>Ne; Pokud zadaný musí taky datum</td><td>Jeden z následujících:<br>• Klikněte na tlačítko<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Nákupu</td><td></td></tr></table><br>Maximální velikost souboru: 200MB<br><br>Příklad:<br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Odpověď**:

Kód stavu HTTP: 200

* `Feed\entry\content\properties\LineCount`-Přijaté počet řádků.
* `Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby.
* `Feed\entry\content\properties\FileId`-Souboru identifikátor.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="912-using-data-acquisition"></a>9.1.2. Pomocí získávání dat
V této části ukazuje, jak k odesílání událostí v reálném čase na Azure Machine Learning doporučení, obvykle z vašeho webu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27` |
| ZÁHLAVÍ |`"Content-Type", "text/xml"` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| apiVersion |1.0 |
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
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
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

### <a name="92----list-model-usage-files"></a>9.2.    Seznam modelu využití souborů
Načte metadata všechny soubory modelu využití.
Využití, které budou soubory načtených v daný okamžik jednu stránku. Položky neobsahuje 100 každé stránky. Pokud chcete získat položky v konkrétním indexem, můžete použít parametr $skip odata. Například pokud chcete získat položky začínající na pozici 100, přidejte parametr $skip = 100 na požadavek.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| forModelId |Jedinečný identifikátor modelu |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za použití souboru. Každá položka má následující data:

* `feed\entry\content\properties\Id`– ID využití souboru.
* `feed\entry\content\properties\Length`-Využití délka souboru v MB.
* `feed\entry\content\properties\DateModified`-Datum vytvoření souboru využití.
* `feed\entry\content\properties\UseInModel`– Jestli využití souboru se používá v modelu.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
              </m:properties>
        </content>
    </entry>
</feed>

### <a name="93----get-usage-statistics"></a>9.3.    Získat statistiku využití
Získá Statistika využití.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| Počátečním |Počáteční datum. Formát: rrrr/MM/ddTHH |
| Koncové datum |Koncové datum. Formát: rrrr/MM/ddTHH |
| eventTypes |Textový soubor s oddělovači řetězec typů událostí nebo hodnota null, zobrazíte všechny události |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Kolekci elementů klíč/hodnota. Každé z nich obsahuje součet události pro určitý typ události seskupené podle hodinu.

* `feed\entry[i]\content\properties\Key`-Obsahuje čas (seskupené podle hodinu) a typ události.
* `feed\entry[i]\content\properties\Value`-Počet celkový počet událostí.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
              </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="94----get-usage-file-sample"></a>9.4.    Získat ukázkový soubor využití
Načte první 2KB využití obsahu souboru.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| fileId |Jedinečný identifikátor využití souboru modelu |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď se vrátí ve formátu raw textu:

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
</pre>


### <a name="95----get-model-usage-file"></a>9.5.    Získat soubor modelu využití
Načte celý obsah souboru využití.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| Mid – |Jedinečný identifikátor modelu |
| FID |Jedinečný identifikátor využití souboru modelu |
| Stahování |1 |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď se vrátí ve formátu raw textu:

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260965,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
102758,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
112602,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
163925,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
262998,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
144717,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
</pre>

### <a name="96----delete-usage-file"></a>9.6.    Odstranit soubor využití
Odstraní soubor využití zadaného modelu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| ODSTRANIT |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| fileId |Jedinečný identifikátor souboru k odstranění |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

### <a name="97----delete-all-usage-files"></a>9.7.    Odstraní všechny soubory využití
Odstraní všechny soubory modelu využití.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| ODSTRANIT |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

## <a name="10-features"></a>10. Funkce
V této části ukazuje, jak načíst informace o funkci, jako je například importované funkce a jejich hodnoty, jejich pořadí a toto pořadí byl přidělen. Funkce importují v rámci katalogu dat a jejich pořadí je pak přidružené, pokud se provádí rank sestavení.
Funkce pořadí můžete změnit podle vzoru data o využití a typu položky. Ale pro konzistentní využití nebo položky, pořadí by měl mít jenom malé kolísání.
Pořadí funkcí je nezáporné číslo. Číslo 0 znamená, že tato funkce nebyla seřazeny (se stane, když vyvolání toto rozhraní API před dokončením první rank sestavení). Datum, kdy byl pořadí s atributy se nazývá skóre podle aktuálnosti.

### <a name="101-get-features-info-for-last-rank-build"></a>10.1. Získání informací o funkcích (pro poslední Rank sestavení)
Načte informace o funkci, včetně hodnocení pro poslední úspěšné rank sestavení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| samplingSize |Počet hodnot, které chcete pro každou součást podle data, která je obsažená v katalogu. <br/>Možné hodnoty:<br> -1 - všechny vzorky. <br>0 – žádný vzorkování. <br>N - vrátí N ukázky pro každý název funkce. |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď obsahuje seznam položek informace o funkci. Každý záznam obsahuje:

* `feed/entry/content/m:properties/d:Name`– Název funkce.
* `feed/entry/content/m:properties/d:RankUpdateDate`-Datum, kdy pořadí byl přidělen k této funkci také znám jako skóre podle aktuálnosti funkce. Historická data (0001-01-01T00:00:00 ") znamená, že byla provedena žádná rank sestavení.
* `feed/entry/content/m:properties/d:Rank`– Pořadí funkce (float). Pořadí 2.0 nebo vyšší je považován za dobré funkce.
* `feed/entry/content/m:properties/d:SampleValues`-Textový soubor s oddělovači seznamu hodnot až je požadovaná velikost vzorkování.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get the features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>

### <a name="102-get-features-info-for-specific-rank-build"></a>10.2. Získání informací o funkcích (pro konkrétní Rank sestavení)
Načte informace o funkci, včetně hodnocení pro konkrétní rank sestavení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| samplingSize |Počet hodnot, které chcete pro každou součást podle data, která je obsažená v katalogu.<br/> Možné hodnoty:<br> -1 - všechny vzorky. <br>0 – žádný vzorkování. <br>N - vrátí N ukázky pro každý název funkce. |
| rankBuildId |Jedinečný identifikátor rank sestavení nebo -1 pro poslední rank sestavení |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď obsahuje seznam položek informace o funkci. Každý záznam obsahuje:

* `feed/entry/content/m:properties/d:Name`– Název funkce.
* `feed/entry/content/m:properties/d:RankUpdateDate`-Datum, kdy pořadí byl přidělen k této funkci také znám jako skóre podle aktuálnosti funkce. Historická data (0001-01-01T00:00:00 ") znamená, že byla provedena žádná rank sestavení.
* `feed/entry/content/m:properties/d:Rank`– Pořadí funkce (float). Pořadí 2.0 nebo vyšší je považován za dobré funkce.
* `feed/entry/content/m:properties/d:SampleValues`-Textový soubor s oddělovači seznamu hodnot až je požadovaná velikost vzorkování.

OData

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get the features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


## <a name="11-build"></a>11. Sestavení
  Tato část vysvětluje různé rozhraní API související s sestavení. Existují 3 typy sestavení: sestavení doporučení, rank sestavení a sestavení FBT (často koupili společně).

Účelem sestavení doporučení je chcete generovat model doporučení použít pro předpovědi. Predikce (pro tento typ sestavení) má ve dvou typů:

* I2I - také znám jako Doporučení položkami - danou položku nebo seznam položek, tato možnost bude předpovídat seznam položek, které by mohly zajímat vysoké.
* U2I - také znám jako Uživatele pro položky doporučení – dané id uživatele (a volitelně seznam položek) Tato možnost bude předvídání seznam položek, které by mohly být vysoké týkající se daného uživatele (a další volbu položek). U2I doporučení jsou založená na historii položky, které byly týkající se uživatelů až do okamžiku, kdy byl vytvořený modelu.

Rank sestavení je technické sestavení, který umožňuje získat informace o použitelnosti vaší funkcí. Obvykle aby bylo možné získat nejlepších výsledků pro model doporučení zahrnující funkce, byste měli vzít následující kroky:

* Aktivovat rank build (Pokud skóre vaší funkcí není stabilní) a počkejte, dokud získat skóre funkce.
* Načtení pořadí vaší funkce voláním [získat informace o funkcích](#101-get-features-info-for-last-rank-build) rozhraní API.
* Konfigurace sestavení doporučení s následujícími parametry:
  * `useFeatureInModel`– Nastavte na hodnotu True.
  * `ModelingFeatureList`– Nastavte na textový soubor s oddělovači seznam funkcí se skóre 2.0 nebo více (podle pořadí, které jste získali v předchozím kroku).
  * `AllowColdItemPlacement`– Nastavte na hodnotu True.
  * Volitelně můžete nastavit `EnableFeatureCorrelation` na hodnotu True a `ReasoningFeatureList` do seznamu funkcí, kterou chcete použít pro vysvětlení (obvykle seznam stejné funkce použité v modelování nebo dílčí seznam).
* Aktivovat sestavení doporučení s konfigurovanými parametry.

Poznámka: Pokud nenakonfigurujete žádné parametry (například vyvolání doporučení sestavení bez parametrů) nebo explicitně nezakážete využití funkcí (například `UseFeatureInModel` nastavena na hodnotu False), nastaví parametry související s funkcí, které vysvětleno v systému výše uvedené hodnoty v případě, že pořadí sestavení existuje.

Neexistuje žádné omezení na spuštění rank sestavení a doporučení sestavení současně pro stejný model. Nicméně na stejný model paralelně nelze spustit dvě sestavení stejného typu.

Sestavení FBT (často koupili společně) je ještě jiný algoritmus doporučení názvem někdy "konzervativní" doporučené, který je vhodný pro katalogů, které nejsou ve své podstatě homogenní (homogenní: knihy, filmy, některé jídlo dotazování; bez homogenní: počítače a zařízení, mezi doménami, velmi různorodému).

Poznámka: Pokud využití soubory, které jste odeslali obsahovat volitelné pole "typ události" pak pro FBT modelování pouze "Nákupu" události se použije. Pokud je k dispozici žádný typ události, že všechny události bude považovat za nákupu.

#### <a name="111-build-parameters"></a>11.1 sestavení parametry
Každý typ sestavení je možné nakonfigurovat přes sadu parametrů (které popsané níže). Pokud nenakonfigurujete parametry, systém automaticky atribut hodnoty na parametry podle informace obsažené v době, že aktivovat build.

##### <a name="1111-usage-condenser"></a>11.1.1. Chladič využití
Další šumu než informace může obsahovat uživatele nebo položky s několika bodů využití. Systém se pokusí minimální počet bodů využití na uživatele nebo položku, který se má použít v modelu předpovědi. Toto číslo se bude nacházet v rozsahu definovaného ItemCutoffLowerBound a ItemCutoffUpperBound parametry pro položky a rozsah definovaný hodnotami parametrů UserCutOffLowerBound a UserCutoffUpperBound pro uživatele. Chladič vliv na položky nebo uživatelé se dá minimalizovat nastavení alespoň jeden odpovídající mezí nule.

##### <a name="1112-rank-build-parameters"></a>11.1.2. Pořadí sestavení parametry
Následující tabulka znázorňuje sestavení parametry rank sestavení.

| Klíč | Popis | Typ | Platnou hodnotu. |
|:--- |:--- |:--- |:--- |
| NumberOfModelIterations |Počet iterací, které provádí modelu se odráží celkovou dobu výpočtů a přesnosti modelu. Tím vyšší je číslo, lepší přesnost budete mít, ale bude trvat delší dobu výpočtů. |Integer |10-50 |
| NumberOfModelDimensions |Počet dimenzí má vztah k číslo modelu se pokusí najít v rámci vašich dat funkcí. Zvýšením počtu dimenzí vám umožní lépe vyladit výsledky do menší clusterů. Však příliš mnoho dimenze zabrání modelu nalézt korelací mezi položkami. |Integer |10-40 |
| ItemCutOffLowerBound |Definuje položku dolní mez pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| ItemCutOffUpperBound |Definuje položku horní mez pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| UserCutOffLowerBound |Definuje dolní mez uživatele pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| UserCutOffUpperBound |Definuje uživatele horní mez pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |

##### <a name="1113-recommendation-build-parameters"></a>11.1.3. Parametry doporučení sestavení
Následující tabulka znázorňuje parametry sestavení pro sestavení doporučení.

| Klíč | Popis | Typ | Platnou hodnotu. |
|:--- |:--- |:--- |:--- |
| NumberOfModelIterations |Počet iterací, které provádí modelu se odráží celkovou dobu výpočtů a přesnosti modelu. Tím vyšší je číslo, lepší přesnost budete mít, ale bude trvat delší dobu výpočtů. |Integer |10-50 |
| NumberOfModelDimensions |Počet dimenzí má vztah k číslo modelu se pokusí najít v rámci vašich dat funkcí. Zvýšením počtu dimenzí vám umožní lépe vyladit výsledky do menší clusterů. Však příliš mnoho dimenze zabrání modelu nalézt korelací mezi položkami. |Integer |10-40 |
| ItemCutOffLowerBound |Definuje položku dolní mez pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| ItemCutOffUpperBound |Definuje položku horní mez pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| UserCutOffLowerBound |Definuje dolní mez uživatele pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| UserCutOffUpperBound |Definuje uživatele horní mez pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| Popis |Vytvořte popis. |Řetězec |Jakýkoli text, maximální 512 znaků |
| EnableModelingInsights |Umožňuje výpočetní metriky pro model doporučení. |Logická hodnota |Hodnotu true nebo False |
| UseFeaturesInModel |Určuje funkce lze nastavit v zájmu modelu doporučení. |Logická hodnota |Hodnotu true nebo False |
| ModelingFeatureList |Čárkami oddělený seznam názvů funkcí pro použití v sestavení doporučení, aby se zvýšila doporučení. |Řetězec |Funkce názvy až 512 znaků |
| AllowColdItemPlacement |Označuje, pokud doporučení by také push cold položky prostřednictvím podobností funkci. |Logická hodnota |Hodnotu true nebo False |
| EnableFeatureCorrelation |Určuje funkce lze nastavit v reasoning. |Logická hodnota |Hodnotu true nebo False |
| ReasoningFeatureList |Čárkami oddělený seznam názvů funkcí má být použit pro odůvodnění věty (např. vysvětlení doporučení). |Řetězec |Funkce názvy až 512 znaků |
| EnableU2I |Povolit přizpůsobené doporučení také znám jako U2I (uživatelům položky doporučení). |Logická hodnota |Logická hodnota (true výchozí) |

##### <a name="1114-fbt-build-parameters"></a>11.1.4. Parametry FBT sestavení
Následující tabulka znázorňuje parametry sestavení pro sestavení doporučení.

| Klíč | Popis | Typ | Platnou hodnotu (výchozí) |
|:--- |:--- |:--- |:--- |
| FbtSupportThreshold |Jak konzervativní je modelu. Počet položek, které mají být považovány za pro modelování společné opakování. |Integer |3-50 (6) |
| FbtMaxItemSetSize |Počet položek v sadě časté Bounds. |Integer |2-3 (2) |
| FbtMinimalScore |Minimální skóre, který by měl být sadu často chcete-li být do vrácených výsledků zahrnuty. Čím lépe. |Double |0 a vyšší (0) |
| FbtSimilarityFunction |Definuje podobností funkci má být používána sestavení. Navýšení upřednostňuje serendipity, společné výskyt upřednostňuje předvídatelnost a Jaccard je to dobrý kompromis mezi nimi. |Řetězec |cooccurrence, navýšení, jaccard (navýšení) |

### <a name="112-trigger-a-recommendation-build"></a>11.2. Aktivovat Build doporučení
  Ve výchozím nastavení toto rozhraní API aktivují sestavení modelu doporučení. K aktivaci rank sestavení (Chcete-li stanovení skóre funkce), slouží variant rozhraní API sestavení s parametrem typu sestavení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |
| ZÁHLAVÍ |`"Content-Type", "text/xml"`(Pokud odesílá text žádosti) |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| userDescription |Textové identifikátor katalogu. Poznámka: Pokud použijete prostory musí zakódovat ho s % 20 místo. Najdete v předchozím příkladu.<br>Maximální délka: 50 |
| apiVersion |1.0 |
|  | |
| Text žádosti |Pokud je ponecháno prázdné pak sestavení bude proveden výchozí parametry sestavení.<br><br>Pokud chcete nastavit parametry sestavení, odešlete parametry ve formátu XML do datové části jako následující příklad. (Viz část "Sestavení parametry" Další informace o parametrech.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>` |

**Odpověď**:

Kód stavu HTTP: 200

Toto je asynchronní rozhraní API. Zobrazí se ID sestavení jako odpověď. Vědět, pokud sestavení skončila, by měly volat rozhraní API "Získat sestavení stavu systému Model" a vyhledejte toto ID sestavení v odpovědi. Všimněte si, že sestavení může trvat minut až hodin v závislosti na velikosti dat.

Nemůžete spotřebovat doporučení do sestavení ukončí.

Stav platný sestavení:

* Vytvořit - byla vytvořena žádost sestavení.
* Ve frontě - sestavení žádost byla odeslána a je ve frontě.
* Probíhá vytváření - sestavení.
* Úspěch - sestavení skončila úspěšně.
* Chyba: sestavení skončila s chybou.
* Zrušena - sestavení byla zrušena.
* Zrušení - byla odeslána žádost o zrušení pro sestavení.

Všimněte si, že ID sestavení naleznete v následující cestě:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="113-trigger-build-recommendation-rank-or-fbt"></a>11.3. Aktivační události sestavení (doporučení, pořadí nebo FBT)
| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| POST |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27` |
| ZÁHLAVÍ |`"Content-Type", "text/xml"`(Pokud odesílá text žádosti) |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| userDescription |Textové identifikátor katalogu. Poznámka: Pokud použijete prostory musí zakódovat ho s % 20 místo. Najdete v předchozím příkladu.<br>Maximální délka: 50 |
| buildType |Typ sestavení, která má být vyvolán: <br/> -"Doporučení" doporučení sestavení <br> -Řazení rank sestavení <br/> -'Fbt' FBT sestavení |
| apiVersion |1.0 |
|  | |
| Text žádosti |Pokud je ponecháno prázdné pak sestavení bude proveden výchozí parametry sestavení.<br><br>Pokud chcete nastavit parametry sestavení, odešlete je ve formátu XML do datové části jako v následující ukázce. (Viz část "Sestavení parametry" vysvětlení a úplný seznam parametrů.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>` |

**Odpověď**:

Kód stavu HTTP: 200

Toto je asynchronní rozhraní API. Zobrazí se ID sestavení jako odpověď. Vědět, pokud sestavení skončila, by měly volat rozhraní API "Získat sestavení stavu systému Model" a vyhledejte toto ID sestavení v odpovědi. Všimněte si, že sestavení může trvat minut až hodin v závislosti na velikosti dat.

Nemůžete spotřebovat doporučení do sestavení ukončí.

Stav platný sestavení:

* Vytvoření – Model byl vytvořen.
* Ve frontě - sestavení modelu byla spuštěna a je ve frontě.
* Sestavení – Model sestavuje.
* Úspěch - sestavení skončila úspěšně.
* Chyba: sestavení skončila s chybou.
* Zrušena - sestavení byla zrušena.
* Zrušení - sestavení se ruší.

Všimněte si, že ID sestavení naleznete v následující cestě:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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




### <a name="114-get-builds-status-of-a-model"></a>11.4. Získat stav sestavení modelu
Načte sestavení a jejich stavu pro zadaný model.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| onlyLastBuild |Označuje, zda vrátit veškerá historie sestavení modelu nebo pouze stav poslední sestavení |
| apiVersion |1.0 |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za sestavení. Každá položka má následující data:

* `feed/entry/content/properties/UserName`-Název uživatele.
* `feed/entry/content/properties/ModelName`-Název modelu.
* `feed/entry/content/properties/ModelId`-Model jedinečný identifikátor.
* `feed/entry/content/properties/IsDeployed`– Jestli (také známa jako nasazení sestavení aktivní sestavení).
* `feed/entry/content/properties/BuildId`-Vytvořte jedinečný identifikátor.
* `feed/entry/content/properties/BuildType`-Typ sestavení.
* `feed/entry/content/properties/Status`-Stav sestavení. Může být jedna z následujících akcí: Chyba, sestavování, zařazeno do fronty, Cancelling, zrušeno, úspěch.
* `feed/entry/content/properties/StatusMessage`-Zpráva podrobné informace o stavu (platí pouze pro konkrétní stavy).
* `feed/entry/content/properties/Progress`-Vytvořte průběh (%).
* `feed/entry/content/properties/StartTime`-Čas spuštění sestavení.
* `feed/entry/content/properties/EndTime`-Vytvořte koncový čas.
* `feed/entry/content/properties/ExecutionTime`-Vytvořte doba trvání.
* `feed/entry/content/properties/ProgressStep`-Podrobnosti o aktuální fáze sestavení v průběhu.

Stav platný sestavení:

* Vytvořit - sestavení požadavek položka byla vytvořena.
* Ve frontě - sestavení požadavku byla spuštěna a je ve frontě.
* Probíhá vytváření - sestavení.
* Úspěch - sestavení skončila úspěšně.
* Chyba: sestavení skončila s chybou.
* Zrušena - sestavení byla zrušena.
* Zrušení - sestavení se ruší.

Platné hodnoty pro typ sestavení:

* Pořadí - pořadí sestavení.
* Doporučení - doporučení sestavení.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="115-get-builds-status"></a>11.5. Získat stav sestavení
Načte sestavení stavy všech modelů uživatele.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| onlyLastBuild |Označuje, zda vrátit veškerá historie sestavení modelu nebo pouze stav poslední sestavení. |
| apiVersion |1.0 |

**Odpověď**:

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za sestavení. Každá položka má následující data:

* `feed/entry/content/properties/UserName`-Název uživatele.
* `feed/entry/content/properties/ModelName`-Název modelu.
* `feed/entry/content/properties/ModelId`-Model jedinečný identifikátor.
* `feed/entry/content/properties/IsDeployed`– Jestli sestavení nasazeno.
* `feed/entry/content/properties/BuildId`-Vytvořte jedinečný identifikátor.
* `feed/entry/content/properties/BuildType`-Typ sestavení.
* `feed/entry/content/properties/Status`-Stav sestavení. Může být jedna z následujících akcí: Chyba, sestavování, zařazeno do fronty, zrušeno, Cancelling, úspěch.
* `feed/entry/content/properties/StatusMessage`-Zpráva podrobné informace o stavu (platí pouze pro konkrétní stavy).
* `feed/entry/content/properties/Progress`-Vytvořte průběh (%).
* `feed/entry/content/properties/StartTime`-Čas spuštění sestavení.
* `feed/entry/content/properties/EndTime`-Vytvořte koncový čas.
* `feed/entry/content/properties/ExecutionTime`-Vytvořte doba trvání.
* `feed/entry/content/properties/ProgressStep`-Podrobnosti o aktuální fáze sestavení v průběhu.

Stav platný sestavení:

* Vytvořit - sestavení požadavek položka byla vytvořena.
* Ve frontě - sestavení požadavku byla spuštěna a je ve frontě.
* Probíhá vytváření - sestavení.
* Úspěch - sestavení skončila úspěšně.
* Chyba: sestavení skončila s chybou.
* Zrušena - sestavení byla zrušena.
* Zrušení - sestavení se ruší.

Platné hodnoty pro typ sestavení:

* Pořadí - pořadí sestavení.
* Doporučení - doporučení sestavení.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="116-delete-build"></a>11.6. Odstranění sestavení
Odstraní sestavení.

POZNÁMKA: <br>Nelze odstranit aktivní sestavení. Model je třeba aktualizovat na různých active Build před odstraněním.<br>Nelze odstranit v průběhu sestavení. Sestavení by měl zrušit nejdřív voláním <strong>zrušit sestavení</strong>.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| ODSTRANIT |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| buildId |Jedinečný identifikátor sestavení. |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

### <a name="117-cancel-build"></a>11.7. Zrušit sestavení
Zruší sestavení se při vytváření stavu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| PUT |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| buildId |Jedinečný identifikátor sestavení. |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

### <a name="118-get-build-parameters"></a>11.8. Získávání parametrů sestavení
Načte sestavení parametry.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| buildId |Jedinečný identifikátor sestavení. |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Toto rozhraní API vrátí kolekci elementů klíč/hodnota. Každý prvek představuje parametr a její hodnotu:

* `feed/entry/content/properties/Key`-Vytvořte název parametru.
* `feed/entry/content/properties/Value`-Vytvořte hodnotu parametru.

Následující tabulka znázorňuje hodnotu, která představuje každý klíč.

| Klíč | Popis | Typ | Platnou hodnotu. |
|:--- |:--- |:--- |:--- |
| NumberOfModelIterations |Počet iterací, které provádí modelu se odráží celkovou dobu výpočtů a přesnosti modelu. Tím vyšší je číslo, lepší přesnost budete mít, ale bude trvat delší dobu výpočtů. |Integer |10-50 |
| NumberOfModelDimensions |Počet dimenzí má vztah k číslo modelu se pokusí najít v rámci vašich dat funkcí. Zvýšením počtu dimenzí vám umožní lépe vyladit výsledky do menší clusterů. Však příliš mnoho dimenze zabrání modelu nalézt korelací mezi položkami. |Integer |10-40 |
| ItemCutOffLowerBound |Definuje položku dolní mez pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| ItemCutOffUpperBound |Definuje položku horní mez pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| UserCutOffLowerBound |Definuje dolní mez uživatele pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| UserCutOffUpperBound |Definuje uživatele horní mez pro chladič. V tématu Použití chladič výše. |Integer |2 nebo více (0 zakázat chladič) |
| Popis |Vytvořte popis. |Řetězec |Jakýkoli text, maximální 512 znaků |
| EnableModelingInsights |Umožňuje výpočetní metriky pro model doporučení. |Logická hodnota |Hodnotu true nebo False |
| UseFeaturesInModel |Určuje funkce lze nastavit v zájmu modelu doporučení. |Logická hodnota |Hodnotu true nebo False |
| ModelingFeatureList |Čárkami oddělený seznam názvů funkcí pro použití v sestavení doporučení, aby se zvýšila doporučení. |Řetězec |Funkce názvy až 512 znaků |
| AllowColdItemPlacement |Označuje, pokud doporučení by také push cold položky prostřednictvím podobností funkci. |Logická hodnota |Hodnotu true nebo False |
| EnableFeatureCorrelation |Určuje funkce lze nastavit v reasoning. |Logická hodnota |Hodnotu true nebo False |
| ReasoningFeatureList |Čárkami oddělený seznam názvů funkcí má být použit pro odůvodnění věty (např. vysvětlení doporučení). |Řetězec |Funkce názvy až 512 znaků |

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

## <a name="12-recommendation"></a>12. Doporučení
### <a name="121-get-item-recommendations-for-active-build"></a>12.1. Získání položky doporučení (pro aktivní sestavení)
Získejte doporučení active sestavení typu "Doporučení" nebo "Fbt" založené na seznam položek semen (vstupu).

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| položky ItemID |Textový soubor s oddělovači seznam položek, doporučujeme pro. <br>Pokud je aktivní sestavení typu FBT můžete odeslat pouze jednu položku. <br>Maximální délka: 1024 |
| numberOfResults |Počet požadovaných výsledků <br> Maximální počet: 150 |
| includeMetatadata |Budoucí použití, vždy hodnotu false. |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za doporučené položky. Každá položka má následující data:

* `Feed\entry\content\properties\Id`– ID Doporučené položky.
* `Feed\entry\content\properties\Name`-Název položky.
* `Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.
* `Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).

Následující příklad odpověď obsahuje 10 doporučené položek.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
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

### <a name="122-get-item-recommendations-of-a-specific-build"></a>12.2. Získání položky doporučení (konkrétní sestavení)
Získejte doporučení konkrétní sestavení typu "Doporučení" nebo "Fbt".

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| položky ItemID |Textový soubor s oddělovači seznam položek, doporučujeme pro. <br>Pokud je aktivní sestavení typu FBT můžete odeslat pouze jednu položku. <br>Maximální délka: 1024 |
| numberOfResults |Počet požadovaných výsledků <br> Maximální počet: 150 |
| includeMetatadata |Budoucí použití, vždy hodnotu false. |
| buildId |id sestavení, který má používat pro tento požadavek doporučení |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za doporučené položky. Každá položka má následující data:

* `Feed\entry\content\properties\Id`– ID Doporučené položky.
* `Feed\entry\content\properties\Name`-Název položky.
* `Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.
* `Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).

Podívejte se na příklad odpovědi v 12.1

### <a name="123-get-fbt-recommendations-for-active-build"></a>12.3. Získejte doporučení FBT (pro aktivní sestavení)
Získejte doporučení active sestavení typu "Fbt" v závislosti na položce počáteční hodnotu (vstup).

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| itemId |Doporučujeme pro položku. <br>Maximální délka: 1024 |
| numberOfResults |Počet požadovaných výsledků <br>Maximální počet: 150 |
| minimalScore |Minimální skóre, který chcete-li být do vrácených výsledků zahrnuty by měl být časté sady |
| includeMetatadata |Budoucí použití, vždy hodnotu false. |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za sadu Doporučené položky (sady položek, které jsou obvykle koupili společně s položka počáteční hodnoty nebo vstupních). Každá položka má následující data:

* `Feed\entry\content\properties\Id1`– ID Doporučené položky.
* `Feed\entry\content\properties\Name1`-Název položky.
* `Feed\entry\content\properties\Id2`ID položky doporučené 2. (volitelné).
* `Feed\entry\content\properties\Name2`-Název 2. položka (volitelné).
* `Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.
* `Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).

Následující příklad odpověď obsahuje 3 sad Doporučené položky.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="124-get-fbt-recommendations-of-a-specific-build"></a>12.4. Získejte doporučení FBT (z konkrétní sestavení)
Získejte doporučení konkrétní sestavení typu "Fbt".

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| itemId |Doporučujeme pro položku. <br>Maximální délka: 1024 |
| numberOfResults |Počet požadovaných výsledků <br>Maximální počet: 150 |
| minimalScore |Minimální skóre, který chcete-li být do vrácených výsledků zahrnuty by měl být časté sady |
| includeMetatadata |Budoucí použití, vždy hodnotu false. |
| buildId |id sestavení, který má používat pro tento požadavek doporučení |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za sadu Doporučené položky (sady položek, které jsou obvykle koupili společně s položka počáteční hodnoty nebo vstupních). Každá položka má následující data:

* `Feed\entry\content\properties\Id1`– ID Doporučené položky.
* `Feed\entry\content\properties\Name1`-Název položky.
* `Feed\entry\content\properties\Id2`ID položky doporučené 2. (volitelné).
* `Feed\entry\content\properties\Name2`-Název 2. položka (volitelné).
* `Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.
* `Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).

Podívejte se na příklad odpovědi v 12.3

### <a name="125-get-user-recommendations-for-active-build"></a>12.5. Získejte doporučení uživatele (pro aktivní sestavení)
Získejte doporučení uživatele sestavení typu "Doporučení" označená jako aktivní sestavení.

Rozhraní API, vrátí se seznam předpokládaných položky podle historie využití uživatele.

Poznámky: 

1. Neexistuje žádný uživatel doporučení pro FBT sestavení.
2. Pokud je aktivní sestavení FBT bude tato metoda vrátí chybu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| ID uživatele |Jedinečný identifikátor uživatele |
| numberOfResults |Počet požadovaných výsledků |
| includeMetatadata |Budoucí použití, vždy hodnotu false. |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za doporučené položky. Každá položka má následující data:

* `Feed\entry\content\properties\Id`– ID Doporučené položky.
* `Feed\entry\content\properties\Name`-Název položky.
* `Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.
* `Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).

Podívejte se na příklad odpovědi v 12.1

### <a name="126-get-user-recommendations-with-item-list-for-active-build"></a>12.6. Získat doporučení uživatele s položky seznamu (pro aktivní sestavení)
Získat uživatele doporučení sestavení typu "Doporučení" označená jako aktivní sestavení s další seznam položek

Rozhraní API vrátí seznam předpokládaných položky podle historie využití uživatele a další zadané položky.

Poznámky: 

1. Neexistuje žádný uživatel doporučení pro FBT sestavení.
2. Pokud je aktivní sestavení FBT bude tato metoda vrátí chybu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| ID uživatele |Jedinečný identifikátor uživatele |
| itemsIds |Textový soubor s oddělovači seznam položek, doporučujeme pro. Maximální délka: 1024 |
| numberOfResults |Počet požadovaných výsledků |
| includeMetatadata |Budoucí použití, vždy hodnotu false. |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za doporučené položky. Každá položka má následující data:

* `Feed\entry\content\properties\Id`– ID Doporučené položky.
* `Feed\entry\content\properties\Name`-Název položky.
* `Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.
* `Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).

Podívejte se na příklad odpovědi v 12.1

### <a name="127-get-user-recommendations--of-a-specific-build"></a>12.7. Získejte doporučení uživatele (z konkrétní sestavení)
Získejte doporučení uživatele konkrétní sestavení typu "Doporučení".

Rozhraní API, vrátí se seznam předpokládaných položky podle historie využití uživatele (používá se v určitém sestavení).

Poznámka: Neexistuje žádný uživatel doporučení pro FBT sestavení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| ID uživatele |Jedinečný identifikátor uživatele |
| numberOfResults |Počet požadovaných výsledků |
| includeMetatadata |Budoucí použití, vždy hodnotu false. |
| buildId |id sestavení, který má používat pro tento požadavek doporučení |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za doporučené položky. Každá položka má následující data:

* `Feed\entry\content\properties\Id`– ID Doporučené položky.
* `Feed\entry\content\properties\Name`-Název položky.
* `Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.
* `Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).

Podívejte se na příklad odpovědi v 12.1

### <a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a>12.8. Získat doporučení uživatele s položky seznamu (konkrétní sestavení)
Získejte doporučení uživatele konkrétní sestavení typu "Doporučení" a seznam dalších položek.

Rozhraní API vrátí seznam předpokládaných položky podle historie využití uživatele a další seznam položek.

Poznámka: Tthere je žádné uživatele doporučení pro FBT sestavení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| ID uživatele |Jedinečný identifikátor uživatele |
| položky ItemID |Textový soubor s oddělovači seznam položek, doporučujeme pro. Maximální délka: 1024 |
| numberOfResults |Počet požadovaných výsledků |
| includeMetatadata |Budoucí použití, vždy hodnotu false. |
| buildId |id sestavení, který má používat pro tento požadavek doporučení |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za doporučené položky. Každá položka má následující data:

* `Feed\entry\content\properties\Id`– ID Doporučené položky.
* `Feed\entry\content\properties\Name`-Název položky.
* `Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.
* `Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).

Podívejte se na příklad odpovědi v 12.1

## <a name="13-user-usage-history"></a>13. Historie využití uživatele
Jakmile doporučení model byl vytvořený systému umožní načíst historii uživatele (položky přidružené k určitým uživatelem) používá pro sestavení.
Toto rozhraní API umožňují načíst historii uživatele

Poznámka: historie uživatelů je aktuálně k dispozici pouze pro sestavení doporučení.

### <a name="131-retrieve-user-history"></a>13.1 načíst historii uživatele
Načtení seznamu položky, které jsou používány v active sestavení nebo v zadané sestavení pro dané id daného uživatele.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |Načtení historie uživatele active sestavení.<br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/>Načtení historie uživatele pro danou sestavení`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`<br/><br/>Příklad:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu. |
| ID uživatele |Jedinečný identifikátor uživatele. |
| buildId |Volitelný parametr, povolit k označení, ze které sestavení historii uživatele by měla být načítání |
| apiVersion |1.0 |

**Odpověď:**

Kód stavu HTTP: 200

Odpověď obsahuje jeden záznam za doporučené položky. Každá položka má následující data:

* `Feed\entry\content\properties\Id`– ID Doporučené položky.
* `Feed\entry\content\properties\Name`-Název položky.
* `Feed\entry\content\properties\Rating`-NENÍ K DISPOZICI.
* `Feed\entry\content\properties\Reasoning`-NENÍ K DISPOZICI.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

## <a name="14-notifications"></a>14. Oznámení
Azure Machine Learning doporučení vytvoří oznámení, když dojde k trvalé chyby v systému. Existují 3 typy oznámení:

1. Sestavení selhání – toto oznámení se aktivuje pro každou chybu sestavení.
2. Získávání dat zpracování selhání – toto oznámení se aktivuje, když máme více než 100 chyby za posledních 5 minut při zpracování událostí využití za modelu.
3. Doporučení spotřeba selhání – toto oznámení se aktivuje, když máme více než 100 chyby za posledních 5 minut při zpracování požadavků doporučení za modelu.

### <a name="141-get-notifications"></a>14.1. Dostávat oznámení
Načte všechna oznámení pro všechny modely nebo pro jeden model.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| GET |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Získávání všechna oznámení pro všechny modely:<br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br>Příklad pro získávání oznámení pro konkrétní model.<br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Volitelný parametr. Když tento parametr vynechán, zobrazí se všechna oznámení pro všechny modely. <br>Platná hodnota: Jedinečný identifikátor modelu. |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď:**

Kód stavu HTTP: 200

OData XML

    The response includes one entry per notification. Each entry has the following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="142-delete-model-notifications"></a>14.2. Oznámení o Model odstraňovat
Odstraní všechny čtení oznámení pro model.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| ODSTRANIT |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| %{ModelID/ |Jedinečný identifikátor modelu |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

### <a name="143-delete-user-notifications"></a>14.3. Odstranit oznámení uživateli
Odstraní všechna oznámení pro všechny modely.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--- |:--- |
| ODSTRANIT |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27` |

| Název parametru | Platné hodnoty |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| Text žádosti |NONE |

**Odpověď**:

Kód stavu HTTP: 200

## <a name="15-legal"></a>15. Právní informace
Tento dokument je poskytován "jako-je". Informace a názory vyjádřené v tomto dokumentu včetně adres URL a dalších odkazů na internetové weby, mohou změnit bez předchozího upozornění.<br><br>
Některé příklady použité v ukázkách jsou jenom ilustrativní a smyšlené. Žádný skutečný vztah nebo připojení je určený nebo událostmi.<br><br>
Tento dokument neposkytuje jste žádná zákonná práva duševního vlastnictví produktů společnosti Microsoft. Můžete kopírovat a tento dokument použít pro interní referenční účely.<br><br>
© 2015 Microsoft. Všechna práva vyhrazena.

