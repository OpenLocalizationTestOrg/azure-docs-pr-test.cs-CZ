---
title: "aaaGuide toocreating datové služby pro hello Marketplace | Microsoft Docs"
description: "Podrobné pokyny, jak toocreate, certifikovat a nasadit službu Data pro zakoupit na hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3a632825-db5b-49ec-98bd-887138798bc4
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: deb2e52dd03f5beb2ad6a927bd2d03e47d20b691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mapping-an-existing-web-service-tooodata-through-csdl"></a>Mapování existující tooOData webové služby prostřednictvím CSDL
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte obchodní aplikace SaaS chcete toopublish na AppSource najdete další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služba vývojáře by jako toopublish na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Tento článek poskytuje přehled o tom, toouse CSDL toomap existující služby tooan kompatibilní služby OData. Vysvětluje, jak toocreate hello mapování dokument (CSDL), který transformuje hello vstupní požadavek od klienta hello prostřednictvím volání služby a hello výstup (data) zpět toohello klienta prostřednictvím kompatibilní datového kanálu OData. Microsoft Azure Marketplace zveřejňuje koncoví uživatelé toohello služby pomocí protokolu OData hello. Služby, které jsou vystavené poskytovatelů obsahu (Data vlastníky) jsou zveřejněné v různých formulářů, například SOAP, REST, atd.

## <a name="what-is-a-csdl-and-its-structure"></a>Co je CSDL a jeho struktura?
CSDL (koncepční Schema Definition Language) je specifikace definování jak toodescribe webové služby nebo databáze služby společné XML tento problém toohello Azure Marketplace.

Jednoduchý přehled hello **požadavku toku:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

Hello **tok dat** v hello opačným směrem:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Obrázek 1** diagramy, jak klient získat data z obsahu zprostředkovatele (služba) prostřednictvím hello Azure Marketplace.  používá Hello CSDL hello mapování/transformační součást toohandle hello požadavku a předejte data mezi hello obsahu poskytovatele služeb a hello požadavku klienta.

*Obrázek 1: Podrobné tok od žádajících poskytovatel toocontent klienta prostřednictvím Azure Marketplace*

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Pozadí na Atom, Atom Pub a protokolu OData hello, při které hello sestavení rozšíření Azure Marketplace, najdete v tématu: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Výňatek ze výše odkaz: *"hello účel hello Open Data protocol (dále jen odkazované tooas OData) je protokol tooprovide bázi REST pro operace CRUD stylu (vytvoření, čtení, aktualizaci a odstraňování) s prostředky, které jsou zveřejněné jako datové služby. "Služba dat" je koncový bod tam, kde se nacházejí data z jedné nebo více "kolekcí" každý s nula nebo více "položky", které se skládají z zveřejněné zadali páry název hodnota. OData je publikovaná společností Microsoft v rámci standardy OASIS (organizace pro hello rozvoj strukturovaných standardy informace), aby každý uživatel, který chce toocan sestavení nástroje bez licenčních nebo omezení, klientů nebo serverů."*

### <a name="three-critical-pieces-that-have-toobe-defined-by-hello-csdl-are"></a>Jsou tři důležité údaje, které mají toobe definované hello CSDL:
* Hello **koncový bod** z hello poskytovatele služeb hello webovou adresu (URI) hello služby
* Hello **parametrů dat** předávány jako vstupní toohello poskytovatele služeb hello Definice parametrů hello odesílány toohello poskytovateli obsahu služby dolů toohello datového typu.
* **Schéma** hello dat nevrátila toohello požaduje služba hello schéma dat hello doručován službou hello poskytovatele obsahu, včetně kontejneru, kolekce nebo tabulky, proměnné nebo sloupce a datové typy.

Hello následující diagram ukazuje přehled hello toku z kde hello klienta do hello – příkaz (volání toohello poskytovatele obsahu webová služba) toogetting hello výsledků nebo dat OData zpět.

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>pomocí následujících kroků:
1. Klient odešle požadavek prostřednictvím volání služby kompletní s vstupní parametry definované v XML toohello Azure Marketplace
2. CSDL je použité toovalidate hello volání služby.
   * Hello formátu volání služby pak odesílají toohello služba obsah zprostředkovatelé hello Azure Marketplace
3. Hello webová služba spustí a preforms hello akce hello příkaz Http (tj. získat) hello data jsou vrácena tooAzure Marketplace, kde hello požadovaná data (pokud existuje) je zpřístupňuje ve formátu XML toohello klienta pomocí mapování definované v hello CSDL hello.
4. Hello klienta se odesílá hello data (pokud existuje) ve formátu XML nebo JSON

