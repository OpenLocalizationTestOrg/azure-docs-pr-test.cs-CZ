---
Title: aaa "Azure Analysis Services kurz Lekce 4: vytvoření relací | Microsoft Docs"Popis: Popisuje, jak hello toocreate vztahy v kurzu projektu Azure Analysis Services. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend
---
# <a name="lesson-4-create-relationships"></a>Lekce 4: Vytvoření relací

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci ověřte hello vztahy, které byly vytvořeny automaticky při importu dat a přidat nové vztahy mezi různých tabulek. Relace je připojení mezi dvěma tabulkami, které vytváří korelace hello data v těchto tabulkách. Například hello DimProduct tabulka a tabulka DimProductSubcategory hello mít vztah podle hello fakt, že každý produkt patří tooa podkategorie. Další, najdete v části toolearn [vztahy](https://docs.microsoft.com/sql/analysis-services/tabular-models/relationships-ssas-tabular).
  
Odhadovaný čas toocomplete této lekci: **10 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [Lekce 3: označte jej jako tabulka kalendářních dat](../tutorials/aas-lesson-3-mark-as-date-table.md). 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Kontrola existujících relací a přidání nových relací  
Při importu dat pomocí načíst Data, vy máte sedm tabulky z databáze AdventureWorksDW2014 hello. Obecně platí Pokud importujete data z relační zdroje, existující relace se automaticky importují společně s hello data. Přesto byste však před tím, než budete pokračovat ve vytváření modelu, měli ověřit správné vytvoření těchto relací mezi tabulkami. Pro účely tohoto kurzu přidáte tři nové relace.  
  
#### <a name="tooreview-existing-relationships"></a>tooreview existující relace  
  
1.  Klikněte na tlačítko hello **modelu** nabídky > **zobrazení modelu** > **zobrazení diagramu**.  

    Hello modelu designer teď zobrazí v zobrazení diagramu, grafické formátu zobrazení všechny tabulky hello, kterou jste naimportovali řádků mezi nimi. Hello čáry mezi tabulkami označují hello vztahy, které byly vytvořeny automaticky při importu dat hello.
    
    ![aas-lesson4-diagram](../tutorials/media/aas-lesson4-diagram.png)
  
    Zahrnout libovolný počet tabulek hello nejblíže pomocí ovládacích prvků minimapa v pravém dolním rohu hello hello modelu návrháře. Můžete také klikněte a přetáhněte tabulky toodifferent umístění, spojením blíže tabulek, nebo jejich uvedení v určitém pořadí. Přesunutí tabulek nemá vliv na relace hello již mezi tabulkami hello. tooview všechny sloupce hello v konkrétní tabulku, klikněte na tlačítko a přetáhněte na tooexpand okraj tabulky nebo vytvořit menší.  
  
2.  Klikněte na souvislou čáru hello mezi hello **DimCustomer** tabulky a hello **DimGeography** tabulky. Plná čára Hello mezi těmito dvěma tabulkami ukazuje, že je tento vztah aktivní, to znamená, že použije se ve výchozím nastavení při výpočtu vzorce DAX.  
  
    Všimněte si hello **GeographyKey** sloupec v hello **DimCustomer** tabulky a hello **GeographyKey** sloupec v hello **DimGeography** tabulky Nyní obě se objeví v rámci pole. V těchto sloupcích se používají v relaci hello. Hello vlastností relace nyní také zobrazí v hello **vlastnosti** okno.  
  
    > [!TIP]  
    > Kromě toho toousing hello Návrhář model v zobrazení diagramu, můžete použít také hello Správa relací dialogové okno pole tooshow hello vztahy mezi všechny tabulky ve formátu tabulky. V Průzkumníku tabelárních modelů klikněte pravým tlačítkem na **Relace** > **Správa relací**.
  
3.  Ověřte následující vztahy hello byly vytvořeny při každé z tabulek hello byly importovány z databáze AdventureWorksDW hello:  
  
    |Aktivní|Table|Související vyhledávací tabulka|  
    |----------|---------|------------------------|  
    |Ano|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Ano|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Ano|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Ano|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Ano|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    Pokud některé z relací hello chybí, ověřte, že model obsahuje následující tabulky hello: DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory a FactInternetSales. Pokud tabulky z hello stejné připojení ke zdroji dat jsou importovány na samostatné časy, všechny vztahy mezi těmito tabulkami nejsou vytvořeny a musí být vytvořeny ručně.  

### <a name="take-a-closer-look"></a>Bližší prozkoumání
V zobrazení diagramu Všimněte si čísla na hello řádky, které se zobrazí hello relace mezi tabulkami, ŠIPKA a hvězdičku.

![aas-lesson4-line](../tutorials/media/aas-lesson4-line.png)

Šipka Hello znázorňuje směr filtru hello. Hello hvězdičky ukazuje, že tato tabulka je hello mnoho straně v relaci hello mohutnost a hello jeden ukazuje, že je tato tabulka hello jedné straně relace hello. Pokud potřebujete tooedit vztahu; například změnit směr filtru nebo mohutnost hello relace, klikněte dvakrát na hello vztah řádku tooopen hello dialogové okno Upravit vztah.

![aas-lesson4-edit](../tutorials/media/aas-lesson4-edit.png)

Tyto funkce jsou určené pro pokročilé dat, modelování a jsou mimo rámec tohoto návodu hello. Další, najdete v části toolearn [obousměrné křížové filtry pro tabulkové modely služby ve službě Analysis Services](https://docs.microsoft.com/sql/analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services).

V některých případech může být nutné toocreate další relace mezi tabulkami v váš model toosupport určité obchodní logiku. Pro účely tohoto kurzu potřebujete toocreate tři další vztahy mezi hello FactInternetSales tabulka a tabulka DimDate hello.  
  
#### <a name="tooadd-new-relationships-between-tables"></a>tooadd nové relace mezi tabulkami.  
  
1.  V Návrháři hello modelu, v hello **FactInternetSales** tabulky, klikněte na tlačítko a počkejte hello **OrderDate** sloupec a pak přetáhněte hello kurzor toohello **datum** sloupec v hello  **DimDate** tabulky a poté uvolněte.  

    Plná čára zobrazí jste vytvořili aktivní relaci mezi hello **OrderDate** sloupec v hello **Internet prodej** tabulky a hello **datum** sloupec v hello **Datum** tabulky. 
  
      ![aas-lesson4-new](../tutorials/media/aas-lesson4-new.png) 
  
    > [!NOTE]  
    > Při vytváření vztahů, je automaticky vybraný směr hello mohutnost a filtr mezi primární tabulce hello a hello související vyhledávací tabulka.  
  
2.  V hello **FactInternetSales** tabulky, klikněte na tlačítko a počkejte hello **DueDate** sloupec a pak přetáhněte hello kurzor toohello **datum** sloupec v hello **DimDate** tabulky a poté uvolněte.  
  
    Tečkovaná čára zobrazí jste vytvořili neaktivní relaci mezi hello **DueDate** sloupec v hello **FactInternetSales** tabulky a hello **datum** sloupec v Hello **DimDate** tabulky. Mezi tabulkami můžete mít víc relací, ale aktivní může vždy být pouze jedna relace. Neaktivní relace lze provést active tooperform speciální agregace v vlastní výrazů DAX.  
  
3.  Nakonec vytvořte ještě jednu relaci. V hello **FactInternetSales** tabulky, klikněte na tlačítko a počkejte hello **DatumOdeslání** sloupec a pak přetáhněte hello kurzor toohello **datum** sloupec v hello **DimDate** tabulky a poté uvolněte.  
    
     ![aas-lesson4-newinactive](../tutorials/media/aas-lesson4-newinactive.png)
  
## <a name="whats-next"></a>Co dále?
[Lekce 5: Vytvoření počítaných sloupců](../tutorials/aas-lesson-5-create-calculated-columns.md)
  
  
  
