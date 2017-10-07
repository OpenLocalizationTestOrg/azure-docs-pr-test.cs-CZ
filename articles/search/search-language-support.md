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
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Vytvoření indexu pro dokumenty v několika jazycích ve službě Azure Search
> [!div class="op_single_selector"]
>
> * [Azure Portal](search-language-support.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

Unleashing hello analyzátory jazyka je stejně snadná jako jednu vlastnost nastavení na prohledávatelné pole v definici indexu hello. Nyní můžete provést tento krok hello portálu.

Níže jsou snímky obrazovky hello oken Azure Portal pro službu Azure Search, který umožní uživatelům toodefine schématu indexu. V tomto okně můžete uživatelům vytvořit všechna pole hello a nastavte vlastnost hello analyzátor pro každý z nich.

> [!IMPORTANT]
> Můžete nastavit pouze analyzátor jazyka během definice pole, jako v při vytváření nového indexu z hello pozadí nebo při přidávání nové pole tooan existující index. Ujistěte se, že zadáte plně všechny atributy, včetně hello analyzátor, při vytváření hello pole. Nebude se moct tooedit hello atributy nebo změnit typ analyzátor hello po uložení změn.
>
>

## <a name="define-a-new-field-definition"></a>Zadejte novou definici pole
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) a otevřete hello služby okno služby search.
2. Klikněte na tlačítko **přidat index** v příkazu hello panel hello horní části toostart řídicí panel služby hello nového indexu nebo otevřete stávající index tooset analyzátor na nová pole, které přidáváte tooan existující index.
3. Otevře se okno pole Hello, možnostmi pro definování schématu hello hello indexu, včetně hello analyzátor karta používá pro výběr analyzátor jazyka.
4. V polích začněte definice pole názvem, zvolit hello datový typ a nastavení atributů toomark hello pole jako textu v plném znění s možností vyhledávání, získat ve výsledcích hledání, dá se použít v struktury omezující vlastnost navigace, řazení a podobně.
5. Než budete pokračovat na další pole toohello, otevřete hello **analyzátor** kartě.

![][1]
*tooselect analyzátor, klikněte na kartu analyzátor hello v okně pole hello*

## <a name="choose-an-analyzer"></a>Zvolte analyzátoru
1. Posuňte se pole hello toofind, kterou definujete.
2. Pokud jste neoznačili hello pole jako prohledávatelný, klikněte na tlačítko hello políčko nyní toomark jej jako **Searchable**.
3. Klikněte na tlačítko hello analyzátor oblasti toodisplay hello seznam dostupných analyzátorů.
4. Zvolte hello analyzátor chcete toouse.

![][2]
*Vyberte jednu z analyzátorů hello podporované pro každé pole*

Ve výchozím nastavení, všechna prohledatelná pole použijte hello [standardní Lucene analyzátor](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) tedy bez ohledu na jazyk. tooview hello úplný seznam podporovaných analyzátorů najdete v tématu [jazyková podpora ve službě Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Až analyzátor jazyka hello je vybraná pro pole, se použije spolu s každou žádostí indexování a vyhledávání pro toto pole. Při dotazu je vydaný pro více polí pomocí různých analyzátorů, hello dotazu hello správné analyzátory pro každé pole zpracovává nezávisle.

Mnoho webových a mobilních aplikací slouží uživatelé kolem hello zeměkouli pomocí různých jazycích. Je možné toodefine index pro scénáře, jako to vytvořením pole pro každý jazyk podporovaný.

![][3]
*Definici indexu s pole popisu pro každý jazyk podporovaný*

Pokud je znám hello jazyk hello agenta zadání dotazu žádost o vyhledávání může být vymezená tooa konkrétní pole pomocí hello **searchFields** parametr dotazu. Následující dotaz Hello bude vydaný pouze pro popis hello v polština:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

Indexu z hello portálu, se můžete dotazovat pomocí **Průzkumník služby Search** toopaste v dotazu podobné toohello, jeden uvedené výše. Průzkumník služby Search je k dispozici z panelu hello příkazů v okně služby hello. V tématu [dotazování indexu Azure Search na portálu hello](search-explorer.md) podrobnosti.

Někdy hello jazyk hello agenta zadání dotazu není znám, ve které případu hello dotazu může být vydaný pro všechna pole současně. V případě potřeby předvolby výsledků v určitém jazyce, lze definovat pomocí [vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx). Následující příklad hello bude mít shodami v popisu hello v angličtině skóre vyšší relativní toomatches v polština a francouzštinu:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

Pokud jste vývojář .NET, Všimněte si, že můžete nakonfigurovat pomocí hello analyzátory jazyka [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search). nejnovější verze Hello zahrnuje podporu pro hello Microsoft analyzátory jazyka a.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
