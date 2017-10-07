---
Title: aaa "Azure Analysis Services kurz lekce 9: vytvořte hierarchie | Microsoft Docs"Popis: služby: documentationcenter služby analysis services: '' Autor: minewiskan manager: erikre editor: '' značky:"

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend
---
# <a name="lesson-9-create-hierarchies"></a>Lekce 9: Vytvoření hierarchií

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci vytvoříte hierarchie. Hierarchie jsou skupiny sloupců uspořádané do úrovní, například hierarchie Geografie může mít podúrovně pro zemi, stát, kraj a město. Hierarchie můžete zobrazit oddělený od ostatních sloupců v generování sestav klienta aplikace seznam polí, snadněji je pro klienta toonavigate uživatelů a zahrnout do sestavy. Další, najdete v části toolearn [hierarchie](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)
  
hierarchie toocreate použít Návrháře hello modelu v *zobrazení diagramu*. Vytváření a správa hierarchií se v zobrazení dat nepodporuje.  
  
Odhadovaný čas toocomplete této lekci: **20 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 8: vytvoření perspektivy](../tutorials/aas-lesson-8-create-perspectives.md).  
  
## <a name="create-hierarchies"></a>Vytvoření hierarchií  
  
#### <a name="toocreate-a-category-hierarchy-in-hello-dimproduct-table"></a>toocreate hierarchie kategorií v tabulce DimProduct hello  
  
1.  V Návrháři modelu hello (zobrazení diagramu), klikněte pravým tlačítkem na hello **DimProduct** tabulky > **vytvořit hierarchii**. Nové hierarchie se zobrazí v dolní části hello okna hello tabulky. Přejmenujte hello hierarchie **kategorie**.  
  
2.  Klikněte a přetáhněte hello **ProductCategoryName** nové sloupce toohello **kategorie** hierarchie.  
  
3.  V hello **kategorie** hierarchie, klikněte pravým tlačítkem na hello **ProductCategoryName** > **přejmenovat**a pak zadejte **kategorie**.  
  
    > [!NOTE]  
    > Přejmenování sloupce v hierarchii nepřejmenuje sloupce v tabulce hello. Sloupec v hierarchii je právě reprezentace hello sloupec v tabulce hello.  
  
4.  Klikněte a přetáhněte hello **ProductSubcategoryName** sloupec toohello **kategorie** hierarchie. Přejmenujte ho na **Subcategory**. 
  
5.  Klikněte pravým tlačítkem na hello **%{ModelName/** sloupce > **přidat toohierarchy**a potom vyberte **kategorie**. Přejmenujte ho na **Model**.

6.  Nakonec přidejte **EnglishProductName** toohello hierarchie kategorií. Přejmenujte ho na **Product**.  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="toocreate-hierarchies-in-hello-dimdate-table"></a>toocreate hierarchií v tabulce DimDate hello  
  
1.  V hello **DimDate** tabulky, vytvořte hierarchii s názvem **kalendáře**.  
  
3.  Přidejte hello následující sloupce v daném pořadí:

    *  CalendarYear
    *  CalendarSemester
    *  CalendarQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
    
4.  V hello **DimDate** tabulky, vytvořte **fiskálním** hierarchie. Zahrnout hello následující sloupce v daném pořadí:  
  
    *  FiscalYear
    *  FiscalSemester
    *  FiscalQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
  
5.  Nakonec v hello **DimDate** tabulky, vytvořte **ProductionCalendar** hierarchie. Zahrnout hello následující sloupce v daném pořadí:  
    *  CalendarYear
    *  WeekNumberOfYear
    *  DayNumberOfWeek
  
 ## <a name="whats-next"></a>Co dále?
[Lekce 10: Vytvoření oddílů](../tutorials/aas-lesson-10-create-partitions.md) 
  
  
