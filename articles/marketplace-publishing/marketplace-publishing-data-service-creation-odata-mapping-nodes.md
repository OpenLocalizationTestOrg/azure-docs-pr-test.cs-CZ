---
title: "aaaGuide toocreating datové služby pro hello Marketplace | Microsoft Docs"
description: "Podrobné pokyny, jak toocreate, certifikovat a nasadit službu Data pro zakoupit na hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 107baab2-5828-4158-abdf-59a580c80d37
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: e3d88412389d43d104662dc4434363b6ad9475f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-nodes-schema-for-mapping-an-existing-web-service-tooodata-through-csdl"></a>Principy hello uzly schématu pro mapování existující tooOData webové služby prostřednictvím CSDL
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte obchodní aplikace SaaS chcete toopublish na AppSource najdete další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služba vývojáře by jako toopublish na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
>
>

Tento dokument vám pomůže vysvětlení hello uzel struktury pro mapování tooCSDL protokolu OData. Je důležité, aby struktura uzlu hello je dobře toonote formátu XML. Kořenové, nadřazených a podřízených schématu tak lze použít při navrhování vaší mapování OData.

## <a name="ignored-elements"></a>Ignoruje elementy
Hello následují hello vysoké úrovně CSDL elementů (z uzlů XML), nebudete toobe používané back-end Azure Marketplace hello během importu hello metadat hello webové služby. Mohou existovat, ale budou ignorovány.

| Element | Rozsah |
| --- | --- |
| Pomocí elementu. |Hello uzlu, dílčí uzly a všech atributů |
| Documentation Element |Hello uzlu, dílčí uzly a všech atributů |
| Typ ComplexType |Hello uzlu, dílčí uzly a všech atributů |
| Element přidružení |Hello uzlu, dílčí uzly a všech atributů |
| Rozšířené vlastnosti |Hello uzlu, dílčí uzly a všech atributů |
| EntityContainer |Pouze hello následující atributy se ignorují: *rozšiřuje* a *AssociationSet* |
| Schéma |Pouze hello následující atributy se ignorují: *Namespace* |
| Element FunctionImport |Pouze hello následující atributy se ignorují: *režimu* (ln se předpokládá výchozí hodnota) |
| Typ entity |Pouze hello následující dílčí uzly se ignorují: *klíč* a *PropertyRef* |

Hello následující text popisuje toohello změny (Přidat a ignoruje elementy) hello různé z uzlů CSDL XML podrobně.

## <a name="functionimport-node"></a>Element FunctionImport uzlu
Uzel FunctionImport představuje jeden URL (vstupního bodu), který zveřejňuje koncového uživatele služby toohello. uzel Hello umožňuje popisující, jak je řešit hello adresu URL, které parametry jsou k dispozici toohello koncového uživatele a jak jsou dostupné tyto parametry.

