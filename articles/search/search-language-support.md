---
title: "Jazyk více Azure Search | Microsoft Docs"
description: "Služba Azure Search podporuje 56 jazyky, využití analyzátory jazyka z Lucene a zpracování přirozeného jazyka technologie společnosti Microsoft."
services: search
documentationcenter: 
author: yahnoosh
manager: pablocas
editor: 
ms.assetid: 55a00b44-804d-41bb-9c96-e6ea498616f5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: jlembicz
ms.openlocfilehash: dbbab31bac66ce73dbf9883992713a2c16581e19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a><span data-ttu-id="fe98e-103">Vytvoření indexu pro dokumenty v několika jazycích ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="fe98e-103">Create an index for documents in multiple languages in Azure Search</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="fe98e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fe98e-104">Portal</span></span>](search-language-support.md)
> * [<span data-ttu-id="fe98e-105">REST</span><span class="sxs-lookup"><span data-stu-id="fe98e-105">REST</span></span>](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [<span data-ttu-id="fe98e-106">.NET</span><span class="sxs-lookup"><span data-stu-id="fe98e-106">.NET</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

<span data-ttu-id="fe98e-107">Unleashing sílu analyzátory jazyka je stejně snadná jako jednu vlastnost nastavení prohledávatelné pole v definici indexu.</span><span class="sxs-lookup"><span data-stu-id="fe98e-107">Unleashing the power of language analyzers is as easy as setting one property on a searchable field in the index definition.</span></span> <span data-ttu-id="fe98e-108">Nyní můžete provést tento krok na portálu.</span><span class="sxs-lookup"><span data-stu-id="fe98e-108">Now you can do this step in the portal.</span></span>

<span data-ttu-id="fe98e-109">Níže jsou snímky obrazovky oken Azure Portal pro službu Azure Search, který povoluje uživatelům definovat schématu indexu.</span><span class="sxs-lookup"><span data-stu-id="fe98e-109">Below are screenshots of the Azure Portal blades for Azure Search that allow users to define an index schema.</span></span> <span data-ttu-id="fe98e-110">V tomto okně můžete uživatelům vytvořit všechna pole a nastavte vlastnost analyzátor pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="fe98e-110">From this blade, users can create all of the fields and set the analyzer property for each of them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe98e-111">Můžete nastavit pouze analyzátor jazyka během definice pole, jako v při vytváření nového indexu od základů nahoru nebo při přidávání nové pole do stávajícího indexu.</span><span class="sxs-lookup"><span data-stu-id="fe98e-111">You can only set a language analyzer during field definition, as in when creating a new index from the ground up, or when adding a new field to an existing index.</span></span> <span data-ttu-id="fe98e-112">Ujistěte se, že zadáte plně všechny atributy, včetně analyzátor, při vytváření pole.</span><span class="sxs-lookup"><span data-stu-id="fe98e-112">Make sure you fully specify all attributes, including the analyzer, while creating the field.</span></span> <span data-ttu-id="fe98e-113">Nebudete moci upravit atributy nebo změňte typ analyzátor po uložení změn.</span><span class="sxs-lookup"><span data-stu-id="fe98e-113">You won't be able to edit the attributes or change the analyzer type once you save your changes.</span></span>
>
>

## <a name="define-a-new-field-definition"></a><span data-ttu-id="fe98e-114">Zadejte novou definici pole</span><span class="sxs-lookup"><span data-stu-id="fe98e-114">Define a new field definition</span></span>
1. <span data-ttu-id="fe98e-115">Přihlaste se k [portál Azure](https://portal.azure.com) a otevřete okno služby služby search.</span><span class="sxs-lookup"><span data-stu-id="fe98e-115">Sign in to the [Azure portal](https://portal.azure.com) and open the service blade of your search service.</span></span>
2. <span data-ttu-id="fe98e-116">Klikněte na tlačítko **přidat index** na panelu příkazů v horní části řídicím panelu služby ke spuštění nového indexu, nebo otevřít existující index nastavení analyzátor na nová pole, které přidáváte do existujícího indexu.</span><span class="sxs-lookup"><span data-stu-id="fe98e-116">Click **Add index** in the command bar at the top of the service dashboard to start a new index, or open an existing index to set an analyzer on new fields you're adding to an existing index.</span></span>
3. <span data-ttu-id="fe98e-117">Otevře se okno pole, možnostmi pro definování schématu indexu, včetně kartě analyzátor používá pro výběr analyzátor jazyka.</span><span class="sxs-lookup"><span data-stu-id="fe98e-117">The Fields blade appears, giving you options for defining the schema of the index, including the Analyzer tab used for choosing a language analyzer.</span></span>
4. <span data-ttu-id="fe98e-118">V polích spusťte definice pole názvem, výběr typu data a nastavení atributy označit pole jako textu v plném znění s možností vyhledávání, získat ve výsledcích hledání, dá se použít v struktury omezující vlastnost navigace, řazení a podobně.</span><span class="sxs-lookup"><span data-stu-id="fe98e-118">In Fields, start a field definition by providing a name, choosing the data type, and setting attributes to mark the field as full text searchable, retrievable in search results, usable in facet navigation structures, sortable, and so forth.</span></span>
5. <span data-ttu-id="fe98e-119">Než budete pokračovat na další pole, otevřete **analyzátor** kartě.</span><span class="sxs-lookup"><span data-stu-id="fe98e-119">Before moving on to the next field, open the **Analyzer** tab.</span></span>

<span data-ttu-id="fe98e-120">![][1]
*Pokud chcete vybrat analyzátor, klikněte na kartu analyzátor v okně pole*</span><span class="sxs-lookup"><span data-stu-id="fe98e-120">![][1]
*To select an analyzer, click the Analyzer tab on the Fields blade*</span></span>

## <a name="choose-an-analyzer"></a><span data-ttu-id="fe98e-121">Zvolte analyzátoru</span><span class="sxs-lookup"><span data-stu-id="fe98e-121">Choose an analyzer</span></span>
1. <span data-ttu-id="fe98e-122">Posuňte se najít pole, které definujete.</span><span class="sxs-lookup"><span data-stu-id="fe98e-122">Scroll to find the field you are defining.</span></span>
2. <span data-ttu-id="fe98e-123">Pokud jste neoznačili pole jako prohledávatelný, klikněte na zaškrtávací políčko nyní a označte ji jako **Searchable**.</span><span class="sxs-lookup"><span data-stu-id="fe98e-123">If you haven't marked the field as searchable, click the checkbox now to mark it as **Searchable**.</span></span>
3. <span data-ttu-id="fe98e-124">Klikněte na tlačítko oblasti analyzátor zobrazíte seznam dostupných analyzátorů.</span><span class="sxs-lookup"><span data-stu-id="fe98e-124">Click the Analyzer area to display the list of available analyzers.</span></span>
4. <span data-ttu-id="fe98e-125">Zvolte analyzátor, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="fe98e-125">Choose the analyzer you want to use.</span></span>

<span data-ttu-id="fe98e-126">![][2]
*Vyberte jednu z podporovaných analyzátory pro každé pole*</span><span class="sxs-lookup"><span data-stu-id="fe98e-126">![][2]
*Select one of the supported analyzers for each field*</span></span>

<span data-ttu-id="fe98e-127">Ve výchozím nastavení, všechna prohledatelná pole použít [standardní Lucene analyzátor](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) tedy bez ohledu na jazyk.</span><span class="sxs-lookup"><span data-stu-id="fe98e-127">By default, all searchable fields use the [Standard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) which is language agnostic.</span></span> <span data-ttu-id="fe98e-128">Pokud chcete zobrazit úplný seznam podporovaných analyzátorů, najdete v části [jazyková podpora ve službě Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).</span><span class="sxs-lookup"><span data-stu-id="fe98e-128">To view the full list of supported analyzers, see [Language Support in Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).</span></span>

<span data-ttu-id="fe98e-129">Po výběru analyzátor jazyka pro pole se použije spolu s každou žádostí indexování a vyhledávání pro toto pole.</span><span class="sxs-lookup"><span data-stu-id="fe98e-129">Once the language analyzer is selected for a field, it will be used with each indexing and search request for that field.</span></span> <span data-ttu-id="fe98e-130">Při dotazu je vydaný pro více polí pomocí různých analyzátorů, dotaz správné analyzátory pro každé pole zpracovává nezávisle.</span><span class="sxs-lookup"><span data-stu-id="fe98e-130">When a query is issued against multiple fields using different analyzers, the query will be processed independently by the right analyzers for each field.</span></span>

<span data-ttu-id="fe98e-131">Mnoho webových a mobilních aplikací slouží uživatelé po celém světě pomocí různých jazycích.</span><span class="sxs-lookup"><span data-stu-id="fe98e-131">Many web and mobile applications serve users around the globe using different languages.</span></span> <span data-ttu-id="fe98e-132">Je možné definovat index pro scénáře, jako je to tak, že vytvoříte pole pro každý jazyk podporovaný.</span><span class="sxs-lookup"><span data-stu-id="fe98e-132">It’s possible to define an index for a scenario like this by creating a field for each language supported.</span></span>

<span data-ttu-id="fe98e-133">![][3]
*Definici indexu s pole popisu pro každý jazyk podporovaný*</span><span class="sxs-lookup"><span data-stu-id="fe98e-133">![][3]
*Index definition with a description field for each language supported*</span></span>

<span data-ttu-id="fe98e-134">Pokud je znám jazyk agenta zadání dotazu může být žádost o vyhledávání vymezena na konkrétní pole pomocí **searchFields** parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="fe98e-134">If the language of the agent issuing a query is known, a search request can be scoped to a specific field using the **searchFields** query parameter.</span></span> <span data-ttu-id="fe98e-135">Následující dotaz bude vydaný pouze pro popis v polština:</span><span class="sxs-lookup"><span data-stu-id="fe98e-135">The following query will be issued only against the description in Polish:</span></span>

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

<span data-ttu-id="fe98e-136">Indexu z portálu, se můžete dotazovat pomocí **Průzkumník služby Search** vložit v podobné výše uvedeném dotazu.</span><span class="sxs-lookup"><span data-stu-id="fe98e-136">You can query your index from the portal, using **Search explorer** to paste in a query similar to the one shown above.</span></span> <span data-ttu-id="fe98e-137">Průzkumník služby Search je dostupné na panelu příkazů v okně služby.</span><span class="sxs-lookup"><span data-stu-id="fe98e-137">Search explorer is available from the command bar in the service blade.</span></span> <span data-ttu-id="fe98e-138">V tématu [dotazování indexu Azure Search na portálu](search-explorer.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="fe98e-138">See [Query your Azure Search index in the portal](search-explorer.md) for details.</span></span>

<span data-ttu-id="fe98e-139">Někdy není znám jazyk agenta zadání dotazu, v takovém případě dotazu může být vydaný pro všechna pole současně.</span><span class="sxs-lookup"><span data-stu-id="fe98e-139">Sometimes the language of the agent issuing a query is not known, in which case the query can be issued against all fields simultaneously.</span></span> <span data-ttu-id="fe98e-140">V případě potřeby předvolby výsledků v určitém jazyce, lze definovat pomocí [vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="fe98e-140">If needed, preference for results in a certain language can be defined using [scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span> <span data-ttu-id="fe98e-141">V následujícím příkladu bude mít shodami v popisu v angličtině skóre vyšší relativně k odpovídá v polština a francouzštinu:</span><span class="sxs-lookup"><span data-stu-id="fe98e-141">In the example below, matches found in the description in English will be scored higher relative to matches in Polish and French:</span></span>

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

<span data-ttu-id="fe98e-142">Pokud jste vývojář .NET, Všimněte si, že můžete nakonfigurovat pomocí analyzátory jazyka [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search).</span><span class="sxs-lookup"><span data-stu-id="fe98e-142">If you're a .NET developer, note that you can configure language analyzers using the [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search).</span></span> <span data-ttu-id="fe98e-143">Nejnovější verze zahrnuje podporu pro analyzátory jazyka Microsoft také.</span><span class="sxs-lookup"><span data-stu-id="fe98e-143">The latest release includes support for the Microsoft language analyzers as well.</span></span>

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
