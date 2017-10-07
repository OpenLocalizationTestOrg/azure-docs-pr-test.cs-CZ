---
title: "aaaAzure vyhledávání více jazyk | Microsoft Docs"
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
ms.openlocfilehash: 9a2e567a82ee563521c12ea320f6c484a8e73f04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a><span data-ttu-id="8bc0e-103">Vytvoření indexu pro dokumenty v několika jazycích ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="8bc0e-103">Create an index for documents in multiple languages in Azure Search</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="8bc0e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8bc0e-104">Portal</span></span>](search-language-support.md)
> * [<span data-ttu-id="8bc0e-105">REST</span><span class="sxs-lookup"><span data-stu-id="8bc0e-105">REST</span></span>](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [<span data-ttu-id="8bc0e-106">.NET</span><span class="sxs-lookup"><span data-stu-id="8bc0e-106">.NET</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

<span data-ttu-id="8bc0e-107">Unleashing hello analyzátory jazyka je stejně snadná jako jednu vlastnost nastavení na prohledávatelné pole v definici indexu hello.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-107">Unleashing hello power of language analyzers is as easy as setting one property on a searchable field in hello index definition.</span></span> <span data-ttu-id="8bc0e-108">Nyní můžete provést tento krok hello portálu.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-108">Now you can do this step in hello portal.</span></span>

<span data-ttu-id="8bc0e-109">Níže jsou snímky obrazovky hello oken Azure Portal pro službu Azure Search, který umožní uživatelům toodefine schématu indexu.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-109">Below are screenshots of hello Azure Portal blades for Azure Search that allow users toodefine an index schema.</span></span> <span data-ttu-id="8bc0e-110">V tomto okně můžete uživatelům vytvořit všechna pole hello a nastavte vlastnost hello analyzátor pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-110">From this blade, users can create all of hello fields and set hello analyzer property for each of them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8bc0e-111">Můžete nastavit pouze analyzátor jazyka během definice pole, jako v při vytváření nového indexu z hello pozadí nebo při přidávání nové pole tooan existující index.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-111">You can only set a language analyzer during field definition, as in when creating a new index from hello ground up, or when adding a new field tooan existing index.</span></span> <span data-ttu-id="8bc0e-112">Ujistěte se, že zadáte plně všechny atributy, včetně hello analyzátor, při vytváření hello pole.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-112">Make sure you fully specify all attributes, including hello analyzer, while creating hello field.</span></span> <span data-ttu-id="8bc0e-113">Nebude se moct tooedit hello atributy nebo změnit typ analyzátor hello po uložení změn.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-113">You won't be able tooedit hello attributes or change hello analyzer type once you save your changes.</span></span>
>
>

## <a name="define-a-new-field-definition"></a><span data-ttu-id="8bc0e-114">Zadejte novou definici pole</span><span class="sxs-lookup"><span data-stu-id="8bc0e-114">Define a new field definition</span></span>
1. <span data-ttu-id="8bc0e-115">Přihlaste se toohello [portál Azure](https://portal.azure.com) a otevřete hello služby okno služby search.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-115">Sign in toohello [Azure portal](https://portal.azure.com) and open hello service blade of your search service.</span></span>
2. <span data-ttu-id="8bc0e-116">Klikněte na tlačítko **přidat index** v příkazu hello panel hello horní části toostart řídicí panel služby hello nového indexu nebo otevřete stávající index tooset analyzátor na nová pole, které přidáváte tooan existující index.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-116">Click **Add index** in hello command bar at hello top of hello service dashboard toostart a new index, or open an existing index tooset an analyzer on new fields you're adding tooan existing index.</span></span>
3. <span data-ttu-id="8bc0e-117">Otevře se okno pole Hello, možnostmi pro definování schématu hello hello indexu, včetně hello analyzátor karta používá pro výběr analyzátor jazyka.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-117">hello Fields blade appears, giving you options for defining hello schema of hello index, including hello Analyzer tab used for choosing a language analyzer.</span></span>
4. <span data-ttu-id="8bc0e-118">V polích začněte definice pole názvem, zvolit hello datový typ a nastavení atributů toomark hello pole jako textu v plném znění s možností vyhledávání, získat ve výsledcích hledání, dá se použít v struktury omezující vlastnost navigace, řazení a podobně.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-118">In Fields, start a field definition by providing a name, choosing hello data type, and setting attributes toomark hello field as full text searchable, retrievable in search results, usable in facet navigation structures, sortable, and so forth.</span></span>
5. <span data-ttu-id="8bc0e-119">Než budete pokračovat na další pole toohello, otevřete hello **analyzátor** kartě.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-119">Before moving on toohello next field, open hello **Analyzer** tab.</span></span>

<span data-ttu-id="8bc0e-120">![][1]
*tooselect analyzátor, klikněte na kartu analyzátor hello v okně pole hello*</span><span class="sxs-lookup"><span data-stu-id="8bc0e-120">![][1]
*tooselect an analyzer, click hello Analyzer tab on hello Fields blade*</span></span>

## <a name="choose-an-analyzer"></a><span data-ttu-id="8bc0e-121">Zvolte analyzátoru</span><span class="sxs-lookup"><span data-stu-id="8bc0e-121">Choose an analyzer</span></span>
1. <span data-ttu-id="8bc0e-122">Posuňte se pole hello toofind, kterou definujete.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-122">Scroll toofind hello field you are defining.</span></span>
2. <span data-ttu-id="8bc0e-123">Pokud jste neoznačili hello pole jako prohledávatelný, klikněte na tlačítko hello políčko nyní toomark jej jako **Searchable**.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-123">If you haven't marked hello field as searchable, click hello checkbox now toomark it as **Searchable**.</span></span>
3. <span data-ttu-id="8bc0e-124">Klikněte na tlačítko hello analyzátor oblasti toodisplay hello seznam dostupných analyzátorů.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-124">Click hello Analyzer area toodisplay hello list of available analyzers.</span></span>
4. <span data-ttu-id="8bc0e-125">Zvolte hello analyzátor chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-125">Choose hello analyzer you want toouse.</span></span>

<span data-ttu-id="8bc0e-126">![][2]
*Vyberte jednu z analyzátorů hello podporované pro každé pole*</span><span class="sxs-lookup"><span data-stu-id="8bc0e-126">![][2]
*Select one of hello supported analyzers for each field*</span></span>

<span data-ttu-id="8bc0e-127">Ve výchozím nastavení, všechna prohledatelná pole použijte hello [standardní Lucene analyzátor](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) tedy bez ohledu na jazyk.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-127">By default, all searchable fields use hello [Standard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) which is language agnostic.</span></span> <span data-ttu-id="8bc0e-128">tooview hello úplný seznam podporovaných analyzátorů najdete v tématu [jazyková podpora ve službě Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bc0e-128">tooview hello full list of supported analyzers, see [Language Support in Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).</span></span>

<span data-ttu-id="8bc0e-129">Až analyzátor jazyka hello je vybraná pro pole, se použije spolu s každou žádostí indexování a vyhledávání pro toto pole.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-129">Once hello language analyzer is selected for a field, it will be used with each indexing and search request for that field.</span></span> <span data-ttu-id="8bc0e-130">Při dotazu je vydaný pro více polí pomocí různých analyzátorů, hello dotazu hello správné analyzátory pro každé pole zpracovává nezávisle.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-130">When a query is issued against multiple fields using different analyzers, hello query will be processed independently by hello right analyzers for each field.</span></span>

<span data-ttu-id="8bc0e-131">Mnoho webových a mobilních aplikací slouží uživatelé kolem hello zeměkouli pomocí různých jazycích.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-131">Many web and mobile applications serve users around hello globe using different languages.</span></span> <span data-ttu-id="8bc0e-132">Je možné toodefine index pro scénáře, jako to vytvořením pole pro každý jazyk podporovaný.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-132">It’s possible toodefine an index for a scenario like this by creating a field for each language supported.</span></span>

<span data-ttu-id="8bc0e-133">![][3]
*Definici indexu s pole popisu pro každý jazyk podporovaný*</span><span class="sxs-lookup"><span data-stu-id="8bc0e-133">![][3]
*Index definition with a description field for each language supported*</span></span>

<span data-ttu-id="8bc0e-134">Pokud je znám hello jazyk hello agenta zadání dotazu žádost o vyhledávání může být vymezená tooa konkrétní pole pomocí hello **searchFields** parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-134">If hello language of hello agent issuing a query is known, a search request can be scoped tooa specific field using hello **searchFields** query parameter.</span></span> <span data-ttu-id="8bc0e-135">Následující dotaz Hello bude vydaný pouze pro popis hello v polština:</span><span class="sxs-lookup"><span data-stu-id="8bc0e-135">hello following query will be issued only against hello description in Polish:</span></span>

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

<span data-ttu-id="8bc0e-136">Indexu z hello portálu, se můžete dotazovat pomocí **Průzkumník služby Search** toopaste v dotazu podobné toohello, jeden uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-136">You can query your index from hello portal, using **Search explorer** toopaste in a query similar toohello one shown above.</span></span> <span data-ttu-id="8bc0e-137">Průzkumník služby Search je k dispozici z panelu hello příkazů v okně služby hello.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-137">Search explorer is available from hello command bar in hello service blade.</span></span> <span data-ttu-id="8bc0e-138">V tématu [dotazování indexu Azure Search na portálu hello](search-explorer.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-138">See [Query your Azure Search index in hello portal](search-explorer.md) for details.</span></span>

<span data-ttu-id="8bc0e-139">Někdy hello jazyk hello agenta zadání dotazu není znám, ve které případu hello dotazu může být vydaný pro všechna pole současně.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-139">Sometimes hello language of hello agent issuing a query is not known, in which case hello query can be issued against all fields simultaneously.</span></span> <span data-ttu-id="8bc0e-140">V případě potřeby předvolby výsledků v určitém jazyce, lze definovat pomocí [vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bc0e-140">If needed, preference for results in a certain language can be defined using [scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span> <span data-ttu-id="8bc0e-141">Následující příklad hello bude mít shodami v popisu hello v angličtině skóre vyšší relativní toomatches v polština a francouzštinu:</span><span class="sxs-lookup"><span data-stu-id="8bc0e-141">In hello example below, matches found in hello description in English will be scored higher relative toomatches in Polish and French:</span></span>

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

<span data-ttu-id="8bc0e-142">Pokud jste vývojář .NET, Všimněte si, že můžete nakonfigurovat pomocí hello analyzátory jazyka [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search).</span><span class="sxs-lookup"><span data-stu-id="8bc0e-142">If you're a .NET developer, note that you can configure language analyzers using hello [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search).</span></span> <span data-ttu-id="8bc0e-143">nejnovější verze Hello zahrnuje podporu pro hello Microsoft analyzátory jazyka a.</span><span class="sxs-lookup"><span data-stu-id="8bc0e-143">hello latest release includes support for hello Microsoft language analyzers as well.</span></span>

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
