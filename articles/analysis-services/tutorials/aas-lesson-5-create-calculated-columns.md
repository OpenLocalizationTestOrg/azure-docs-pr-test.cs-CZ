---
Title: aaa "Azure Analysis Services kurz Lekce 5: vytvoření počítané sloupce | Microsoft Docs"Popis: Popisuje způsob výpočtu toocreate sloupců v kurzu projekt hello Azure Analysis Services. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 01/06/2017 ms.author: owend
---
# <a name="lesson-5-create-calculated-columns"></a>Lekce 5: Vytvoření počítaných sloupců

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci vytvoříte v modelu data přidáním počítaných sloupců. Můžete vytvořit počítané sloupce (jako vlastní sloupce) při použití načíst Data, pomocí hello editoru dotazů, nebo později v hello modelu návrháře jako jste to zde. Další, najdete v části toolearn [počítaných sloupců](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).
  
Vytvoříte pět nových počítaných sloupců ve třech různých tabulkách. Postup Hello je mírně odlišný pro každý úkol zobrazující existuje několik způsobů toocreate sloupců, přejmenujte je a umístit je na různých místech v tabulce.  

V této lekci také poprvé použijete jazyk DAX (Data Analysis Expressions). DAX je speciální jazyk pro vytváření vysoce přizpůsobitelných výrazů se vzorci pro tabelární modely. V tomto kurzu použijete toocreate počítaného sloupce, míry a role filtry jazyka DAX. Další, najdete v části toolearn [jazyka DAX v tabulkové modely](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular). 
  
Odhadovaný čas toocomplete této lekci: **15 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [Lekce 4: vytvoření vztahů](../tutorials/aas-lesson-4-create-relationships.md). 
  
## <a name="create-calculated-columns"></a>Vytvoření počítaných sloupců  
  
#### <a name="create-a-monthcalendar-calculated-column-in-hello-dimdate-table"></a>Vytvoření MonthCalendar počítaného sloupce v tabulce DimDate hello  
  
1.  Klikněte na tlačítko hello **modelu** nabídky > **zobrazení modelu** > **zobrazení dat**.  
  
    Počítané sloupce lze vytvořit pouze pomocí návrháře hello model dat zobrazení.  
  
2.  V Návrháři hello model, klikněte na tlačítko hello **DimDate** tabulky (karta).  
  
3.  Klikněte pravým tlačítkem na hello **CalendarQuarter** záhlaví sloupce a pak klikněte na tlačítko **vložit sloupec**.  
  
    Nový sloupec s názvem **počítaného sloupce 1** je vložené toohello nalevo od hello **čtvrtletí kalendáře** sloupce.  
  
4.  V řádku vzorců hello výše hello tabulky, zadejte následující vzorec DAX hello: automatického dokončování pomáhá zadáte hello plně kvalifikované názvy sloupců a tabulek a seznamů hello funkce, které jsou k dispozici.  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    Hodnoty jsou pak vyplněný pro všechny řádky hello v počítaném sloupci hello. Pokud přejděte dolů prostřednictvím tabulky hello zobrazí řádky může mít různé hodnoty pro tento sloupec na základě dat hello v jednotlivých řádcích.    
  
5.  Přejmenujte tento sloupec příliš**MonthCalendar**. 

    ![aas-lesson5-newcolumn](../tutorials/media/aas-lesson5-newcolumn.png) 
  
počítaný sloupec Hello MonthCalendar poskytuje řazení název pro měsíc.  
  
#### <a name="create-a-dayofweek-calculated-column-in-hello-dimdate-table"></a>Vytvoření DayOfWeek počítaného sloupce v tabulce DimDate hello  
  
1.  S hello **DimDate** tabulky stále aktivní, klikněte na tlačítko hello **sloupec** nabídce a pak klikněte na tlačítko **přidat sloupec**.  
  
2.  V řádku vzorců hello zadejte hello následující vzorec:  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    Pokud jste dokončili vytváření hello vzorec, stiskněte klávesu ENTER. nový sloupec Hello se přidá toohello nejvíce vpravo hello tabulky.  
  
3.  Přejmenovat sloupec hello příliš**DayOfWeek**.  
  
4.  Kliknutím na záhlaví sloupce hello a poté přetáhněte hello sloupce mezi hello **EnglishDayNameOfWeek** sloupce a hello **DayNumberOfMonth** sloupce.  
  
    > [!TIP]  
    > Přesunutí sloupců v tabulce je snazší toonavigate.  
  
počítaný sloupec Hello DayOfWeek poskytuje řazení název hello den v týdnu.  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-hello-dimproduct-table"></a>Vytvoření ProductSubcategoryName počítaného sloupce v tabulce DimProduct hello  
  
  
1.  V hello **DimProduct** tabulky, posuňte toohello daleko vpravo od hello tabulky. Všimněte si hello nejvíce vpravo sloupec nazýval **přidat sloupec** (kurzívou), kliknutím na záhlaví sloupce hello.  
  
2.  V řádku vzorců hello zadejte hello následující vzorec:  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  Přejmenovat sloupec hello příliš**ProductSubcategoryName**.  
  
počítaný sloupec ProductSubcategoryName Hello je použité toocreate hierarchie v tabulce DimProduct hello, které zahrnují data z hello EnglishProductSubcategoryName sloupec v tabulce DimProductSubcategory hello. Hierarchie nemůžou zahrnovat víc než jednu tabulku. Hierarchie vytvoříte později v lekci 9.  
  
#### <a name="create-a-productcategoryname-calculated-column-in-hello-dimproduct-table"></a>Vytvoření ProductCategoryName počítaného sloupce v tabulce DimProduct hello  
  
1.  S hello **DimProduct** tabulky stále aktivní, klikněte na tlačítko hello **sloupec** nabídce a pak klikněte na tlačítko **přidat sloupec**.  
  
2.  V řádku vzorců hello zadejte hello následující vzorec:  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  Přejmenovat sloupec hello příliš**ProductCategoryName**.  
  
počítaný sloupec ProductCategoryName Hello je použité toocreate hierarchie v tabulce DimProduct hello, které zahrnují data z hello EnglishProductCategoryName sloupec v tabulce DimProductCategory hello. Hierarchie nemůžou zahrnovat víc než jednu tabulku.  
  
#### <a name="create-a-margin-calculated-column-in-hello-factinternetsales-table"></a>Vytvoření okraj počítaného sloupce v tabulce FactInternetSales hello  
  
1.  V Návrháři hello modelu, vyberte hello **FactInternetSales** tabulky.  
  
2.  Vytvořit nový počítaný sloupec mezi hello **SalesAmount** sloupce a hello **TaxAmt** sloupce.  
  
3.  V řádku vzorců hello zadejte hello následující vzorec:  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  Přejmenovat sloupec hello příliš**okraj**.  
 
      ![aas-lesson5-newmargin](../tutorials/media/aas-lesson5-newmargin.png)
      
    počítaný sloupec Hello okraj je použité tooanalyze zisku okrajů pro každý prodej.  
  
## <a name="whats-next"></a>Co dále?
[Lekce 6: Vytvoření měřítek](../tutorials/aas-lesson-6-create-measures.md)
  
  
  
