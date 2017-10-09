---
Title: aaa "Azure Analysis Services kurz lekce 10: vytvoření oddílů | Microsoft Docs"Popis: Popisuje, jak toocreate oddíly v kurzu projektu hello Azure Analysis Services. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend
---
# <a name="lesson-10-create-partitions"></a>Lekce 10: Vytvoření oddílů

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci vytvoříte tabulka FactInternetSales hello toodivide oddíly do menších logických částí, které mohou být zpracován (Aktualizovat) bez ohledu na ostatní oddíly. Ve výchozím nastavení má každá tabulka, které můžete zahrnout do modelu jeden oddíl, který zahrnuje všechny hello tabulka sloupce a řádky. Pro tabulka FactInternetSales hello chceme toodivide hello dat po letech; jeden oddíl pro každou tabulku hello pět let. Jednotlivé oddíly pak bude možné zpracovávat nezávisle na sobě. Další, najdete v části toolearn [oddíly](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular). 
  
Odhadovaný čas toocomplete této lekci: **15 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 9: vytvoření hierarchie](../tutorials/aas-lesson-9-create-hierarchies.md).  
  
## <a name="create-partitions"></a>Vytvoření oddílů  
  
#### <a name="toocreate-partitions-in-hello-factinternetsales-table"></a>toocreate oddílů v tabulce FactInternetSales hello  
  
1.  V Průzkumníku tabelárních modelů rozbalte **Tabulky** a potom klikněte pravým tlačítkem na **FactInternetSales** > **Oddíly**.  
  
2.  Ve Správci oddílů, klikněte na tlačítko **kopie**a potom změňte název hello příliš**FactInternetSales2010**.
  
    Protože chcete pouze řádky, hello oddílu tooinclude v určitém období, pro hello roku 2010, musíte upravit hello výrazu dotazu.
  
4.  Klikněte na tlačítko **návrhu** tooopen dotaz Editor a potom klikněte na hello **FactInternetSales2010** dotazu.

5.  Ve verzi preview, klikněte na tlačítko hello v hello šipka dolů **OrderDate** záhlaví sloupce a pak klikněte na tlačítko **datum a čas filtry** > **mezi**.

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  V hello Filtr řádků dialogové okno v **zobrazit tyto řádky: OrderDate**, nechte **je po nebo je rovno**a potom zadejte do pole pro datum hello **1. 1. 2010**. Nechte hello **a** operátor vybraná, pak vyberte **před**, potom zadejte do pole pro datum hello, **1/1/2011**a potom klikněte na **OK**.

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    Všimněte si, že v editoru dotazů se v části POUŽITÝ POSTUP zobrazí další krok s názvem Filtrované řádky. Tento filtr je tooselect pouze data objednávky z 2010.

8.  Klikněte na **Importovat**.

    Ve Správci oddílů, Všimněte si hello dotazů výraz teď obsahuje další klauzuli filtrované řádky.

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    Tento příkaz určuje, že tento oddíl obsahovat pouze hello data v těchto řádcích hello OrderDate je-li hello kalendářním roce 2010 jako zadaný v klauzuli hello filtrované řádky.  
  
  
#### <a name="toocreate-a-partition-for-hello-2011-year"></a>toocreate oddíl pro hello roku 2011  
  
1.  V seznamu hello oddíly, klikněte na tlačítko hello **FactInternetSales2010** oddílu, kterou jste vytvořili a potom klikněte na **kopie**.  Změňte název oddílu hello příliš**FactInternetSales2011**. 

    Není nutné toouse Editor dotazů toocreate nové klauzule filtrované řádky. Vzhledem k tomu, že vytvoříte kopii hello dotazu pro 2010, je vše, co potřebujete toodo, změňte mírné v hello dotazu pro 2011.
  
2.  V **výrazu dotazu**, v pořadí pro tento oddíl tooinclude pouze řádky pro hello rok 2011, nahraďte hello let v klauzuli hello filtrované řádky s **2011** a **2012**, jako jsou:  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="toocreate-partitions-for-2012-2013-and-2014"></a>pro 2012, 2013 a 2014 toocreate oddíly.  
  
- Postupujte podle předchozích kroků hello, vytváření oddílů pro 2012, 2013 a 2014, změna hello let v hello filtrované řádky klauzule tooinclude pouze řádky pro tohoto roku. 
  

## <a name="delete-hello-factinternetsales-partition"></a>Odstranit oddíl FactInternetSales hello
Nyní když máte oddíly pro každý rok, můžete odstranit hello FactInternetSales oddíl. Při výběru zpracovat všechny při zpracování oddílů, brání překrývají.

#### <a name="toodelete-hello-factinternetsales-partition"></a>hello toodelete FactInternetSales oddílu
-  Klikněte na možnost FactInternetSales oddílu hello a pak klikněte na **odstranit**.



## <a name="process-partitions"></a>Zpracování oddílů  
Ve Správci oddílů, Všimněte si hello **poslední zpracovat** sloupec pro každou hello nové oddíly, které jste vytvořili, zobrazuje tyto oddíly nikdy byly zpracovány. Při vytváření oddílů, byste měli spustit proces oddíly nebo zpracovat tabulku operaci toorefresh hello data v těchto oddílech.  
  
#### <a name="tooprocess-hello-factinternetsales-partitions"></a>tooprocess hello FactInternetSales oddíly  
  
1.  Klikněte na tlačítko **OK** tooclose Správce oddílů.  
  
2.  Klikněte na tlačítko hello **FactInternetSales** tabulky a pak klikněte na hello **modelu** nabídky > **proces** > **proces oddíly**.  
  
3.  V dialogovém okně proces oddíly hello, ověřte **režimu** je nastaven příliš**výchozí proces**.  
  
4.  Zaškrtněte políčko hello v hello **proces** sloupec pro každou hello pět oddíly jste vytvořili a pak klikněte na tlačítko **OK**.  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    Pokud se zobrazí výzva pro pověření k zosobnění, zadejte uživatelské jméno systému Windows hello a heslo, které jste zadali v lekci 2.  
  
    Hello **zpracování dat** se zobrazí dialogové okno a zobrazí podrobnosti o procesu pro každý oddíl. Všimněte si, že pro každý oddíl se přenáší jiný počet řádků. Každý oddíl obsahuje pouze řádky hello roku uvedený v hello klauzule WHERE v příkazu SQL hello. Po dokončení zpracování pokračujte a zavřete dialogové okno zpracování dat hello.  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a>Co dále?
Další lekce přejděte toohello: [lekce 11: vytvoření role](../tutorials/aas-lesson-11-create-roles.md). 
