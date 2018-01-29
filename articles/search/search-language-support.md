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
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Vytvoření indexu pro dokumenty v několika jazycích ve službě Azure Search
> [!div class="op_single_selector"]
>
> * [Azure Portal](search-language-support.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

Unleashing sílu analyzátory jazyka je stejně snadná jako jednu vlastnost nastavení prohledávatelné pole v definici indexu. Nyní můžete provést tento krok na portálu.

Níže jsou snímky obrazovky oken Azure Portal pro službu Azure Search, který povoluje uživatelům definovat schématu indexu. V tomto okně můžete uživatelům vytvořit všechna pole a nastavte vlastnost analyzátor pro každý z nich.

> [!IMPORTANT]
> Můžete nastavit pouze analyzátor jazyka během definice pole, jako v při vytváření nového indexu od základů nahoru nebo při přidávání nové pole do stávajícího indexu. Ujistěte se, že zadáte plně všechny atributy, včetně analyzátor, při vytváření pole. Nebudete moci upravit atributy nebo změňte typ analyzátor po uložení změn.
>
>

## <a name="define-a-new-field-definition"></a>Zadejte novou definici pole
1. Přihlaste se k [portál Azure](https://portal.azure.com) a otevřete okno služby služby search.
2. Klikněte na tlačítko **přidat index** na panelu příkazů v horní části řídicím panelu služby ke spuštění nového indexu, nebo otevřít existující index nastavení analyzátor na nová pole, které přidáváte do existujícího indexu.
3. Otevře se okno pole, možnostmi pro definování schématu indexu, včetně kartě analyzátor používá pro výběr analyzátor jazyka.
4. V polích spusťte definice pole názvem, výběr typu data a nastavení atributy označit pole jako textu v plném znění s možností vyhledávání, získat ve výsledcích hledání, dá se použít v struktury omezující vlastnost navigace, řazení a podobně.
5. Než budete pokračovat na další pole, otevřete **analyzátor** kartě.

![][1]
*Pokud chcete vybrat analyzátor, klikněte na kartu analyzátor v okně pole*

## <a name="choose-an-analyzer"></a>Zvolte analyzátoru
1. Posuňte se najít pole, které definujete.
2. Pokud jste neoznačili pole jako prohledávatelný, klikněte na zaškrtávací políčko nyní a označte ji jako **Searchable**.
3. Klikněte na tlačítko oblasti analyzátor zobrazíte seznam dostupných analyzátorů.
4. Zvolte analyzátor, který chcete použít.

![][2]
*Vyberte jednu z podporovaných analyzátory pro každé pole*

Ve výchozím nastavení, všechna prohledatelná pole použít [standardní Lucene analyzátor](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) tedy bez ohledu na jazyk. Pokud chcete zobrazit úplný seznam podporovaných analyzátorů, najdete v části [jazyková podpora ve službě Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Po výběru analyzátor jazyka pro pole se použije spolu s každou žádostí indexování a vyhledávání pro toto pole. Při dotazu je vydaný pro více polí pomocí různých analyzátorů, dotaz správné analyzátory pro každé pole zpracovává nezávisle.

Mnoho webových a mobilních aplikací slouží uživatelé po celém světě pomocí různých jazycích. Je možné definovat index pro scénáře, jako je to tak, že vytvoříte pole pro každý jazyk podporovaný.

![][3]
*Definici indexu s pole popisu pro každý jazyk podporovaný*

Pokud je znám jazyk agenta zadání dotazu může být žádost o vyhledávání vymezena na konkrétní pole pomocí **searchFields** parametr dotazu. Následující dotaz bude vydaný pouze pro popis v polština:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

Indexu z portálu, se můžete dotazovat pomocí **Průzkumník služby Search** vložit v podobné výše uvedeném dotazu. Průzkumník služby Search je dostupné na panelu příkazů v okně služby. V tématu [dotazování indexu Azure Search na portálu](search-explorer.md) podrobnosti.

Někdy není znám jazyk agenta zadání dotazu, v takovém případě dotazu může být vydaný pro všechna pole současně. V případě potřeby předvolby výsledků v určitém jazyce, lze definovat pomocí [vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx). V následujícím příkladu bude mít shodami v popisu v angličtině skóre vyšší relativně k odpovídá v polština a francouzštinu:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

Pokud jste vývojář .NET, Všimněte si, že můžete nakonfigurovat pomocí analyzátory jazyka [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search). Nejnovější verze zahrnuje podporu pro analyzátory jazyka Microsoft také.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
