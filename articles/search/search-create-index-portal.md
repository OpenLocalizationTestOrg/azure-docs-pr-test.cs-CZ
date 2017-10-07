---
title: "AAA \"Vytvoření indexu (portál – Azure Search) | Microsoft Docs\""
description: "Vytvoření indexu pomocí hello portálu Azure."
services: search
manager: jhubbard
author: heidisteen
documentationcenter: 
ms.assetid: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/20/2017
ms.author: heidist
ms.openlocfilehash: 4c5d663499bf73a8883690aa9482b6ba59d1ddf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index-using-hello-azure-portal"></a>Vytvoření indexu Azure Search pomocí portálu Azure hello
> [!div class="op_single_selector"]
> * [Přehled](search-what-is-an-index.md)
> * [Azure Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Použít Návrháře hello předdefinované indexu v portálu Azure tooprototype nebo vytvořte [indexu vyhledávání](search-what-is-an-index.md) toorun na služby Azure Search. 

## <a name="prerequisites"></a>Požadavky

Tento článek předpokládá [předplatného Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) a [služby Azure Search](search-create-service-portal.md).  

## <a name="find-your-search-service"></a>Vyhledejte svoji službu vyhledávání
1. Přihlaste se toohello stránky portálu Azure a zkontrolujte hello [vyhledávací služby pro vaše předplatné](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Vyberte službu Azure Search.

## <a name="name-hello-index"></a>Název indexu hello

1. Klikněte na tlačítko hello **přidat index** tlačítka na panelu příkazů hello hello horní části stránky hello.
2. Název indexu Azure Search. 
   * Začínat písmenem.
   * Použít jenom malá písmena, číslice nebo spojovníky ("-").
   * Omezit hello název too60 znaků.

  Název indexu Hello stane součástí adresy URL koncového bodu hello použít v indexu toohello připojení a pro odesílání požadavků HTTP v hello REST API služby Azure Search.

## <a name="define-hello-fields-of-your-index"></a>Definování hello polí indexu

Zahrnuje sestavení indexu *polí kolekce* , který definuje hello prohledávatelná data v indexu. Přesněji řečeno určuje hello struktura dokumentů, které můžete nahrávat na server samostatně. kolekce polí Hello zahrnuje povinná a nepovinná pole s názvem a zadán s toodetermine atributy indexu použití pole hello.

1. V hello **přidat Index** okně klikněte na tlačítko **pole >** tooslide otevřete okno Definice pole hello. 

2. Přijměte hello generované *klíč* pole typu Edm.String. Ve výchozím nastavení, je hello pole s názvem *id* ho mohli přejmenovat, tak dlouho, dokud splňuje hello řetězec, ale [pravidla po pojmenování](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Klíčové pole je povinné pro každý index Azure Search a musí být řetězec.

3. Přidáte pole toofully zadejte hello dokumenty, které odešlete. Pokud obsahovat dokumenty *id*, *hotelů název*, *adresu*, *města*, a *oblast*, vytvoření odpovídající pole pro každé z nich v indexu hello. Zkontrolujte hello [návrh pokyny v následující části hello](#design) pomoc při nastavování atributů.

4. Volitelně lze přidáte interně všechna pole, které se používají ve výrazech filtru. Atributy pole hello lze nastavit tooexclude pole z operace hledání.

5. Po dokončení klikněte na tlačítko **OK** toosave a vytvořit hello index.

## <a name="tips-for-adding-fields"></a>Tipy pro přidávání polí

Vytvoření indexu portálu hello je klávesnice náročné. Pomocí následujících tento pracovní postup minimalizujte kroky:

1. Nejprve vytvoříte seznam polí hello zadáním názvů a nastavení datových typů.

2. Potom pomocí hello zaškrtávacích políček hello horní části každé atribut toobulk hello nastavení povolit pro všechna pole a selektivně zrušte zaškrtnutí políček u hello několik polí, které ji nevyžadují. Například pole řetězce jsou obvykle prohledávatelné. Takto může klikněte na **Retrievable** a **Searchable** tooboth hello návratové hodnoty hello pole ve výsledcích hledání, jakož i povolit fulltextové vyhledávání na pole hello. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Pokyny k nastavení atributů návrhu

I když můžete kdykoli přidat nová pole, jsou zamčené existující definice pole dobu jeho existence hello hello indexu. Z tohoto důvodu vývojáři obvykle používají pro vytvoření jednoduché indexy, testování nápady nebo pomocí toolook hello stránky portálu nastavovat hello portálu. Časté iteraci přes návrh rejstříku je efektivnější, pokud budete postupovat podle použití metody založené na kódu, takže můžete znovu sestavit hello index snadno.

Analyzátory a trochu jsou přidruženy k pole před uložením hello index. Být jisti tooclick prostřednictvím každý záložkách stránky tooadd jazyk analyzátorů nebo trochu tooyour definici indexu.

Řetězec pole jsou často označena jako **Searchable** a **Retrievable**.

Výsledky hledání toonarrow pole sloužící obsahovat **Sortable**, **Filterable**, a **Facetable**.

Atributy pole určit, jak se používá pole, jako je například, zda je použit v fulltextové vyhledávání, Fasetové navigace, operace řazení a tak dále. Hello následující tabulka popisuje každý atribut.

|Atribut|Popis|  
|---------------|-----------------|  
|**s možností vyhledávání**|Fulltextové prohledávatelné, předmět toolexical analysis například dělení slov během indexování. Pokud nastavíte hodnotu prohledávatelné pole tooa jako "slunečného dne", interně jej bude možné rozdělit na hello jednotlivých tokeny "slunečného" a "dne". Podrobnosti najdete v tématu [jak úplné textové vyhledávání funguje](search-lucene-query-architecture.md).|  
|**filtrování**|Odkazovaná v **$filter** dotazy. Filtrování pole typu `Edm.String` nebo `Collection(Edm.String)` nejsou prováděny dělení slov, tak porovnání jsou pro jenom přesné shody. Například pokud nastavíte toto pole f příliš "slunečného dne" `$filter=f eq 'sunny'` se najít žádné shody, ale `$filter=f eq 'sunny day'` bude. |  
|**řazení**|Ve výchozím nastavení systém hello řadí výsledky podle skóre, ale můžete nakonfigurovat na základě polí v dokumentech hello řazení. Pole typu `Collection(Edm.String)` nemůže být **řazení**. |  
|**facetable (kategorizovatelné)**|Obvykle se používá v prezentaci výsledky hledání, který obsahuje počet přístupů podle kategorie (například hotels v konkrétním městě). Tuto možnost nelze použít s pole typu `Edm.GeographyPoint`. Pole typu `Edm.String` , které jsou **filtrovatelných**, **řazení**, nebo **facetable** může být maximálně 32 kilobajtů délku. Podrobnosti najdete v tématu [vytvoření indexu (rozhraní API REST)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**klíč**|Jedinečný identifikátor pro dokumenty v indexu hello. Právě jedno pole musí být zvolena jako klíčové pole hello a musí být typu `Edm.String`.|  
|**získat**|Určuje, zda může být hello pole vrácené ve výsledku hledání. To je užitečné, když chcete, aby toouse pole (například *zisku*) jako filtr, řazení, nebo vyhodnocování mechanismus, ale proveďte není chcete hello pole toobe viditelné toohello koncového uživatele. Tento atribut musí být `true` pro `key` pole.|  

## <a name="create-hello-hotels-index-used-in-example-api-sections"></a>Vytvoření indexu hotels hello používá v částech příklad rozhraní API

Dokumentace k Azure Search rozhraní API obsahuje příklady kódu jednoduchý vybaveným *hotels* index. V hello následujících snímcích obrazovky, uvidíte definici indexu hello, včetně analyzátor francouzštinu hello zadaný během definici indexu, které můžete znovu vytvořit jako cvičení postupem hello portálu.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Další kroky

Po vytvoření indexu Azure Search, můžete přesunout toohello další krok: [nahrát prohledávatelná data do indexu hello](search-what-is-data-import.md).

Alternativně může trvat i hlubší pohled na indexy. Kromě toho toohello kolekce polí, indexu také určuje analyzátorů, trochu, vyhodnocování profily a nastavení CORS. Hello portál poskytuje stránky s kartami pro definování běžných prvků hello: polí, analyzátorů a trochu. toocreate nebo upravte další prvky, můžete použít hello REST API nebo .NET SDK.

## <a name="see-also"></a>Viz také

 [Jak funguje fulltextové vyhledávání](search-lucene-query-architecture.md)  
 [Rozhraní API REST služby vyhledávání](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

