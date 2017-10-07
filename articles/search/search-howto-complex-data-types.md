---
title: "aaaHow toomodel komplexní datové typy ve službě Azure Search | Microsoft Docs"
description: "Vnořené nebo hierarchické datové struktury můžete modelován v indexu Azure Search pomocí plochou řádků a typu dat kolekce."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: complex data types; compound data types; aggregate data types
ms.assetid: e4bf86b4-497a-4179-b09f-c1b56c3c0bb2
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: b330c5b322f4f33123a454be11733b977684b9e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomodel-complex-data-types-in-azure-search"></a><span data-ttu-id="0fd75-103">Jak toomodel rozšířené datové typy ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="0fd75-103">How toomodel complex data types in Azure Search</span></span>
<span data-ttu-id="0fd75-104">Externích datových sad použít toopopulate indexu Azure Search někdy obsahovat hierarchické nebo vnořená používání dílčích struktur, které nejsou v tabulkovém řádků přehledně rozdělení.</span><span class="sxs-lookup"><span data-stu-id="0fd75-104">External datasets used toopopulate an Azure Search index sometimes include hierarchical or nested substructures that do not break down neatly into a tabular rowset.</span></span> <span data-ttu-id="0fd75-105">Příkladem takových struktury může zahrnovat více umístění a telefonních čísel pro jednoho zákazníka, více barvy a velikosti pro jednu SKU, více autorů jeden knihy a tak dále.</span><span class="sxs-lookup"><span data-stu-id="0fd75-105">Examples of such structures might include multiple locations and phone numbers for a single customer, multiple colors and sizes for a single SKU, multiple authors of a single book, and so on.</span></span> <span data-ttu-id="0fd75-106">V modelování podmínky, můžete vidět těchto struktur označuje tooas *komplexními datovými typy*, *složené datové typy*, *složené datové typy*, nebo *agregační datové typy*, tooname a několika.</span><span class="sxs-lookup"><span data-stu-id="0fd75-106">In modeling terms, you might see these structures referred tooas *complex data types*, *compound data types*, *composite data types*, or *aggregate data types*, tooname a few.</span></span>

<span data-ttu-id="0fd75-107">Komplexní datové typy nejsou podporovány nativně ve službě Azure Search, ale Principy řešení zahrnuje dvoustupňový proces sloučení hello struktura a pak pomocí **kolekce** datový typ vnitřních struktura tooreconstitute hello.</span><span class="sxs-lookup"><span data-stu-id="0fd75-107">Complex data types are not natively supported in Azure Search, but a proven workaround includes a two-step process of flattening hello structure and then using a **Collection** data type tooreconstitute hello interior structure.</span></span> <span data-ttu-id="0fd75-108">Následující hello technik popsaných v tomto článku umožňuje obsahu toobe hello vyhledávaná, Fasetové, filtrovat a seřazeny.</span><span class="sxs-lookup"><span data-stu-id="0fd75-108">Following hello technique described in this article allows hello content toobe searched, faceted, filtered, and sorted.</span></span>

## <a name="example-of-a-complex-data-structure"></a><span data-ttu-id="0fd75-109">Příklad rozšířené datové struktury</span><span class="sxs-lookup"><span data-stu-id="0fd75-109">Example of a complex data structure</span></span>
<span data-ttu-id="0fd75-110">Data hello dotyčném se obvykle nachází jako sada dokumentů XML nebo JSON, nebo jako položky v úložišti NoSQL například Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0fd75-110">Typically, hello data in question resides as a set of JSON or XML documents, or as items in a NoSQL store such as Azure Cosmos DB.</span></span> <span data-ttu-id="0fd75-111">Strukturálně hello výzvy byl vyvolán s více podřízené položky, které je třeba toobe vyhledávat a filtrovat.</span><span class="sxs-lookup"><span data-stu-id="0fd75-111">Structurally, hello challenge stems from having multiple child items that need toobe searched and filtered.</span></span>  <span data-ttu-id="0fd75-112">Jako výchozí bod pro znázornění hello alternativní řešení proveďte následující dokument JSON, který obsahuje sadu kontakty jako příklad hello:</span><span class="sxs-lookup"><span data-stu-id="0fd75-112">As a starting point for illustrating hello workaround, take hello following JSON document that lists a set of contacts as an example:</span></span>

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

