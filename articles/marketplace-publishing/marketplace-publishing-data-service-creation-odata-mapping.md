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
# <a name="mapping-an-existing-web-service-tooodata-through-csdl"></a><span data-ttu-id="edb93-103">Mapování existující tooOData webové služby prostřednictvím CSDL</span><span class="sxs-lookup"><span data-stu-id="edb93-103">Mapping an existing web service tooOData through CSDL</span></span>
> [!IMPORTANT]
> <span data-ttu-id="edb93-104">**V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.**</span><span class="sxs-lookup"><span data-stu-id="edb93-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="edb93-105">Pokud máte obchodní aplikace SaaS chcete toopublish na AppSource najdete další informace [zde](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="edb93-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="edb93-106">Pokud máte IaaS aplikace nebo služba vývojáře by jako toopublish na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="edb93-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="edb93-107">Tento článek poskytuje přehled o tom, toouse CSDL toomap existující služby tooan kompatibilní služby OData.</span><span class="sxs-lookup"><span data-stu-id="edb93-107">This article gives an overview on how toouse a CSDL toomap an existing service tooan OData compatible service.</span></span> <span data-ttu-id="edb93-108">Vysvětluje, jak toocreate hello mapování dokument (CSDL), který transformuje hello vstupní požadavek od klienta hello prostřednictvím volání služby a hello výstup (data) zpět toohello klienta prostřednictvím kompatibilní datového kanálu OData.</span><span class="sxs-lookup"><span data-stu-id="edb93-108">It explains how toocreate hello mapping document (CSDL) that transforms hello input request from hello client via a service call and hello output (data) back toohello client via an OData compatible feed.</span></span> <span data-ttu-id="edb93-109">Microsoft Azure Marketplace zveřejňuje koncoví uživatelé toohello služby pomocí protokolu OData hello.</span><span class="sxs-lookup"><span data-stu-id="edb93-109">Microsoft’s Azure Marketplace exposes services toohello end-users by using hello OData protocol.</span></span> <span data-ttu-id="edb93-110">Služby, které jsou vystavené poskytovatelů obsahu (Data vlastníky) jsou zveřejněné v různých formulářů, například SOAP, REST, atd.</span><span class="sxs-lookup"><span data-stu-id="edb93-110">Services that are exposed by content providers (Data Owners) are exposed in a variety of forms, such as REST, SOAP, etc.</span></span>

## <a name="what-is-a-csdl-and-its-structure"></a><span data-ttu-id="edb93-111">Co je CSDL a jeho struktura?</span><span class="sxs-lookup"><span data-stu-id="edb93-111">What is a CSDL and its structure?</span></span>
<span data-ttu-id="edb93-112">CSDL (koncepční Schema Definition Language) je specifikace definování jak toodescribe webové služby nebo databáze služby společné XML tento problém toohello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="edb93-112">A CSDL (Conceptual Schema Definition Language) is a specification defining how toodescribe web service or database service in common XML verbiage toohello Azure Marketplace.</span></span>

<span data-ttu-id="edb93-113">Jednoduchý přehled hello **požadavku toku:**</span><span class="sxs-lookup"><span data-stu-id="edb93-113">Simple overview of hello **request flow:**</span></span>

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

<span data-ttu-id="edb93-114">Hello **tok dat** v hello opačným směrem:</span><span class="sxs-lookup"><span data-stu-id="edb93-114">hello **data flow** is in hello opposite direction:</span></span>

  `Client <- Azure Marketplace <- Content Provider’s WebService`

<span data-ttu-id="edb93-115">**Obrázek 1** diagramy, jak klient získat data z obsahu zprostředkovatele (služba) prostřednictvím hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="edb93-115">**Figure 1** diagrams how a client would obtain data from a content provider (your service) by going through hello Azure Marketplace.</span></span>  <span data-ttu-id="edb93-116">používá Hello CSDL hello mapování/transformační součást toohandle hello požadavku a předejte data mezi hello obsahu poskytovatele služeb a hello požadavku klienta.</span><span class="sxs-lookup"><span data-stu-id="edb93-116">hello CSDL is used by hello Mapping/Transformation component toohandle hello request and data pass between hello content provider’s service(s) and hello requesting client.</span></span>

