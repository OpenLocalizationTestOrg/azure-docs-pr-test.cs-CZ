---
Title: aaa "Azure Analysis Services kurz lekce 6: vytvoření míry | Microsoft Docs"Popis: Popisuje, jak toocreate měří v kurzu projektu hello Azure Analysis Services. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 01/06/2017 ms.author: owend
---
# <a name="lesson-6-create-measures"></a>Lekce 6: Vytvoření měr

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci vytvoříte míry toobe součástí modelu. Podobně jako toohello počítaného sloupce, které jste vytvořili, míry je výpočet vytvořili pomocí vzorce DAX. Na rozdíl od počítaných sloupců se ale míry vyhodnocují na základě uživatelem vybraného *filtru*. Například konkrétní sloupec nebo průřez přidat pole toohello popisky řádků v kontingenční tabulce. Hodnotu pro každou buňku v hello filtru se pak počítá hello použít míru. Míry jsou efektivní, flexibilní výpočty, které chcete tooinclude v téměř všechny tabulkové modely tooperform dynamické výpočty v číselná data. Další, najdete v části toolearn [míry](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).
  
toocreate míry, použijte hello *mřížce měr*. Ve výchozím nastavení má každá tabulka prázdnou mřížku měr. Obvykle ale nevytváříte míry pro každou tabulku. níže uvedená tabulka v Návrháři hello model v zobrazení dat se zobrazí Hello měr mřížky. toohide nebo zobrazit mřížky měr hello pro tabulku, klikněte na tlačítko hello **tabulky** nabídce a pak klikněte na tlačítko **zobrazit mřížky měr**.  
  
Míra můžete vytvořit tak, že kliknete na prázdné buňky v mřížce měr hello a poté zadat vzorec jazyka DAX do řádku vzorců hello. Když klikněte ENTER toocomplete hello vzorec, hello měr, pak se zobrazí v buňce hello. Můžete vytvořit také pomocí kliknutím na sloupec standardní agregační funkce míry, a potom kliknutím na tlačítko AutoSum hello (**∑**) na panelu nástrojů hello. Míry vytvořený pomocí funkce AutoSum hello objeví v buňky v mřížce měr hello přímo pod hello sloupec, ale lze přesunout.  
  
V této lekci vytvoříte míry zadáním obou vzorec jazyka DAX do řádku vzorců hello a pomocí funkce AutoSum hello.  
  
Odhadovaný čas toocomplete této lekci: **30 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [Lekce 5: vytvoření počítané sloupce](../tutorials/aas-lesson-5-create-calculated-columns.md).  
  
## <a name="create-measures"></a>Vytvoření měr  
  
#### <a name="toocreate-a-dayscurrentquartertodate-measure-in-hello-dimdate-table"></a>toocreate DaysCurrentQuarterToDate měr v tabulce DimDate hello  
  
1.  V Návrháři hello model, klikněte na tlačítko hello **DimDate** tabulky.  
  
2.  V mřížce měr hello klikněte na tlačítko hello levého horního prázdné buňky.  
  
3.  V řádku vzorců hello zadejte hello následující vzorec:  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    Všimněte si hello levého horního buňka teď obsahuje název míry, **DaysCurrentQuarterToDate**, za nímž následují hello výsledek **92**.
    
      ![aas-lesson6-newmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    Na rozdíl od počítané sloupce se vzorci měr můžete zadat hello název míry, následovaným dvojtečkou, následuje hello vzorce výraz.

  
#### <a name="toocreate-a-daysincurrentquarter-measure-in-hello-dimdate-table"></a>toocreate DaysInCurrentQuarter měr v tabulce DimDate hello  
  
1.  S hello **DimDate** tabulky v Návrháři hello modelu, v mřížce měr hello stále aktivní, klikněte na tlačítko hello prázdné buňky pod hello měr, které jste vytvořili.  
  
2.  V řádku vzorců hello zadejte hello následující vzorec:  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    Při vytváření porovnání poměr mezi jeden neúplné období a hello předchozího období. Vzorec Hello musí vypočítat hello podíl hello období, kdy a porovnávají ho toohello stejné deklarovaná čistá nebo hrubá v hello předchozího období. V tomto případě [DaysCurrentQuarterToDate] / [DaysInCurrentQuarter] poskytuje hello část uplynulý čas v hello aktuální období.  
  
#### <a name="toocreate-an-internetdistinctcountsalesorder-measure-in-hello-factinternetsales-table"></a>toocreate InternetDistinctCountSalesOrder měr v tabulka FactInternetSales hello  
  
1.  Klikněte na tlačítko hello **FactInternetSales** tabulky.   
  
2.  Klikněte na tlačítko hello **SalesOrderNumber** záhlaví sloupce.  
  
3.  Na panelu nástrojů hello, klikněte na tlačítko Další toohello šipky dolů hello AutoSum (**∑**) tlačítko a potom vyberte **DistinctCount**.  
  
    funkce AutoSum Hello automaticky vytvoří míru pomocí vzorce standardní agregace DistinctCount hello vybrané sloupce hello.  
    
       ![aas-lesson6-newmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  V mřížce měr hello, klikněte na novou measure hello a potom v hello **vlastnosti** okno v **název míry**, přejmenujte hello míra příliš**InternetDistinctCountSalesOrder**. 
 
  
#### <a name="toocreate-additional-measures-in-hello-factinternetsales-table"></a>toocreate další míry v tabulka FactInternetSales hello  
  
1.  Pomocí funkce AutoSum hello vytvořte a pojmenujte hello následující míry:  

    |Sloupec|Název míry|AutoSum (∑)|Vzorec|  
    |----------------|----------|-----------------|-----------|  
    |SalesOrderLineNumber|InternetOrderLinesCount|Počet|=POČET2([SalesOrderLineNumber])|  
    |OrderQuantity|InternetTotalUnits|Součet|=SUMA([OrderQuantity])|  
    |DiscountAmount|InternetTotalDiscountAmount|Součet|=SUMA([DiscountAmount])|  
    |TotalProductCost|InternetTotalProductCost|Součet|=SUMA([TotalProductCost])|  
    |SalesAmount|InternetTotalSales|Součet|=SUMA([SalesAmount])|  
    |Margin|InternetTotalMargin|Součet|=SUMA([Margin])|  
    |TaxAmt|InternetTotalTaxAmt|Součet|=SUMA([TaxAmt])|  
    |Freight|InternetTotalFreight|Součet|=SUMA([Freight])|  
  
2.  Kliknutím na vytvořit prázdné buňky v mřížce měr hello a pomocí hello řádku vzorců, a název hello následující měří v pořadí:  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
Míry vytvořené pro tabulka FactInternetSales hello může být důležité finanční údaje použité tooanalyze například prodej, náklady a zisku pro položky definované hello uživatele vybraného filtru.  
  
## <a name="whats-next"></a>Co dále?
[Lekce 7: Vytvoření klíčových ukazatelů výkonu](../tutorials/aas-lesson-7-create-key-performance-indicators.md)  

  