<span data-ttu-id="0fd75-113">Při hello pole s názvem "id", "název" a "společnost" lze snadno mapovat 1: 1 jako pole v indexu Azure Search, hello 'umístění' pole obsahuje pole umístění, i skupinu ID umístění, jakož i popisy umístění.</span><span class="sxs-lookup"><span data-stu-id="0fd75-113">While hello fields named ‘id’, ‘name’ and ‘company’ can easily be mapped one-to-one as fields within an Azure Search index, hello ‘locations’ field contains an array of locations, having both a set of location IDs as well as location descriptions.</span></span> <span data-ttu-id="0fd75-114">Vzhledem k tomu, že Azure Search nemá datový typ, který to podporuje, budeme potřebovat toomodel jiný způsob, jak ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="0fd75-114">Given that Azure Search does not have a data type that supports this, we need a different way toomodel this in Azure Search.</span></span> 

> [!NOTE]
> <span data-ttu-id="0fd75-115">Tento postup je popsán také zařízení Kirk Evans v příspěvku blogu [indexování DocumentDB s Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), který ukazuje techniku zvanou "sloučení hello data", které byste měli pole s názvem `locationsID` a `locationsDescription` které jsou obě [kolekce](https://msdn.microsoft.com/library/azure/dn798938.aspx) (nebo pole řetězců).</span><span class="sxs-lookup"><span data-stu-id="0fd75-115">This technique is also described by Kirk Evans in a blog post [Indexing DocumentDB with Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), which shows a technique called "flattening hello data", whereby you would have a field called `locationsID` and `locationsDescription` that are both [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span>   
> 
> 

## <a name="part-1-flatten-hello-array-into-individual-fields"></a><span data-ttu-id="0fd75-116">Část 1: Narovnání hello pole na jednotlivých polí</span><span class="sxs-lookup"><span data-stu-id="0fd75-116">Part 1: Flatten hello array into individual fields</span></span>
<span data-ttu-id="0fd75-117">toocreate indexu Azure Search, která bude vyhovovat tuto datovou sadu vytvořit jednotlivých polí pro vnořené podkladní hello: `locationsID` a `locationsDescription` s typem dat [kolekce](https://msdn.microsoft.com/library/azure/dn798938.aspx) (nebo pole řetězců).</span><span class="sxs-lookup"><span data-stu-id="0fd75-117">toocreate an Azure Search index that accommodates this dataset, create individual fields for hello nested substructure: `locationsID` and `locationsDescription` with a data type of [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span> <span data-ttu-id="0fd75-118">V těchto polích by indexu hello hodnoty '1' a "2" do hello `locationsID` pole pro hodnoty John Smith a hello "3" a "4" do hello `locationsID` pole pro Jen Campbell.</span><span class="sxs-lookup"><span data-stu-id="0fd75-118">In these fields you would index hello values ‘1’ and ‘2’ into hello `locationsID` field for John Smith and hello values ‘3’ & ‘4’ into hello `locationsID` field for Jen Campbell.</span></span>  

<span data-ttu-id="0fd75-119">Vaše data v rámci Azure Search bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="0fd75-119">Your data within Azure Search would look like this:</span></span> 

![Ukázková data, 2 řádků](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-hello-index-definition"></a><span data-ttu-id="0fd75-121">Část 2: Přidání kolekci polí v definici indexu hello</span><span class="sxs-lookup"><span data-stu-id="0fd75-121">Part 2: Add a collection field in hello index definition</span></span>
<span data-ttu-id="0fd75-122">V hello schéma indexu definice pole hello může vypadat podobně jako příklad toothis.</span><span class="sxs-lookup"><span data-stu-id="0fd75-122">In hello index schema, hello field definitions might look similar toothis example.</span></span>

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-hello-index"></a><span data-ttu-id="0fd75-123">Ověření chování vyhledávání a případně rozšířit hello indexu</span><span class="sxs-lookup"><span data-stu-id="0fd75-123">Validate search behaviors and optionally extend hello index</span></span>
<span data-ttu-id="0fd75-124">Za předpokladu, že vytvořený hello index a načíst hello data, můžete prověřit provádění dotazu hello řešení tooverify vyhledávání proti hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="0fd75-124">Assuming you created hello index and loaded hello data, you can now test hello solution tooverify search query execution against hello dataset.</span></span> <span data-ttu-id="0fd75-125">Každý **kolekce** pole by mělo být **prohledávatelné**, **filtrovatelných** a **facetable**.</span><span class="sxs-lookup"><span data-stu-id="0fd75-125">Each **collection** field should be **searchable**, **filterable** and **facetable**.</span></span> <span data-ttu-id="0fd75-126">Měli byste mít možnost toorun dotazy jako:</span><span class="sxs-lookup"><span data-stu-id="0fd75-126">You should be able toorun queries like:</span></span>

* <span data-ttu-id="0fd75-127">Najít všichni uživatelé, kteří pracují v hello 'Adventureworks ústředí'.</span><span class="sxs-lookup"><span data-stu-id="0fd75-127">Find all people who work at hello ‘Adventureworks Headquarters’.</span></span>
* <span data-ttu-id="0fd75-128">Získáte počet hello počet lidí, kteří pracují v Home Office.</span><span class="sxs-lookup"><span data-stu-id="0fd75-128">Get a count of hello number of people who work in a ‘Home Office’.</span></span>  
* <span data-ttu-id="0fd75-129">Hello osobám, které fungují na Home Office zobrazit jaké ostatních kancelářích fungují společně s počtem hello lidem v každém umístění.</span><span class="sxs-lookup"><span data-stu-id="0fd75-129">Of hello people who work at a ‘Home Office’, show what other offices they work along with a count of hello people in each location.</span></span>  

<span data-ttu-id="0fd75-130">Kde tato technika spadá rozdělovat je, když potřebujete toodo hledání, které kombinuje id umístění hello jak hello umístění popis.</span><span class="sxs-lookup"><span data-stu-id="0fd75-130">Where this technique falls apart is when you need toodo a search that combines both hello location id as well as hello location description.</span></span> <span data-ttu-id="0fd75-131">Například:</span><span class="sxs-lookup"><span data-stu-id="0fd75-131">For example:</span></span>

* <span data-ttu-id="0fd75-132">Najít všechny osoby, které mají Home Office a mají ID umístění na 4.</span><span class="sxs-lookup"><span data-stu-id="0fd75-132">Find all people where they have a Home Office AND has a location ID of 4.</span></span>  

<span data-ttu-id="0fd75-133">Pokud si Vzpomínáte hello původní obsah hledá takto:</span><span class="sxs-lookup"><span data-stu-id="0fd75-133">If you recall hello original content looked like this:</span></span>

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

<span data-ttu-id="0fd75-134">Ale teď, když budeme mít oddělené hello data do samostatných polí, jsme nijak zjistit, pokud má příliš vztah hello Home Office pro Jen Campbell`locationsID 3` nebo `locationsID 4`.</span><span class="sxs-lookup"><span data-stu-id="0fd75-134">However, now that we have separated hello data into separate fields, we have no way of knowing if hello Home Office for Jen Campbell relates too`locationsID 3` or `locationsID 4`.</span></span>  

<span data-ttu-id="0fd75-135">toohandle v tomto případě definovat jiné pole v indexu hello, které se kombinuje všechna hello data do jedné kolekce.</span><span class="sxs-lookup"><span data-stu-id="0fd75-135">toohandle this case, define another field in hello index that combines all of hello data into a single collection.</span></span>  <span data-ttu-id="0fd75-136">Pro náš příklad jsme bude volat v tomto poli `locationsCombined` a jsme dojde k oddělení hello obsah s `||` však můžete zvolit jakékoli oddělovač, který se domníváte, že by jedinečnou sadu znaků pro obsah.</span><span class="sxs-lookup"><span data-stu-id="0fd75-136">For our example, we will call this field `locationsCombined` and we will separate hello content with a `||` although you can choose any separator that you think would be a unique set of characters for your content.</span></span> <span data-ttu-id="0fd75-137">Například:</span><span class="sxs-lookup"><span data-stu-id="0fd75-137">For example:</span></span> 

![Ukázková data, 2 řádky s oddělovačem](./media/search-howto-complex-data-types/sample-data-2.png)

<span data-ttu-id="0fd75-139">Pomocí této `locationsCombined` pole, jsme teď zvládne i další dotazy, například:</span><span class="sxs-lookup"><span data-stu-id="0fd75-139">Using this `locationsCombined` field, we can now accommodate even more queries, such as:</span></span>

* <span data-ttu-id="0fd75-140">Zobrazí počet uživatelů, kteří pracovat na 'domovské úřadu, s umístěním Id "4".</span><span class="sxs-lookup"><span data-stu-id="0fd75-140">Show a count of people who work at a ‘Home Office’ with location Id of ‘4’.</span></span>  
* <span data-ttu-id="0fd75-141">Vyhledejte lidem, kteří pracují v Home Office s umístěním Id "4".</span><span class="sxs-lookup"><span data-stu-id="0fd75-141">Search for people who work at a ‘Home Office’ with location Id ‘4’.</span></span> 

## <a name="limitations"></a><span data-ttu-id="0fd75-142">Omezení</span><span class="sxs-lookup"><span data-stu-id="0fd75-142">Limitations</span></span>
<span data-ttu-id="0fd75-143">Tato metoda je užitečná pro různé scénáře, ale není možné v každém případě ho.</span><span class="sxs-lookup"><span data-stu-id="0fd75-143">This technique is useful for a number of scenarios, but it is not applicable in every case.</span></span>  <span data-ttu-id="0fd75-144">Například:</span><span class="sxs-lookup"><span data-stu-id="0fd75-144">For example:</span></span>

1. <span data-ttu-id="0fd75-145">Pokud nemáte sadu statických polí v komplexního datového typu a se žádný způsob, jak toomap všechny možné hello typy tooa jediné pole.</span><span class="sxs-lookup"><span data-stu-id="0fd75-145">If you do not have a static set of fields in your complex data type and there was no way toomap all hello possible types tooa single field.</span></span> 
2. <span data-ttu-id="0fd75-146">Aktualizace hello vnořených objektů vyžaduje některé další práci toodetermine přesně toho, co je toobe aktualizovat v indexu Azure Search hello</span><span class="sxs-lookup"><span data-stu-id="0fd75-146">Updating hello nested objects requires some extra work toodetermine exactly what needs toobe updated in hello Azure Search index</span></span>

## <a name="sample-code"></a><span data-ttu-id="0fd75-147">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="0fd75-147">Sample code</span></span>
<span data-ttu-id="0fd75-148">Můžete zobrazit příklad, jak tooindex komplexní data JSON nastavit Azure Search a provádět spoustu dotazy přes tuto datovou sadu v této [úložiště GitHub](https://github.com/liamca/AzureSearchComplexTypes).</span><span class="sxs-lookup"><span data-stu-id="0fd75-148">You can see an example on how tooindex a complex JSON data set into Azure Search and perform a number of queries over this dataset at this [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes).</span></span>

## <a name="next-step"></a><span data-ttu-id="0fd75-149">Další krok</span><span class="sxs-lookup"><span data-stu-id="0fd75-149">Next step</span></span>
<span data-ttu-id="0fd75-150">[Hlas pro nativní podpora pro komplexní datové typy](https://feedback.azure.com/forums/263029-azure-search) na hello Azure Search UserVoice stránku a všechny další zadání, který byste nám chtěli tooconsider týkající se funkce implementace.</span><span class="sxs-lookup"><span data-stu-id="0fd75-150">[Vote for native support for complex data types](https://feedback.azure.com/forums/263029-azure-search) on hello Azure Search UserVoice page and provide any additional input that you’d like us tooconsider regarding feature implementation.</span></span> <span data-ttu-id="0fd75-151">Můžete také oslovení toome přímo na Twitteru v @liamca.</span><span class="sxs-lookup"><span data-stu-id="0fd75-151">You can also reach out toome directly on Twitter at @liamca.</span></span>