<span data-ttu-id="edb93-117">*Obrázek 1: Podrobné tok od žádajících poskytovatel toocontent klienta prostřednictvím Azure Marketplace*</span><span class="sxs-lookup"><span data-stu-id="edb93-117">*Figure 1: Detailed flow from requesting client toocontent provider via Azure Marketplace*</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

<span data-ttu-id="edb93-119">Pozadí na Atom, Atom Pub a protokolu OData hello, při které hello sestavení rozšíření Azure Marketplace, najdete v tématu: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)</span><span class="sxs-lookup"><span data-stu-id="edb93-119">For background on Atom, Atom Pub, and hello OData protocol upon which hello Azure Marketplace extensions build, please review: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)</span></span>

<span data-ttu-id="edb93-120">Výňatek ze výše odkaz: *"hello účel hello Open Data protocol (dále jen odkazované tooas OData) je protokol tooprovide bázi REST pro operace CRUD stylu (vytvoření, čtení, aktualizaci a odstraňování) s prostředky, které jsou zveřejněné jako datové služby. "Služba dat" je koncový bod tam, kde se nacházejí data z jedné nebo více "kolekcí" každý s nula nebo více "položky", které se skládají z zveřejněné zadali páry název hodnota. OData je publikovaná společností Microsoft v rámci standardy OASIS (organizace pro hello rozvoj strukturovaných standardy informace), aby každý uživatel, který chce toocan sestavení nástroje bez licenčních nebo omezení, klientů nebo serverů."*</span><span class="sxs-lookup"><span data-stu-id="edb93-120">Excerpt from above link: *“hello purpose of hello Open Data protocol (hereafter referred tooas OData) is tooprovide a REST-based protocol for CRUD-style operations (Create, Read, Update and Delete) against resources exposed as data services. A “data service” is an endpoint where there is data exposed from one or more “collections” each with zero or more “entries”, which consist of typed named-value pairs. OData is published by Microsoft under OASIS (Organization for hello Advancement of Structured Information Standards) Standards so that anyone that wants toocan build servers, clients or tools without royalties or restrictions.”*</span></span>

### <a name="three-critical-pieces-that-have-toobe-defined-by-hello-csdl-are"></a><span data-ttu-id="edb93-121">Jsou tři důležité údaje, které mají toobe definované hello CSDL:</span><span class="sxs-lookup"><span data-stu-id="edb93-121">Three Critical Pieces that have toobe defined by hello CSDL are:</span></span>
* <span data-ttu-id="edb93-122">Hello **koncový bod** z hello poskytovatele služeb hello webovou adresu (URI) hello služby</span><span class="sxs-lookup"><span data-stu-id="edb93-122">hello **endpoint** of hello Service Provider hello Web Address (URI) of hello Service</span></span>
* <span data-ttu-id="edb93-123">Hello **parametrů dat** předávány jako vstupní toohello poskytovatele služeb hello Definice parametrů hello odesílány toohello poskytovateli obsahu služby dolů toohello datového typu.</span><span class="sxs-lookup"><span data-stu-id="edb93-123">hello **data parameters** being passed as input toohello Service Provider hello definitions of hello parameters being sent toohello Content Provider’s service down toohello data type.</span></span>
* <span data-ttu-id="edb93-124">**Schéma** hello dat nevrátila toohello požaduje služba hello schéma dat hello doručován službou hello poskytovatele obsahu, včetně kontejneru, kolekce nebo tabulky, proměnné nebo sloupce a datové typy.</span><span class="sxs-lookup"><span data-stu-id="edb93-124">**Schema** of hello data being returned toohello Requesting Service hello schema of hello data being delivered by hello Content Provider’s service, including Container, collections/tables, variables/columns, and data types.</span></span>

