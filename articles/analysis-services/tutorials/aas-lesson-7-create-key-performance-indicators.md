---
Title: aaa "Azure Analysis Services kurz Lekce 7: vytvoření klíčové ukazatele výkonu | Microsoft Docs"Popis: Popisuje, jak hello toocreate klíčových ukazatelů výkonu v kurzu projektu Azure Analysis Services. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend
---
# <a name="lesson-7-create-key-performance-indicators"></a>Lekce 7: Vytvoření klíčových ukazatelů výkonu

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci vytvoříte klíčové ukazatele výkonu (KPI). Klíčové ukazatele výkonu představují používané toogauge výkonu definované hodnoty *základní* míry, proti *cíl* hodnotu také definovat míru, nebo absolutní hodnotu. V sestavách klientské aplikace, nabízejí klíčových ukazatelů výkonu obchodní specialisty toounderstand rychlý a snadný způsob shrnutí obchodních úspěch nebo tooidentify trendů. Další, najdete v části toolearn [klíčové ukazatele výkonu.](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)
  
Odhadovaný čas toocomplete této lekci: **15 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 6: vytvoření míry](../tutorials/aas-lesson-6-create-measures.md).   
  
## <a name="create-key-performance-indicators"></a>Vytvoření klíčových ukazatelů výkonu  
  
#### <a name="toocreate-an-internetcurrentquartersalesperformance-kpi"></a>toocreate InternetCurrentQuarterSalesPerformance klíčového ukazatele výkonu  
  
1.  V Návrháři hello model, klikněte na tlačítko hello **FactInternetSales** tabulky.  
  
2.  V mřížce měr hello klikněte na tlačítko prázdné buňky.  
  
3.  V řádku vzorců hello výše hello tabulky zadejte hello následující vzorec: 
 
    ```  
    InternetCurrentQuarterSalesPerformance :=DIVIDE([InternetCurrentQuarterSales]/[InternetPreviousQuarterSalesProportionToQTD],BLANK())  
    ```

    Tato míra slouží jako základní míru hello pro hello klíčového ukazatele výkonu.  
  
4.  Pravým tlačítkem myši klikněte na **InternetCurrentQuarterSalesPerformance** > **Vytvořit klíčový ukazatel výkonu**.   
  
5.  V dialogovém okně klíčového ukazatele výkonu (KPI) hello v **cíl** vyberte **absolutní hodnota**a pak zadejte **1.1**.  
  
7.  V poli hello levý jezdec (nízká), zadejte **1**a potom v posuvníku hello vpravo (vysoká) zadejte **1.07**.  
  
8.  V **vyberte ikonu styl**vyberte hello kosočtverec (červený), trojúhelník (žlutý), typ (zelený) ikony v kruhu.
  
    ![aas-lesson7-kpi](../tutorials/media/aas-lesson7-kpi.png)
    
    > [!TIP]  
    > Všimněte si hello rozšíření **popisy** popisek pod hello k dispozici ikona stylů. Použití popisů pro hello různé elementy toomake klíčový ukazatel výkonu je více identifikovatelnými v klientských aplikacích.  
  
9. Klikněte na tlačítko **OK** toocomplete hello klíčového ukazatele výkonu.  
  
    V mřížce měr hello, Všimněte si hello ikonu další toohello **InternetCurrentQuarterSalesPerformance** měr. Tato ikona označuje, že tato míra slouží jako základní hodnota klíčového ukazatele výkonu.  
  
#### <a name="toocreate-an-internetcurrentquartermarginperformance-kpi"></a>toocreate InternetCurrentQuarterMarginPerformance klíčového ukazatele výkonu  
  
1.  V mřížce měr hello pro hello **FactInternetSales** tabulky, klikněte na tlačítko prázdné buňky.  
  
2.  V řádku vzorců hello výše hello tabulky zadejte hello následující vzorec:  

    ```
    InternetCurrentQuarterMarginPerformance :=IF([InternetPreviousQuarterMarginProportionToQTD]<>0,([InternetCurrentQuarterMargin]-[InternetPreviousQuarterMarginProportionToQTD])/[InternetPreviousQuarterMarginProportionToQTD],BLANK())  
    ```
 
3.  Pravým tlačítkem myši klikněte na **InternetCurrentQuarterMarginPerformance** > **Vytvořit klíčový ukazatel výkonu**.  
  
4.  V dialogovém okně klíčového ukazatele výkonu (KPI) hello v **cíl** vyberte **absolutní hodnota**a pak zadejte **1,25**.   
  
5.  V poli hello levý jezdec (nízká), posuňte dokud hello pole zobrazuje **0,8**, a pak hello snímku pravým pole posuvník (vysoká), dokud pole hello zobrazuje **číslem 1,03**.  
  
6.  V **vyberte ikonu styl**, vyberte hello kosočtverec (červený), trojúhelník (žlutý), typ ikony kruhu (zelený) a pak klikněte na tlačítko **OK**.  
  
## <a name="whats-next"></a>Co dále?
[Lekce 8: Vytvoření perspektiv](../tutorials/aas-lesson-8-create-perspectives.md)
  
  
