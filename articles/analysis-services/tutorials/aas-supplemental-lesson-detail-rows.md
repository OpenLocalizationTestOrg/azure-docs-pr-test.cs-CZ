---
Title: aaa "kurz dodatečné lekce Azure Analysis Services: řádky s podrobnostmi | Microsoft Docs"Popis: Popisuje, jak toocreate a výraz řádky podrobností v hello kurz služby Azure Analysis Services.
služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend
---
# <a name="supplemental-lesson---detail-rows"></a>Doplňková lekce – Řádky podrobností

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této další lekci použijete hello toodefine DAX Editor vlastního výrazu řádky podrobností. Výraz řádky podrobností je vlastnost na míru, poskytuje koncovým uživatelům další informace o výsledcích hello agregovat míry. 
  
Odhadovaný čas toocomplete této lekci: **10 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma doplňkové lekce je součástí kurzu tabulkového modelování. Před provedením úlohy hello v této další lekci, by byly dokončeny všechny předchozí lekce nebo máte dokončený projekt modelu ukázkové společnosti Adventure Works Internet prodej.  
  
## <a name="what-do-we-need-toosolve"></a>Co dělat, potřebujeme toosolve?
Podívejme se na podrobnosti hello míry naše InternetTotalSales před přidáním výraz řádky podrobností.

1.  V sadě SSDT, klikněte na tlačítko hello **modelu** nabídky > **analyzovat v aplikaci Excel** tooopen aplikace Excel a vytvořit prázdnou kontingenční tabulku.
  
2.  V **pole kontingenční tabulky**, přidejte hello **InternetTotalSales** měření z tabulka FactInternetSales hello příliš**hodnoty**, **CalendarYear**z hello DimDate tabulky příliš**sloupce**, a **EnglishCountryRegionName** příliš**řádky**. Naše kontingenční tabulky nyní umožňuje nám agregované výsledky z měr InternetTotalSales hello oblasti a roku. 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. V hello kontingenční tabulky klikněte dvakrát na agregované hodnoty pro název oblasti a roce. Zde jsme dvakrát kliknuli hello hodnotu pro Austrálii a hello rok 2014. Otevře se nový list, který obsahuje data. Tato data ale nejsou moc užitečná.

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
Co jsme chcete toosee tady je tabulku obsahující sloupce a řádky dat, která přispívat toohello agregován výsledek naše InternetTotalSales míry. toodo, že jsme přidat výraz řádky podrobností jako vlastnost míry hello.

## <a name="add-a-detail-rows-expression"></a>Přidání výrazu řádků podrobností

#### <a name="toocreate-a-detail-rows-expression"></a>toocreate výraz řádky podrobností 
  
1. V sadě SSDT, v mřížce měr tabulka FactInternetSales hello, klikněte na tlačítko hello **InternetTotalSales** měr. 

2. V **vlastnosti** > **výraz řádky podrobností**, klikněte na tlačítko hello editor tlačítko tooopen hello editoru jazyka DAX.

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. V editoru jazyka DAX, zadejte následující výraz hello:

    ```
    SELECTCOLUMNS(
    FactInternetSales,
    "Sales Order Number", FactInternetSales[SalesOrderNumber],
    "Customer First Name", RELATED(DimCustomer[FirstName]),
    "Customer Last Name", RELATED(DimCustomer[LastName]),
    "City", RELATED(DimGeography[City]),
    "Order Date", FactInternetSales[OrderDate],
    "Internet Total Sales", [InternetTotalSales]
    )

    ```

    Tento výraz určuje názvy sloupců a měr výsledky z tabulka FactInternetSales hello a související tabulky se vrátí při poklepání výsledek agregované v kontingenční tabulce nebo sestavy.

4. Zpět v aplikaci Excel odstranit list hello vytvořili v kroku 3, pak poklikejte na agregovaná hodnota. Tentokrát s výraz řádky podrobností vlastnost definovanou pro míru hello, nový list otevře obsahující mnoho užitečnější data.

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. Znovu nasaďte model.

  
## <a name="see-also"></a>Viz také  
[Funkce SELECTCOLUMNS (DAX)](https://msdn.microsoft.com/library/mt761759.aspx)   
[Doplňková lekce – Dynamické zabezpečení](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Doplňková lekce – Nepravidelné hierarchie](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  