<span data-ttu-id="edb93-125">Hello následující diagram ukazuje přehled hello toku z kde hello klienta do hello – příkaz (volání toohello poskytovatele obsahu webová služba) toogetting hello výsledků nebo dat OData zpět.</span><span class="sxs-lookup"><span data-stu-id="edb93-125">hello following diagram shows an overview of hello flow from where hello client enters hello OData statement (call toohello content provider’s web service) toogetting hello results/data back.</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a><span data-ttu-id="edb93-127">pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="edb93-127">Steps:</span></span>
1. <span data-ttu-id="edb93-128">Klient odešle požadavek prostřednictvím volání služby kompletní s vstupní parametry definované v XML toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="edb93-128">Client sends request via Service call complete with Input Parameters defined in XML toohello Azure Marketplace</span></span>
2. <span data-ttu-id="edb93-129">CSDL je použité toovalidate hello volání služby.</span><span class="sxs-lookup"><span data-stu-id="edb93-129">CSDL is used toovalidate hello Service call.</span></span>
   * <span data-ttu-id="edb93-130">Hello formátu volání služby pak odesílají toohello služba obsah zprostředkovatelé hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="edb93-130">hello Formatted Service Call is then sent toohello Content Providers Service by hello Azure Marketplace</span></span>
3. <span data-ttu-id="edb93-131">Hello webová služba spustí a preforms hello akce hello příkaz Http (tj. získat) hello data jsou vrácena tooAzure Marketplace, kde hello požadovaná data (pokud existuje) je zpřístupňuje ve formátu XML toohello klienta pomocí mapování definované v hello CSDL hello.</span><span class="sxs-lookup"><span data-stu-id="edb93-131">hello Webservice executes and preforms hello action of hello Http Verb (i.e. GET) hello data is returned tooAzure Marketplace where hello requested data (if any) is exposes in XML Format toohello Client using hello Mapping defined in hello CSDL.</span></span>
4. <span data-ttu-id="edb93-132">Hello klienta se odesílá hello data (pokud existuje) ve formátu XML nebo JSON</span><span class="sxs-lookup"><span data-stu-id="edb93-132">hello Client is sent hello data (if any) in XML or JSON format</span></span>

## <a name="definitions"></a><span data-ttu-id="edb93-133">Definice</span><span class="sxs-lookup"><span data-stu-id="edb93-133">Definitions</span></span>
### <a name="odata-atom-pub"></a><span data-ttu-id="edb93-134">Protokol pub OData ATOM</span><span class="sxs-lookup"><span data-stu-id="edb93-134">OData ATOM pub</span></span>
<span data-ttu-id="edb93-135">Nastavit rozšíření toohello ATOM pub kde každá položka představuje jeden řádek výsledků.</span><span class="sxs-lookup"><span data-stu-id="edb93-135">An extension toohello ATOM pub where each entry represents one row of a result set.</span></span> <span data-ttu-id="edb93-136">Hello obsahu součástí hello položka je rozšířené toocontain hello hodnoty řádku hello – jako párů klíčových hodnot.</span><span class="sxs-lookup"><span data-stu-id="edb93-136">hello content part of hello entry is enhanced toocontain hello values of hello row – as key value pairs.</span></span> <span data-ttu-id="edb93-137">Další informace naleznete zde: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)</span><span class="sxs-lookup"><span data-stu-id="edb93-137">More information is found here: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)</span></span>

### <a name="csdl---conceptual-schema-definition-language"></a><span data-ttu-id="edb93-138">CSDL - Conceptual Schema Definition Language</span><span class="sxs-lookup"><span data-stu-id="edb93-138">CSDL - Conceptual Schema Definition Language</span></span>
<span data-ttu-id="edb93-139">Umožňuje definovat funkce (SPROCs) a entity, které jsou k dispozici prostřednictvím databáze.</span><span class="sxs-lookup"><span data-stu-id="edb93-139">Allows defining functions (SPROCs) and entities that are exposed through a database.</span></span> <span data-ttu-id="edb93-140">Další informace, které se nachází tady: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)</span><span class="sxs-lookup"><span data-stu-id="edb93-140">More information found here: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)</span></span>  