Podrobnosti o tomto uzlu se nacházejí v [sem][MSDNFunctionImportLink](https://msdn.microsoft.com/library/cc716710.aspx)

Hello následují další atributy, které hello (nebo přidání tooattributes) jsou vystavené hello FunctionImport uzlu:

**d:BaseUri** -hello URI šablony pro prostředek REST hello, který je zveřejněné tooMarketplace. Marketplace používá hello šablony tooconstruct dotazy proti hello webové služby REST. Hello URI šablona obsahuje zástupné symboly pro parametry hello hello tvar {parameterName}, kde název parametru je hello název parametru hello. Příklad: apiVersion = {apiVersion}.
Parametry jsou povoleny tooappear jako parametry URI nebo jako součást cesta hello URI. V případě hello hello výskytu v cestě hello jsou vždy povinné (nemohou být označeny jako s možnou hodnotou Null). *Příklad:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Název** -hello název hello importu funkce.  Nemůže být hello stejné jako jiné názvy definované v hello CSDL.  Příklad: Název = "GetModelUsageFile"

**Objekt EntitySet** *(volitelné)* – Pokud hello funkce vrátí kolekci typů entit, hello hodnotu hello **EntitySet** musí být hello entity sadu toowhich hello kolekce patří. V opačném hello **EntitySet** nesmí se používat atribut. *Příklad:*`EntitySet="GetUsageStatisticsEntitySet"`

**Vlastnost ReturnType** *(volitelné)* – Určuje typ hello elementů vrácený hello identifikátor URI.  Tento atribut nepoužívejte, pokud hello funkce nevrací hodnotu. Hello následují hello podporované typy:

* **Kolekce (<Entity type name>)**: Určuje kolekci typů entit definované. Název Hello není k dispozici v atributu název hello hello EntityType uzlu. Příkladem je kolekce (WXC. HourlyResult).
* **Nezpracovaná (<mime type>)**: Určuje, nezpracované dokumentu nebo objekt blob, který je vrácen toohello uživatele. Příklad je Raw(image/jpeg) Další příklady:

  * ReturnType="Raw(text/plain)"
  * Vlastnost ReturnType = "kolekce (sage. DeleteAllUsageFilesEntity) "*

**d:paging** -Určuje, jak je stránkování zpracovávat hello prostředek REST. Hello parametr hodnoty jsou používány ve složené odvětví, například stránka = {$page} & vlastnost itemsperpage = {$size} hello dostupné jsou možnosti:

* **Žádné:** je k dispozici žádné stránkování
* **Přeskočit:** stránkování je vyjádřit pomocí logických "Přeskočit" a "vzít" (nahoře). Přeskočit skáče přes M elementy a proveďte pak vrátí hello další prvky N. Hodnota parametru: $skip
* **Proveďte:** proveďte vrátí další elementy N hello. Hodnota parametru: $take
* **PageSize:** stránkování je vyjádřit pomocí logické stránky a velikost (počet položek na stránku). Představuje stránku hello aktuální stránku, která je vrácena. Hodnota parametru: $page
* **Velikost:** velikost představuje hello počet vrácených pro jednotlivé stránky položek. Hodnota parametru: $size

**d:AllowedHttpMethods** *(volitelné)* -Určuje, které operace se zpracovává souborem hello prostředek REST. Také omezuje toohello přijala operaci zadané hodnoty.  Výchozí = POST.  *Příklad:* `d:AllowedHttpMethods="GET"` hello dostupné jsou možnosti:

* **GET:** obvykle používají tooreturn dat
* **POST:** obvykle používají tooinsert nová data
* **PUT:** obvykle používají tooupdate dat
* **ODSTRANĚNÍ:** používá toodelete dat

Další podřízené uzly (nevztahuje hello CSDL dokumentace) v rámci uzlu FunctionImport hello jsou:

**d:RequestBody** *(volitelné)* -hello požadavek textu je použité tooindicate, který hello požadavek očekává textu toobe, odeslána. Parametry lze zadat v textu požadavku hello. Jsou vyjádřeny v rámci složené závorky, například {parameterName}. Tyto parametry nebyly mapovány z hello uživatelský vstup do hello text, který se přenáší toohello poskytovateli obsahu služby. Hello requestBody element má atribut s názvem httpMethod. atribut Hello umožňuje dvou hodnot:

* **POST:** použít, pokud je hello požadavek HTTP POST
* **GET:** použít, pokud je hello požadavek HTTP GET

    Příklad:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:namespaces** a **d:Namespace** -tento uzel popisuje hello obory názvů, které jsou definovány v hello XML, který je vrácené importem funkce hello (URI koncového bodu). Hello XML, který je vrácen hello back-end službu může obsahovat libovolný počet obory názvů toodifferentiate hello obsah, který je vrácen. **Všechny tyto obory názvů, pokud se používá v d:Map nebo d:Match dotazů XPath musí toobe uvedené.** uzel d:Namespaces Hello obsahuje sadu nebo seznam d:Namespace uzlů. Každý z nich uvádí jeden obor názvů použít v odpovědi služby back-end hello. Hello následují hello atribut hello d:Namespace uzlu:

* **d:prefix:** hello předponu pro obor názvů hello, jak je vidět v hello XML výsledky vrácené službou hello, např. f:FirstName, f:LastName, kde f je předpona hello.
* **d:URI:** hello úplný identifikátor URI oboru názvů hello v dokumentu výsledek hello. Reprezentuje hodnotu hello tuto předponu hello je vyřešen tooat runtime.

**d:ErrorHandling** *(volitelné)* -tento uzel obsahuje podmínky pro zpracování chyb. Všech hello podmínek bude ověřeno hello výsledek, která je vrácena službou hello poskytovateli obsahu. Pokud podmínka odpovídá hello navrhované kód chyby protokolu HTTP se vrátí chybovou zprávu toohello koncového uživatele.

**d:ErrorHandling** *(volitelné)* a **d:Condition** *(volitelné)* -uzel podmínek obsahuje jednu podmínku, která je vrácena se změnami hello výsledek vrácený Služba Hello poskytovateli obsahu. Hello následují hello **požadované** atributy:

* **d:match:** výraz XPath, který ověřuje, zda je daný uzel hodnota v poskytovateli obsahu hello výstupu XML. Hello XPath spustí pro výstup hello a by měla vrátit hodnotu true, pokud je podmínka hello shodu nebo hodnotu NEPRAVDA, jinak hodnota.
* **d:HttpStatusCode:** hello stavový kód protokolu HTTP, který má být vrácen Marketplace v hello případu hello podmínky shody. Marketplace signalizes chyby toohello uživatele prostřednictvím stavové kódy HTTP. Seznam stavové kódy HTTP jsou k dispozici na http://en.wikipedia.org/wiki/HTTP_status_code
* **d:errorMessage:** hello chybovou zprávu, která je vrácená – s kódem stavu hello HTTP – toohello koncového uživatele. To by měl být popisný chybovou zprávu, která neobsahuje žádné tajných klíčů.

**d:Title** *(volitelné)* -umožňuje popisující hello název funkce hello. Hello hodnotu pro název hello pochází z

* Hello volitelné mapy atribut (xpath), která určuje, kde název hello toofind v odpovědi hello vrácených hello žádost o služby.
* - Nebo - title hello zadaný jako hodnota hello uzlu.

**d:Rights** *(volitelné)* -hello práva (například autorským) přiřazená hello funkce. Hodnota Hello hello práva pochází od:

* Hello volitelné mapy atribut (xpath), která určuje, kde toofind hello práva v hello odpověď vrácená z hello žádost o služby.
* - Nebo - hello práva zadaný jako hodnota hello uzlu.

**d:Description** *(volitelné)* – krátkodobých popis funkce hello. Hodnota Hello hello popis pochází z

* Hello volitelné mapy atribut (xpath), která určuje, kde toofind hello popis v odpovědi hello vrácených hello žádost o služby.
* - Nebo – popis hello zadaný jako hodnota hello uzlu.

**d:EmitSelfLink** - *. viz výše příklad "FunctionImport 'stránkování, prostřednictvím vrácená data"*

**d:EncodeParameterValue** -tooOData volitelné rozšíření

**d:QueryResourceCost** -tooOData volitelné rozšíření

**d:map** -tooOData volitelné rozšíření

**d:Headers** -tooOData volitelné rozšíření

**d:Headers** -tooOData volitelné rozšíření

**d:value** -tooOData volitelné rozšíření

**d:HttpStatusCode** -tooOData volitelné rozšíření

**d:errorMessage** -tooOData volitelné rozšíření

## <a name="parameter-node"></a>Uzel parametru
Tento uzel představuje jeden parametr, který je zveřejněný v rámci šablony URI hello / text, který byl zadaný v elementu FunctionImport uzlu hello žádosti.

Na stránce dokumentu velmi užitečné informace o uzlu hello "Prvek parametru" se nachází zde [zde](http://msdn.microsoft.com/library/ee473431.aspx) (použití hello **další verze** rozevírací tooselect jinou verzi, pokud nezbytné tooview hello dokumentaci). *Příklad:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Atribut parametru | Je vyžadován | Hodnota |
| --- | --- | --- |
| Name (Název) |Ano |Hello název parametru hello. Velká a malá písmena!  Malá a velká písmena BaseUri hello. **Příklad:**`<Property Name="IsDormant" Type="Byte" />` |
| Typ |Ano |Typ parametru Hello. Hello hodnota musí být **EDMSimpleType** nebo komplexní typ, který je v rozsahu hello hello modelu. Další informace najdete v části "6 typy podporované parametr nebo vlastnost".  (Rozlišuje velká a malá písmena! První znak je velké písmeno, rest jsou malá písmena.)  Také najdete [konceptuálního modelu typy (CSDL)][MSDNParameterLink](http://msdn.microsoft.com/library/bb399548.aspx). **Příklad:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Režim |Ne |**V**, Out nebo InOut, v závislosti na tom, zda je parametr hello vstupní, výstupní nebo vstupní a výstupní parametr. ("V" je k dispozici jen v Azure Marketplace.) **Příklad:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| Hodnota maxLength |Ne |Hello maximální povolenou délku parametru hello. **Příklad:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| přesnost |Ne |přesnost Hello hello parametru. **Příklad:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Měřítko |Ne |škálování Hello hello parametru. **Příklad:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

Hello následují hello atributy, které byly přidány toohello CSDL specifikace:

| Atribut parametru | Popis |
| --- | --- |
| **d:Regex** *(volitelné)* |Příkaz regex použitý toovalidate hello vstupní hodnota pro parametr hello. Pokud vstupní hodnoty hello se neshoduje se hodnota hello příkaz hello byl odmítnut. To umožňuje toospecify také sadu možných hodnot, například ^ [0-9] +? $ tooonly povolit čísla. **Příklad:** ' < název parametru = "name" režimu = "V" typ = "Řetězec" d: null = "false" d:Regex = "^ [a-zA-Z] * $" d:Description = "A název, který nemůže obsahovat žádné mezery nebo jiné než alfanumerické znaky jiné než anglické" d:SampleValues = "Jirka |
| **d:enum** *(volitelné)* |Svislicí oddělený seznam hodnot pro parametr hello platná. Typ Hello hodnot hello musí toomatch hello definovaný typ parametru hello. Příklad:, angličtina |
| **d: Null** *(volitelné)* |Umožňuje definovat, zda parametr může mít hodnotu null. Hello výchozí hodnota je: true. Parametry, které jsou zveřejněné jako část cesty hello v šabloně URI hello však nemůže mít hodnotu null. Když nastavený atribut hello toofalse pro tyto parametry – vstup uživatele hello přepsána. **Příklad:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(volitelné)* |Ukázka toodisplay hodnotu jako poznámka toohello klienta v hello uživatelského rozhraní.  Je možné tooadd několik hodnot pomocí kanálu oddělený seznam, tj. |

## <a name="entitytype-node"></a>Typ entity uzlu
Tento uzel představuje jeden z typů hello, které jsou vráceny z Marketplace toohello koncového uživatele. Obsahuje taky hello mapování z hello výstupu, který je vrácen hello poskytovateli obsahu služby toohello hodnoty, které jsou vráceny toohello koncového uživatele.

Podrobnosti o tomto uzlu se nacházejí v [sem](http://msdn.microsoft.com/library/bb399206.aspx) (použití hello **další verze** rozevírací tooselect jinou verzi, pokud nezbytné tooview hello dokumentaci.)

| Název atributu | Je vyžadován | Hodnota |
| --- | --- | --- |
| Name (Název) |Ano |Název Hello hello typu entity. **Příklad:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| Základní typ BaseType |Ne |Hello název jiného typu entity, která je základní typ hello hello typu entity, který je definovaný. **Příklad:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Hello následují hello atributy, které byly přidány toohello CSDL specifikace:

**d:map** -výraz XPath spustit pro výstup hello služby. Hello předpokládá zde je, že služba výstup hello obsahuje sadu elementů, které se opakují, jako ATOM kanálu, kde je sada uzlů položky, které se opakují. Každá z těchto opakující se uzly obsahuje jeden záznam. Hello XPath je zadaný toopoint v jednotlivých opakovaných uzlu hello ve výsledku služby hello poskytovateli obsahu, který obsahuje hello hodnoty pro jednotlivé záznamy. Příklad výstupu z hello služby:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

Hello výraz XPath by /foo/bar, protože každý uzel panelu hello je hello opakující se uzel v hello výstup a obsahuje hello skutečný obsah, který je vrácen toohello koncového uživatele.

**Klíč** -tento atribut je ignorován v Marketplace. REST na základě webové služby, obecně Nevystavujte primární klíč.

## <a name="property-node"></a>Vlastnosti uzlu
Tento uzel obsahuje jednu vlastnost hello záznamu.

Podrobnosti o tomto uzlu se nacházejí v [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (použití hello **další verze** rozevírací tooselect jinou verzi, pokud nezbytné tooview hello dokumentaci.) *Příklad:*`<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"     Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Požaduje se | Hodnota |
| --- | --- | --- |
| Name (Název) |Ano |Hello název vlastnosti hello. |
| Typ |Ano |Typ Hello hello hodnotu vlastnosti. musí být typ hodnoty vlastnosti Hello **EDMSimpleType** nebo komplexní typ (označená plně kvalifikovaný název), který je v rozsahu hello modelu. Další informace najdete v tématu typy konceptuální Model (CSDL). |
| S možnou hodnotou Null |Ne |**Hodnota TRUE,** (hello výchozí hodnota) nebo **False** v závislosti na tom, jestli vlastnost hello může mít hodnotu null. Poznámka: V hello verzi CSDL indikován hello [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) obor názvů, vlastnost komplexního typu musí mít Nullable = "False". |
| Výchozí hodnota |Ne |Výchozí hodnota Hello hello vlastnosti. |
| Hodnota maxLength |Ne |Hello maximální délka hodnoty vlastnosti hello. |
| Řetězci FixedLength |Ne |**Hodnota TRUE,** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti hello jako řetězec délky fiexed. |
| přesnost |Ne |Odkazuje toohello maximální počet číslic tooretain v hello číselná hodnota. |
| Měřítko |Ne |Maximální počet desetinných míst tooretain v hello číselná hodnota. |
| Kódování Unicode |Ne |**Hodnota TRUE,** nebo **False** v závislosti na tom, jestli se hodnota vlastnosti hello uložené jako řetězec znaků Unicode. |
| Kolace |Ne |Řetězec, který určuje hello používá ve zdroji dat hello toobe pořadí řazení. |
| Režim ConcurrencyMode |Ne |**Žádný** (hello výchozí hodnota) nebo **pevné**. Pokud je příliš nastavena hodnota hello**pevné**, hodnota vlastnosti hello se použije v optimistickou metodu souběžného kontroly. |

Hello následují hello další atributy, které byly přidány toohello CSDL specifikace:

**d:map** -výraz XPath u služby hello spustit výstup a extrahuje jednu vlastnost hello výstupu. Hello zadaný výraz XPath je relativní toohello opakující se uzel, který byl zvolen v uzlu EntityType hello XPath. Je také možné toospecify absolutní tooallow XPath včetně statické prostředků v každé z hello výstupních uzlů, jako je například autorským příkaz, který se nachází pouze po v hello výstup původní služby, ale měl by být ve všech řádků hello v hello OData výstup. Příklad ze služby hello:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

výraz XPath Hello tady by ./bar/baz0 tooget hello baz0 uzlu ze služby hello poskytovateli obsahu.

**d:CharMaxLength** – pro typ řetězce, můžete zadat maximální délka hello. Viz příklad DataService CSDL

**d:IsPrimaryKey** -značí, zda text hello sloupec hello primární klíč v hello tabulky či zobrazení. Podívejte se na příklad DataService CSDL.

**d:isExposed** -Určuje, pokud je vystaven schématu tabulky hello (obvykle true). Viz příklad DataService CSDL

**d:IsView** *(volitelné)* – hodnota true, pokud to je založené na zobrazení a nikoli na tabulku.  Viz příklad DataService CSDL

**d:Tableschema** – viz příklad DataService CSDL

**d:ColumnName** -je název hello hello sloupce v hello tabulky či zobrazení.  Viz příklad DataService CSDL

**d:IsReturned** -je hello logická hodnota, která určuje, pokud hello služby zpřístupní tohoto klienta toohello hodnotu.  Viz příklad DataService CSDL

**d:IsQueryable** -je hello logická hodnota, která určuje, pokud sloupec hello lze použít v dotazu databáze.   Viz příklad DataService CSDL

**d:OrdinalPosition** -je sloupec hello číselné pozice výskytu, x, v hello tabulka nebo zobrazení hello, kde x je od 1 toohello počtu sloupců v tabulce hello.  Viz příklad DataService CSDL

**d:DatabaseDataType** -je hello datový typ sloupce hello v databázi hello, tj. datový typ SQL. Viz příklad DataService CSDL

## <a name="supported-parametersproperty-types"></a>Typy podporované parametry nebo vlastnost
Hello Toto jsou typy hello podporován pro parametry a vlastnosti. (Malá a velká písmena)

| Primitivní typy | Popis |
| --- | --- |
| Hodnotu Null |Představuje nepřítomnost hello |
| Logická hodnota |Představuje hello matematickém koncept s hodnotou binární logiky |
| Bajtů |Celé číslo bez znaménka 8bitové hodnoty |
| Data a času |Představuje data a času s hodnotami v rozsahu od půlnoci 12:00:00, 1, 1753. ledna prostřednictvím 11:59:59 P.M, prosince 9999 A.D. |
| Decimal |Představuje číselných hodnot s pevnou přesnost a měřítko. Tento typ, ale popíše číselnou hodnotu v rozsahu od 10 záporné ^ 255 + 1 toopositive 10 ^ 255 -1 |
| Double |Představuje bod plovoucí desetinné číslo s přesností na 15 číslic, která představuje hodnoty s přibližnou rozsah rozmezí 2, 23E-308 odchylkou až 1, 79E +308. **Použití Decimal kvůli tooExel export problém** |
| Jeden |Představuje bod plovoucí desetinné číslo s přesností 7 míst, která představuje hodnoty s přibližnou rozsah rozmezí 1, 18E-38 odchylkou až 3, 40E +38 |
| Identifikátor GUID |Reprezentuje hodnotu jedinečný identifikátor 16 bajtů (128 bitů) |
| Int16 |Reprezentuje hodnotu podepsaný 16bitové celé číslo |
| Int32 |Reprezentuje hodnotu podepsaný 32bitové celé číslo |
| Int64 |Reprezentuje hodnotu podepsaný 64bitové celé číslo |
| Řetězec |Představuje pevné nebo proměnná délky textová data |

## <a name="see-also"></a>Viz také
* Pokud vás zajímá Principy hello celý proces mapování OData a účel, přečtěte si tento článek [mapování dat služby OData](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definice struktury a pokynů.
* Pokud vás zajímá kontrola příklady, přečtěte si tento článek [Data služby OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee ukázkový kód a pochopit syntaxe kódu a kontext.
* tooreturn toohello předepsané cestu pro publikování dat služby toohello Azure Marketplace, přečtěte si tento článek [Průvodce publikování dat služby](marketplace-publishing-data-service-creation.md).