## <a name="definitions"></a>Definice
### <a name="odata-atom-pub"></a>Protokol pub OData ATOM
Nastavit rozšíření toohello ATOM pub kde každá položka představuje jeden řádek výsledků. Hello obsahu součástí hello položka je rozšířené toocontain hello hodnoty řádku hello – jako párů klíčových hodnot. Další informace naleznete zde: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - Conceptual Schema Definition Language
Umožňuje definovat funkce (SPROCs) a entity, které jsou k dispozici prostřednictvím databáze. Další informace, které se nachází tady: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [!TIP]
> Klikněte na tlačítko hello **jiné verze** rozevírací seznam a vyberte verzi, pokud nevidíte hello článku.
> 
> 

### <a name="edm---entry-data-model"></a>EDM - vstupní datový Model
* Přehled: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* Ve verzi Preview: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* Datové typy: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

Následující Hello zobrazuje tok levém tooRight ze kde hello klienta do hello – příkaz (volání toohello poskytovatele obsahu webová služba) toogetting hello výsledků nebo dat OData zpět podrobné hello:

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a>Základy CSDL
CSDL (koncepční Schema Definition Language) je specifikace definování jak toodescribe webové služby nebo databáze služby společné XML tento problém toohello Azure Marketplace. Popisuje CSDL hello kritické kusy, který **umožňuje hello předávání dat ze zdroje dat toohello hello Azure Marketplace.** Hello hlavní části jsou popsány zde:

* Rozhraní informace, které popisují všechny veřejně dostupné funkce (FunctionImport uzel)
* Datový typ informace pro všechny zprávy requests(input) a zpráva responses(outputs) (EntityContainer nebo objekt EntitySet/EntityType uzlů)
* Informace o hello přenosu protokolu toobe vazby použít (záhlaví uzlu)
* Informace o adrese pro vyhledání hello zadané služby (BaseURI atribut)

Stručně řečeno hello CSDL představuje kontraktu nezávislé na platformě a jazyku mezi hello služby žadatel a hello poskytovatele služeb. Pomocí hello CSDL, můžete klienta najít webové služby a databáze služby a vyvolání svých veřejně dostupné úkolů.

### <a name="relating-a-csdl-tooa-database-or-a-collection"></a>Související CSDL tooa databáze nebo kolekce
**Hello CSDL specifikace**

CSDL se gramatika XML pro popis webové služby. Specifikace Hello, samotné je rozdělené do 4 důležité elementy: EntitySet, FunctionImport; Obor názvů a typ EntityType.

toomake umožňuje snazší toounderstand tato abstrakce tabulku tooa CSDL se týkají.

Mějte na paměti,

  CSDL představuje kontraktu nezávislé na platformě a jazyku mezi hello **služby žadatele** a hello **poskytovatele služeb**. Pomocí CSDL, **klienta** mohou vyhledat **webové služby a databáze služby** a spustit žádné jeho veřejně dostupné **funkce.**

Pro Data služby hello čtyři části CSDL si lze představit z hlediska databáze, tabulky, sloupce a proceduře úložiště.

Týkající se těchto takto pro datové služby:

* EntityContainer ~ = databáze
* Objekt EntitySet ~ = tabulky
* EntityType ~ = sloupců
* Element FunctionImport ~ = uložené procedury

**Akce HTTP povolena**

* GET – vrátí hodnoty z databáze hello (vrátí kolekci)
* POST – použít toopass tooand volitelné návratové hodnoty dat z databáze hello (vytvořit novou položku v kolekci hello, návratový id nebo identifikátor URI)
* ODSTRANIT – odstranění dat z hello DB (odstraní kolekce)
* PUT – aktualizace dat do DB (nahraďte kolekci nebo ji vytvořte)

## <a name="metadatamapping-document"></a>Dokument metadat/mapování
Dokument metadat/mapování Hello je použité toomap, které poskytovateli obsahu existující webové služby, aby mohou být zpřístupněny jako webovou službu OData systémem hello Azure Marketplace. Je založena na CSDL a implementuje několik rozšíření tooCSDL tooaccommodate hello potřebám REST na základě webové služby, které jsou k dispozici prostřednictvím Azure Marketplace. rozšíření Hello se nacházejí v hello [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) oboru názvů.