> [!TIP]
> <span data-ttu-id="edb93-141">Klikněte na tlačítko hello **jiné verze** rozevírací seznam a vyberte verzi, pokud nevidíte hello článku.</span><span class="sxs-lookup"><span data-stu-id="edb93-141">Click hello **other versions** dropdown and select a version if you don’t see hello article.</span></span>
> 
> 

### <a name="edm---entry-data-model"></a><span data-ttu-id="edb93-142">EDM - vstupní datový Model</span><span class="sxs-lookup"><span data-stu-id="edb93-142">EDM - Entry Data Model</span></span>
* <span data-ttu-id="edb93-143">Přehled: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]</span><span class="sxs-lookup"><span data-stu-id="edb93-143">Overview: [http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx][OverviewLink]</span></span>

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* <span data-ttu-id="edb93-144">Ve verzi Preview: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]</span><span class="sxs-lookup"><span data-stu-id="edb93-144">Preview: [http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx][PreviewLink]</span></span>

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* <span data-ttu-id="edb93-145">Datové typy: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]</span><span class="sxs-lookup"><span data-stu-id="edb93-145">Data types: [http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx][DataTypesLink]</span></span>

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

<span data-ttu-id="edb93-146">Následující Hello zobrazuje tok levém tooRight ze kde hello klienta do hello – příkaz (volání toohello poskytovatele obsahu webová služba) toogetting hello výsledků nebo dat OData zpět podrobné hello:</span><span class="sxs-lookup"><span data-stu-id="edb93-146">hello following shows hello detailed Left tooRight flow from where hello client enters hello OData statement (call toohello content provider’s web service) toogetting hello results/data back:</span></span>

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a><span data-ttu-id="edb93-148">Základy CSDL</span><span class="sxs-lookup"><span data-stu-id="edb93-148">CSDL Basics</span></span>
<span data-ttu-id="edb93-149">CSDL (koncepční Schema Definition Language) je specifikace definování jak toodescribe webové služby nebo databáze služby společné XML tento problém toohello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="edb93-149">A CSDL (Conceptual Schema Definition Language) is a specification defining how toodescribe web service or database service in common XML verbiage toohello Azure Marketplace.</span></span> <span data-ttu-id="edb93-150">Popisuje CSDL hello kritické kusy, který **umožňuje hello předávání dat ze zdroje dat toohello hello Azure Marketplace.**</span><span class="sxs-lookup"><span data-stu-id="edb93-150">CSDL describes hello critical pieces that **makes hello passing of data from hello Data Source toohello Azure Marketplace possible.**</span></span> <span data-ttu-id="edb93-151">Hello hlavní části jsou popsány zde:</span><span class="sxs-lookup"><span data-stu-id="edb93-151">hello main pieces are described here:</span></span>

* <span data-ttu-id="edb93-152">Rozhraní informace, které popisují všechny veřejně dostupné funkce (FunctionImport uzel)</span><span class="sxs-lookup"><span data-stu-id="edb93-152">Interface information describing all publicly available functions (FunctionImport Node)</span></span>
* <span data-ttu-id="edb93-153">Datový typ informace pro všechny zprávy requests(input) a zpráva responses(outputs) (EntityContainer nebo objekt EntitySet/EntityType uzlů)</span><span class="sxs-lookup"><span data-stu-id="edb93-153">Data type information for all message requests(input) and message responses(outputs) (EntityContainer/EntitySet/EntityType Nodes)</span></span>
* <span data-ttu-id="edb93-154">Informace o hello přenosu protokolu toobe vazby použít (záhlaví uzlu)</span><span class="sxs-lookup"><span data-stu-id="edb93-154">Binding information about hello transport protocol toobe used (Header Node)</span></span>
* <span data-ttu-id="edb93-155">Informace o adrese pro vyhledání hello zadané služby (BaseURI atribut)</span><span class="sxs-lookup"><span data-stu-id="edb93-155">Address information for locating hello specified service (BaseURI attribute)</span></span>

