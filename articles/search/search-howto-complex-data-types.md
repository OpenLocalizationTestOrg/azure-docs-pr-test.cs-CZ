---
title: "Postup modelu komplexní datové typy ve službě Azure Search | Microsoft Docs"
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
ms.openlocfilehash: d576fd7bb267ae7a100589413185b595e3b2be42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a><span data-ttu-id="8b330-103">Postup modelu komplexní datové typy ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="8b330-103">How to model complex data types in Azure Search</span></span>
<span data-ttu-id="8b330-104">Externích datových sad, které používá k naplnění indexu Azure Search někdy obsahovat hierarchické nebo vnořená používání dílčích struktur, které nejsou v tabulkovém řádků přehledně rozdělení.</span><span class="sxs-lookup"><span data-stu-id="8b330-104">External datasets used to populate an Azure Search index sometimes include hierarchical or nested substructures that do not break down neatly into a tabular rowset.</span></span> <span data-ttu-id="8b330-105">Příkladem takových struktury může zahrnovat více umístění a telefonních čísel pro jednoho zákazníka, více barvy a velikosti pro jednu SKU, více autorů jeden knihy a tak dále.</span><span class="sxs-lookup"><span data-stu-id="8b330-105">Examples of such structures might include multiple locations and phone numbers for a single customer, multiple colors and sizes for a single SKU, multiple authors of a single book, and so on.</span></span> <span data-ttu-id="8b330-106">V modelování podmínky, můžete se setkat těchto struktur, označuje jako *komplexními datovými typy*, *složené datové typy*, *složené datové typy*, nebo *agregace datové typy*, a další.</span><span class="sxs-lookup"><span data-stu-id="8b330-106">In modeling terms, you might see these structures referred to as *complex data types*, *compound data types*, *composite data types*, or *aggregate data types*, to name a few.</span></span>

<span data-ttu-id="8b330-107">Komplexní datové typy nejsou podporovány nativně ve službě Azure Search, ale Principy řešení zahrnuje dvoustupňový proces sloučení strukturu a potom pomocí **kolekce** datový typ pro-rekonstruovat pro vnitřní strukturu.</span><span class="sxs-lookup"><span data-stu-id="8b330-107">Complex data types are not natively supported in Azure Search, but a proven workaround includes a two-step process of flattening the structure and then using a **Collection** data type to reconstitute the interior structure.</span></span> <span data-ttu-id="8b330-108">Podle postupu popsaného v tomto článku umožňuje obsah má proběhnout, Fasetové, filtrovat a seřadit.</span><span class="sxs-lookup"><span data-stu-id="8b330-108">Following the technique described in this article allows the content to be searched, faceted, filtered, and sorted.</span></span>

## <a name="example-of-a-complex-data-structure"></a><span data-ttu-id="8b330-109">Příklad rozšířené datové struktury</span><span class="sxs-lookup"><span data-stu-id="8b330-109">Example of a complex data structure</span></span>
<span data-ttu-id="8b330-110">Dotyčné údaje se obvykle nachází jako sada dokumentů XML nebo JSON, nebo jako položky v úložišti NoSQL například Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8b330-110">Typically, the data in question resides as a set of JSON or XML documents, or as items in a NoSQL store such as Azure Cosmos DB.</span></span> <span data-ttu-id="8b330-111">Strukturálně výzvy byl vyvolán s více podřízené položky, které je třeba vyhledávat a filtrovat.</span><span class="sxs-lookup"><span data-stu-id="8b330-111">Structurally, the challenge stems from having multiple child items that need to be searched and filtered.</span></span>  <span data-ttu-id="8b330-112">Jako výchozí bod pro znázornění alternativní řešení proveďte následující dokument JSON, který obsahuje sadu kontakty jako příklad:</span><span class="sxs-lookup"><span data-stu-id="8b330-112">As a starting point for illustrating the workaround, take the following JSON document that lists a set of contacts as an example:</span></span>

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