Příklad hello CSDL: (kopírování a vkládání hello následujícím příkladu CSDL do editoru XML a změňte toomatch služby.  Vložte do mapování CSDL kartě DataService při vytváření služby v hello [publikování portálu Azure Marketplace](https://publish.windowsazure.com)).

**Podmínky:** pokud jde o hello CSDL podmínky toohello [publikování portál](https://publish.windowsazure.com) podmínky uživatelského rozhraní (PPUI).

* Nabízí "Title" v hello PPUI má vztah tooMyWebOffer
* Moje_firma v hello PPUI má vztah příliš**zobrazovaný název vydavatele** v hello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) uživatelského rozhraní
* Rozhraní API má vztah tooa Web nebo dat služby (plán v hello PPUI)

**Hierarchie:** společnost (poskytovateli obsahu) vlastní tyto nabídky, které mají/í, konkrétně service(s), který řádek nahoru rozhraním API.

### <a name="webservice-csdl-example"></a>Příklad CSDL webovou službu
Připojí tooa služba, která je vystavení koncový bod webové aplikace (např. aplikaci C#)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all hello web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition near hello bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is hello entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines hello service call, replace & with hello encode value (&amp;).
        In hello input name value pairs {param} represents passed in value.
        Or hello value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows hello CSDL toospecifically specify hello verb of hello service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes hello parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how hello web service call is exposed toomarketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, hello {...} point toohello parameters defined below.
        Marketplace will read hello parameters from this URITemplate and fill hello values into hello corresponding parameters of hello BaseUri or RequestBody (below) during calls tooyour service.  
        It is okay for @d:BaseUri tooinclude information only for Marketplace consumption, it will not be exposed tooend users. i.e. hello hardcoded AccountKey in hello above BaseURI does not show up in hello client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of hello RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders tooinsert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how toopass userid and password via hello header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed tooend users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with hello value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited tooappearing as hello value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used toospecify values tooinsert into hello ATOM feed returned toohello end user.  You can specify hello element toocontain a fixed message by providing a value for hello element (this is hello default value).  @d:Map is an XPath expression that points into hello response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay tooalso add a d:Map attribute toooverride this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of hello web service call:
        @Name should match exactly (case sensitive) hello {…} placeholders in hello @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether hello parameter is required.
        @d:Regex - optional, attribute toodescribe hello string, limiting unwanted input at hello entry of hello system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in hello Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating hello service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in hello order listed.
        @d:Match is an Xpath query on hello service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is hello error code tooreturn if an response matches hello error.
        @d:errorMessage is hello user friendly message tooreturn when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect toohello service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- hello EntityContainer defines hello output data schema -->
        </EntityContainer>
        <!-- hello EntityType @d:Map defines hello repeating node (an XPath query) in hello response (output data schema). -->
        <!-- If these nodes are outside a namespace, add hello prefix in hello xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in hello ATOM feed, so comply with hello XML element naming restrictions (no spaces or other illegal characters).
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query toopoint at hello location tooextract hello content from your services response.
        hello "." is relative toohello repeating node in hello EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID"    d:IsPrimaryKey="True" Type="Int32"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount"    Type="Double"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"    Type="String"    Nullable="false" d:Map="./City"/>
        <Property Name="State"    Type="String"    Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime"    Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [!TIP]
> Zobrazit další příklady CSDL webové služby v článku hello [příklady mapování existující webové služby tooOData prostřednictvím CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)
> 
> 

### <a name="dataservice-csdl-example"></a>Příklad DataService CSDL
Připojí tooa služba, která je vystavení databázové tabulky nebo zobrazení, protože koncový bod následující příklad ukazuje dva rozhraní API pro Data základní založené na rozhraní API CSDL (můžete použít zobrazení, nikoli tabulky).

        <?xml version="1.0"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all hello data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of hello EntitySet as a Service
        @Name is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); hello table (or view) and columns toobe returned by hello data service.)
            Map is hello schema.tabel or schema.view
            dals.TableName is hello table Name
            Name is hello name identifier for hello EntityType and hello Name of hello service exposed toohello client via hello UI.
            dals:IsExposed determines if hello table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is hello schema name of hello table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines hello column properties and hello output of hello service.
            dals:ColumnName is hello name of hello column in hello table /view.
            Type is hello emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is hello maximum length of hello output value
            Name is hello name of hello Property and is exposed toohello client facing UI
            dals:IsReturned is hello Boolean that determines if hello Service exposes this value toohello client.
            IsQueryable is hello Boolean that determines if hello column can be used in a database query
            (For data Services: tooimprove Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is hello numerical position x in hello table or hello View, where x is from 1 toohello number of columns in hello table.
            dals:DatabaseDataType is hello data type of hello column in hello database, i.e. SQL data type dals:IsPrimaryKey indicates if hello column is hello Primary key in hello table/view.  (hello columns marked ISPrimaryKey are used in hello Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Viz také
* Pokud vás zajímá učení a pochopení hello konkrétní uzlů a jejich parametrů, přečtěte si tento článek [datové služby OData mapování uzly](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) pro definice a vysvětlení, příklady a kontext případů použití.
* Pokud vás zajímá kontrola příklady, přečtěte si tento článek [Data služby OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee ukázkový kód a pochopit syntaxe kódu a kontext.
* tooreturn toohello předepsané cestu pro publikování dat služby toohello Azure Marketplace, přečtěte si tento článek [Průvodce publikování dat služby](marketplace-publishing-data-service-creation.md).