<span data-ttu-id="edb93-156">Stručně řečeno hello CSDL představuje kontraktu nezávislé na platformě a jazyku mezi hello služby žadatel a hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="edb93-156">In a nutshell, hello CSDL represents a platform- and language-independent contract between hello service requestor and hello service provider.</span></span> <span data-ttu-id="edb93-157">Pomocí hello CSDL, můžete klienta najít webové služby a databáze služby a vyvolání svých veřejně dostupné úkolů.</span><span class="sxs-lookup"><span data-stu-id="edb93-157">Using hello CSDL, a client can locate a web service/database service and invoke any of its publicly available functions.</span></span>

### <a name="relating-a-csdl-tooa-database-or-a-collection"></a><span data-ttu-id="edb93-158">Související CSDL tooa databáze nebo kolekce</span><span class="sxs-lookup"><span data-stu-id="edb93-158">Relating a CSDL tooa Database or a Collection</span></span>
<span data-ttu-id="edb93-159">**Hello CSDL specifikace**</span><span class="sxs-lookup"><span data-stu-id="edb93-159">**hello CSDL Specification**</span></span>

<span data-ttu-id="edb93-160">CSDL se gramatika XML pro popis webové služby.</span><span class="sxs-lookup"><span data-stu-id="edb93-160">CSDL is XML grammar for describing a web service.</span></span> <span data-ttu-id="edb93-161">Specifikace Hello, samotné je rozdělené do 4 důležité elementy: EntitySet, FunctionImport; Obor názvů a typ EntityType.</span><span class="sxs-lookup"><span data-stu-id="edb93-161">hello specification itself is divided into 4 major elements:  EntitySet, FunctionImport; NameSpace, and EntityType.</span></span>

<span data-ttu-id="edb93-162">toomake umožňuje snazší toounderstand tato abstrakce tabulku tooa CSDL se týkají.</span><span class="sxs-lookup"><span data-stu-id="edb93-162">toomake this abstraction easier toounderstand lets relate a CSDL tooa table.</span></span>

<span data-ttu-id="edb93-163">Mějte na paměti,</span><span class="sxs-lookup"><span data-stu-id="edb93-163">Remember;</span></span>

  <span data-ttu-id="edb93-164">CSDL představuje kontraktu nezávislé na platformě a jazyku mezi hello **služby žadatele** a hello **poskytovatele služeb**.</span><span class="sxs-lookup"><span data-stu-id="edb93-164">CSDL represents a platform- and language-independent contract between hello **service requestor** and hello **service provider**.</span></span> <span data-ttu-id="edb93-165">Pomocí CSDL, **klienta** mohou vyhledat **webové služby a databáze služby** a spustit žádné jeho veřejně dostupné **funkce.**</span><span class="sxs-lookup"><span data-stu-id="edb93-165">Using CSDL, a **client** can locate a **web service/database service** and invoke any of its publicly available **functions.**</span></span>

<span data-ttu-id="edb93-166">Pro Data služby hello čtyři části CSDL si lze představit z hlediska databáze, tabulky, sloupce a proceduře úložiště.</span><span class="sxs-lookup"><span data-stu-id="edb93-166">For a Data Service hello four parts of a CSDL can be thought of in terms of a Database, Table, Column, and Store Procedure.</span></span>

<span data-ttu-id="edb93-167">Týkající se těchto takto pro datové služby:</span><span class="sxs-lookup"><span data-stu-id="edb93-167">Relating these as follows for a Data Service:</span></span>

* <span data-ttu-id="edb93-168">EntityContainer ~ = databáze</span><span class="sxs-lookup"><span data-stu-id="edb93-168">EntityContainer  ~=  Database</span></span>
* <span data-ttu-id="edb93-169">Objekt EntitySet ~ = tabulky</span><span class="sxs-lookup"><span data-stu-id="edb93-169">EntitySet  ~=  Table</span></span>
* <span data-ttu-id="edb93-170">EntityType ~ = sloupců</span><span class="sxs-lookup"><span data-stu-id="edb93-170">EntityType  ~= Columns</span></span>
* <span data-ttu-id="edb93-171">Element FunctionImport ~ = uložené procedury</span><span class="sxs-lookup"><span data-stu-id="edb93-171">FunctionImport  ~=  Stored Procedure</span></span>