<span data-ttu-id="8b330-113">Při pole s názvem "id", "název" a "společnost" lze snadno mapovat 1: 1 jako pole v indexu Azure Search, pole, umístění, obsahuje pole umístění, i skupinu ID umístění, jakož i popisy umístění.</span><span class="sxs-lookup"><span data-stu-id="8b330-113">While the fields named ‘id’, ‘name’ and ‘company’ can easily be mapped one-to-one as fields within an Azure Search index, the ‘locations’ field contains an array of locations, having both a set of location IDs as well as location descriptions.</span></span> <span data-ttu-id="8b330-114">Vzhledem k tomu, že Azure Search nemá datový typ, který to podporuje, potřebujeme jiný způsob, jak to modelu ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="8b330-114">Given that Azure Search does not have a data type that supports this, we need a different way to model this in Azure Search.</span></span> 

> [!NOTE]
> <span data-ttu-id="8b330-115">Tento postup je popsán také zařízení Kirk Evans v příspěvku blogu [indexování DocumentDB s Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), zobrazující techniku nazývaný "sloučení dat", kterým byste měli pole s názvem `locationsID` a `locationsDescription` jsou oba [kolekce](https://msdn.microsoft.com/library/azure/dn798938.aspx) (nebo pole řetězců).</span><span class="sxs-lookup"><span data-stu-id="8b330-115">This technique is also described by Kirk Evans in a blog post [Indexing DocumentDB with Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), which shows a technique called "flattening the data", whereby you would have a field called `locationsID` and `locationsDescription` that are both [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span>   
> 
> 

## <a name="part-1-flatten-the-array-into-individual-fields"></a><span data-ttu-id="8b330-116">Část 1: Narovnání pole na jednotlivých polí</span><span class="sxs-lookup"><span data-stu-id="8b330-116">Part 1: Flatten the array into individual fields</span></span>
<span data-ttu-id="8b330-117">K vytvoření indexu Azure Search, která bude vyhovovat tuto datovou sadu, vytvoření jednotlivých polí pro vnořené podkladní: `locationsID` a `locationsDescription` s typem dat [kolekce](https://msdn.microsoft.com/library/azure/dn798938.aspx) (nebo pole řetězců).</span><span class="sxs-lookup"><span data-stu-id="8b330-117">To create an Azure Search index that accommodates this dataset, create individual fields for the nested substructure: `locationsID` and `locationsDescription` with a data type of [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span> <span data-ttu-id="8b330-118">V těchto polích by do indexu hodnoty '1' a (2) `locationsID` do pole pro John Smith a hodnoty "3" a "4" `locationsID` pole pro Jen Campbell.</span><span class="sxs-lookup"><span data-stu-id="8b330-118">In these fields you would index the values ‘1’ and ‘2’ into the `locationsID` field for John Smith and the values ‘3’ & ‘4’ into the `locationsID` field for Jen Campbell.</span></span>  

<span data-ttu-id="8b330-119">Vaše data v rámci Azure Search bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="8b330-119">Your data within Azure Search would look like this:</span></span> 

![Ukázková data, 2 řádků](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-the-index-definition"></a><span data-ttu-id="8b330-121">Část 2: Přidání kolekci polí v definici indexu</span><span class="sxs-lookup"><span data-stu-id="8b330-121">Part 2: Add a collection field in the index definition</span></span>
<span data-ttu-id="8b330-122">Ve schématu indexu může vypadat podobně jako tento příklad definice pole.</span><span class="sxs-lookup"><span data-stu-id="8b330-122">In the index schema, the field definitions might look similar to this example.</span></span>

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

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a><span data-ttu-id="8b330-123">Ověření chování vyhledávání a případně rozšířit index</span><span class="sxs-lookup"><span data-stu-id="8b330-123">Validate search behaviors and optionally extend the index</span></span>
<span data-ttu-id="8b330-124">Za předpokladu, že jste vytvořili index a načíst data, můžete prověřit řešení ověření při provádění dotazu hledání podle datové sady.</span><span class="sxs-lookup"><span data-stu-id="8b330-124">Assuming you created the index and loaded the data, you can now test the solution to verify search query execution against the dataset.</span></span> <span data-ttu-id="8b330-125">Každý **kolekce** pole by mělo být **prohledávatelné**, **filtrovatelných** a **facetable**.</span><span class="sxs-lookup"><span data-stu-id="8b330-125">Each **collection** field should be **searchable**, **filterable** and **facetable**.</span></span> <span data-ttu-id="8b330-126">Nyní byste měli mít ke spouštění dotazů jako:</span><span class="sxs-lookup"><span data-stu-id="8b330-126">You should be able to run queries like:</span></span>

* <span data-ttu-id="8b330-127">Najít všichni uživatelé, kteří pracují v ústředí"Adventureworks".</span><span class="sxs-lookup"><span data-stu-id="8b330-127">Find all people who work at the ‘Adventureworks Headquarters’.</span></span>
* <span data-ttu-id="8b330-128">Získáte počet lidí, kteří pracují v Home Office.</span><span class="sxs-lookup"><span data-stu-id="8b330-128">Get a count of the number of people who work in a ‘Home Office’.</span></span>  
* <span data-ttu-id="8b330-129">Osob, které fungují na Home Office zobrazit jaké ostatních kancelářích fungují společně s počet uživatelů v každé lokalitě.</span><span class="sxs-lookup"><span data-stu-id="8b330-129">Of the people who work at a ‘Home Office’, show what other offices they work along with a count of the people in each location.</span></span>  

<span data-ttu-id="8b330-130">Kde tato technika spadá rozdělovat je, když je potřeba udělat hledání, které kombinuje id umístění jak popis umístění.</span><span class="sxs-lookup"><span data-stu-id="8b330-130">Where this technique falls apart is when you need to do a search that combines both the location id as well as the location description.</span></span> <span data-ttu-id="8b330-131">Například:</span><span class="sxs-lookup"><span data-stu-id="8b330-131">For example:</span></span>

* <span data-ttu-id="8b330-132">Najít všechny osoby, které mají Home Office a mají ID umístění na 4.</span><span class="sxs-lookup"><span data-stu-id="8b330-132">Find all people where they have a Home Office AND has a location ID of 4.</span></span>  

<span data-ttu-id="8b330-133">Pokud si Vzpomínáte, původní obsah hledá takto:</span><span class="sxs-lookup"><span data-stu-id="8b330-133">If you recall the original content looked like this:</span></span>

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

<span data-ttu-id="8b330-134">Ale nyní, když budeme mít oddělené data do samostatných polí, jsme nijak starostí, protože pokud Home Office pro Jen Campbell má vztah k `locationsID 3` nebo `locationsID 4`.</span><span class="sxs-lookup"><span data-stu-id="8b330-134">However, now that we have separated the data into separate fields, we have no way of knowing if the Home Office for Jen Campbell relates to `locationsID 3` or `locationsID 4`.</span></span>  

<span data-ttu-id="8b330-135">Chcete-li zpracovávat tento případ, definujte jiné pole v indexu, které se kombinuje všechna data do jedné kolekce.</span><span class="sxs-lookup"><span data-stu-id="8b330-135">To handle this case, define another field in the index that combines all of the data into a single collection.</span></span>  <span data-ttu-id="8b330-136">Pro náš příklad jsme bude volat v tomto poli `locationsCombined` a jsme dojde k oddělení obsah s `||` však můžete zvolit jakékoli oddělovač, který se domníváte, že by jedinečnou sadu znaků pro obsah.</span><span class="sxs-lookup"><span data-stu-id="8b330-136">For our example, we will call this field `locationsCombined` and we will separate the content with a `||` although you can choose any separator that you think would be a unique set of characters for your content.</span></span> <span data-ttu-id="8b330-137">Například:</span><span class="sxs-lookup"><span data-stu-id="8b330-137">For example:</span></span> 

![Ukázková data, 2 řádky s oddělovačem](./media/search-howto-complex-data-types/sample-data-2.png)

<span data-ttu-id="8b330-139">Pomocí této `locationsCombined` pole, jsme teď zvládne i další dotazy, například:</span><span class="sxs-lookup"><span data-stu-id="8b330-139">Using this `locationsCombined` field, we can now accommodate even more queries, such as:</span></span>

* <span data-ttu-id="8b330-140">Zobrazí počet uživatelů, kteří pracovat na 'domovské úřadu, s umístěním Id "4".</span><span class="sxs-lookup"><span data-stu-id="8b330-140">Show a count of people who work at a ‘Home Office’ with location Id of ‘4’.</span></span>  
* <span data-ttu-id="8b330-141">Vyhledejte lidem, kteří pracují v Home Office s umístěním Id "4".</span><span class="sxs-lookup"><span data-stu-id="8b330-141">Search for people who work at a ‘Home Office’ with location Id ‘4’.</span></span> 

## <a name="limitations"></a><span data-ttu-id="8b330-142">Omezení</span><span class="sxs-lookup"><span data-stu-id="8b330-142">Limitations</span></span>
<span data-ttu-id="8b330-143">Tato metoda je užitečná pro různé scénáře, ale není možné v každém případě ho.</span><span class="sxs-lookup"><span data-stu-id="8b330-143">This technique is useful for a number of scenarios, but it is not applicable in every case.</span></span>  <span data-ttu-id="8b330-144">Například:</span><span class="sxs-lookup"><span data-stu-id="8b330-144">For example:</span></span>

1. <span data-ttu-id="8b330-145">Pokud nemáte sadu statických polí v komplexního datového typu a se žádný způsob, jak všechny možné typy mapování na jedno pole.</span><span class="sxs-lookup"><span data-stu-id="8b330-145">If you do not have a static set of fields in your complex data type and there was no way to map all the possible types to a single field.</span></span> 
2. <span data-ttu-id="8b330-146">Aktualizace vnořených objektů vyžaduje další práci k určení toho, co přesně je aktualizovat v indexu Azure Search</span><span class="sxs-lookup"><span data-stu-id="8b330-146">Updating the nested objects requires some extra work to determine exactly what needs to be updated in the Azure Search index</span></span>

## <a name="sample-code"></a><span data-ttu-id="8b330-147">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="8b330-147">Sample code</span></span>
<span data-ttu-id="8b330-148">Můžete zobrazit příklad o tom, jak index komplexní JSON datové sady Azure Search a provádět spoustu dotazy přes tuto datovou sadu v této [úložiště GitHub](https://github.com/liamca/AzureSearchComplexTypes).</span><span class="sxs-lookup"><span data-stu-id="8b330-148">You can see an example on how to index a complex JSON data set into Azure Search and perform a number of queries over this dataset at this [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes).</span></span>

## <a name="next-step"></a><span data-ttu-id="8b330-149">Další krok</span><span class="sxs-lookup"><span data-stu-id="8b330-149">Next step</span></span>
<span data-ttu-id="8b330-150">[Hlas pro nativní podpora pro komplexní datové typy](https://feedback.azure.com/forums/263029-azure-search) na UserVoice hledání Azure stránky a zadání žádné další, které chcete nám ke zvážení při výběru implementace funkce.</span><span class="sxs-lookup"><span data-stu-id="8b330-150">[Vote for native support for complex data types](https://feedback.azure.com/forums/263029-azure-search) on the Azure Search UserVoice page and provide any additional input that you’d like us to consider regarding feature implementation.</span></span> <span data-ttu-id="8b330-151">Vám může také oslovení mi přímo na Twitteru v @liamca.</span><span class="sxs-lookup"><span data-stu-id="8b330-151">You can also reach out to me directly on Twitter at @liamca.</span></span>