<span data-ttu-id="edb93-172">**Akce HTTP povolena**</span><span class="sxs-lookup"><span data-stu-id="edb93-172">**HTTP Verbs allowed**</span></span>

* <span data-ttu-id="edb93-173">GET – vrátí hodnoty z databáze hello (vrátí kolekci)</span><span class="sxs-lookup"><span data-stu-id="edb93-173">GET – returns values from hello db (returns a Collection)</span></span>
* <span data-ttu-id="edb93-174">POST – použít toopass tooand volitelné návratové hodnoty dat z databáze hello (vytvořit novou položku v kolekci hello, návratový id nebo identifikátor URI)</span><span class="sxs-lookup"><span data-stu-id="edb93-174">POST – used toopass data tooand optional return values from hello db (Create a new entry in hello collection, return id/URI)</span></span>
* <span data-ttu-id="edb93-175">ODSTRANIT – odstranění dat z hello DB (odstraní kolekce)</span><span class="sxs-lookup"><span data-stu-id="edb93-175">DELETE – Deletes data from hello DB (Deletes a collection)</span></span>
* <span data-ttu-id="edb93-176">PUT – aktualizace dat do DB (nahraďte kolekci nebo ji vytvořte)</span><span class="sxs-lookup"><span data-stu-id="edb93-176">PUT – Update data into a DB (replace a collection or create one)</span></span>

## <a name="metadatamapping-document"></a><span data-ttu-id="edb93-177">Dokument metadat/mapování</span><span class="sxs-lookup"><span data-stu-id="edb93-177">Metadata/Mapping Document</span></span>
<span data-ttu-id="edb93-178">Dokument metadat/mapování Hello je použité toomap, které poskytovateli obsahu existující webové služby, aby mohou být zpřístupněny jako webovou službu OData systémem hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="edb93-178">hello metadata/mapping document is used toomap a content provider’s existing web services so that it can be exposed as an OData web service by hello Azure Marketplace system.</span></span> <span data-ttu-id="edb93-179">Je založena na CSDL a implementuje několik rozšíření tooCSDL tooaccommodate hello potřebám REST na základě webové služby, které jsou k dispozici prostřednictvím Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="edb93-179">It is based on CSDL and implements a few extensions tooCSDL tooaccommodate hello needs of REST based web services exposed through Azure Marketplace.</span></span> <span data-ttu-id="edb93-180">rozšíření Hello se nacházejí v hello [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="edb93-180">hello extensions are found in hello [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) namespace.</span></span>

<span data-ttu-id="edb93-181">Příklad hello CSDL: (kopírování a vkládání hello následujícím příkladu CSDL do editoru XML a změňte toomatch služby.</span><span class="sxs-lookup"><span data-stu-id="edb93-181">An example of hello CSDL follows:  (Copy and paste hello below example CSDL into an XML editor and change toomatch your Service.</span></span>  <span data-ttu-id="edb93-182">Vložte do mapování CSDL kartě DataService při vytváření služby v hello [publikování portálu Azure Marketplace](https://publish.windowsazure.com)).</span><span class="sxs-lookup"><span data-stu-id="edb93-182">Then paste into CSDL Mapping under DataService tab when creating your service in hello  [Azure Marketplace Publishing Portal](https://publish.windowsazure.com)).</span></span>

<span data-ttu-id="edb93-183">**Podmínky:** pokud jde o hello CSDL podmínky toohello [publikování portál](https://publish.windowsazure.com) podmínky uživatelského rozhraní (PPUI).</span><span class="sxs-lookup"><span data-stu-id="edb93-183">**Terms:** Relating hello CSDL terms toohello [Publishing Portal](https://publish.windowsazure.com) UI (PPUI) terms.</span></span>

* <span data-ttu-id="edb93-184">Nabízí "Title" v hello PPUI má vztah tooMyWebOffer</span><span class="sxs-lookup"><span data-stu-id="edb93-184">Offer “Title” in hello PPUI relates tooMyWebOffer</span></span>
* <span data-ttu-id="edb93-185">Moje_firma v hello PPUI má vztah příliš**zobrazovaný název vydavatele** v hello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="edb93-185">MyCompany in hello PPUI relates too**Publisher Display Name** in hello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI</span></span>
* <span data-ttu-id="edb93-186">Rozhraní API má vztah tooa Web nebo dat služby (plán v hello PPUI)</span><span class="sxs-lookup"><span data-stu-id="edb93-186">Your API relates tooa Web or Data Service (a Plan in hello PPUI)</span></span>

<span data-ttu-id="edb93-187">**Hierarchie:** společnost (poskytovateli obsahu) vlastní tyto nabídky, které mají/í, konkrétně service(s), který řádek nahoru rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="edb93-187">**Hierarchy:** A Company (Content Provider) owns Offer(s) which have Plan(s), namely service(s), which line up with an API.</span></span>

### <a name="webservice-csdl-example"></a><span data-ttu-id="edb93-188">Příklad CSDL webovou službu</span><span class="sxs-lookup"><span data-stu-id="edb93-188">WebService CSDL Example</span></span>
<span data-ttu-id="edb93-189">Připojí tooa služba, která je vystavení koncový bod webové aplikace (např. aplikaci C#)</span><span class="sxs-lookup"><span data-stu-id="edb93-189">Connects tooa service that is exposing an web application endpoint (like a C# application)</span></span>

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
> <span data-ttu-id="edb93-190">Zobrazit další příklady CSDL webové služby v článku hello [příklady mapování existující webové služby tooOData prostřednictvím CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)</span><span class="sxs-lookup"><span data-stu-id="edb93-190">View more CSDL Web Service examples in hello article [Examples of mapping an existing web service tooOData through CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)</span></span>
> 
> 

### <a name="dataservice-csdl-example"></a><span data-ttu-id="edb93-191">Příklad DataService CSDL</span><span class="sxs-lookup"><span data-stu-id="edb93-191">DataService CSDL Example</span></span>
<span data-ttu-id="edb93-192">Připojí tooa služba, která je vystavení databázové tabulky nebo zobrazení, protože koncový bod následující příklad ukazuje dva rozhraní API pro Data základní založené na rozhraní API CSDL (můžete použít zobrazení, nikoli tabulky).</span><span class="sxs-lookup"><span data-stu-id="edb93-192">Connects tooa service that is exposing a database table or view as an endpoint Below example shows two APIs for Data base based API CSDL (can use views rather than tables).</span></span>

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

## <a name="see-also"></a><span data-ttu-id="edb93-193">Viz také</span><span class="sxs-lookup"><span data-stu-id="edb93-193">See Also</span></span>
* <span data-ttu-id="edb93-194">Pokud vás zajímá učení a pochopení hello konkrétní uzlů a jejich parametrů, přečtěte si tento článek [datové služby OData mapování uzly](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) pro definice a vysvětlení, příklady a kontext případů použití.</span><span class="sxs-lookup"><span data-stu-id="edb93-194">If you are interested in learning and understanding hello specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="edb93-195">Pokud vás zajímá kontrola příklady, přečtěte si tento článek [Data služby OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee ukázkový kód a pochopit syntaxe kódu a kontext.</span><span class="sxs-lookup"><span data-stu-id="edb93-195">If you are interested in reviewing examples, read this article [Data Service OData Mapping Examples](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee sample code and understand code syntax and context.</span></span>
* <span data-ttu-id="edb93-196">tooreturn toohello předepsané cestu pro publikování dat služby toohello Azure Marketplace, přečtěte si tento článek [Průvodce publikování dat služby](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="edb93-196">tooreturn toohello prescribed path for publishing a Data Service toohello Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

