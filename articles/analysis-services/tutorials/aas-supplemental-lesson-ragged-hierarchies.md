---
Title: aaa "kurz dodatečné lekce Azure Analysis Services: nepravidelné hierarchie s logickými | Microsoft Docs"Popis: Popisuje, jak toofix nepravidelné logickými hierarchií v kurzu hello Azure Analysis Services.
služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend
---
# <a name="supplemental-lesson---ragged-hierarchies"></a>Doplňková lekce – Nepravidelné hierarchie

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této doplňkové lekci vyřešíte běžný problém vyskytující se při přesunu do hierarchií, které obsahují prázdné hodnoty (členy) na různých úrovních. Může se to například týkat organizace, ve které vysoce postavenému manažerovi přímo reportují manažeři a jiní zaměstnanci oddělení. Nebo se může jednat o geografické hierarchie skládající se z položek Země, Oblast, Město, kde některá města nemají nadřazený stát nebo kraj, například Washington D. C. nebo Vatikán. Hierarchie má prázdné členy, je často descends toodifferent nebo nepravidelné logickými úrovně.

![aas-lesson-detail-ragged-hierarchies-table](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

Tabulkové modely na úrovni kompatibility hello 1400 mají další **skrýt členy** vlastnost pro hierarchie. Hello **výchozí** nastavení předpokládá, že neexistují členové, prázdné jakékoli úrovni. Hello **skrýt prázdné členy** nastavení vyloučí prázdné členy z hierarchie hello při přidání tooa kontingenční tabulku nebo sestavy.  
  
Odhadovaný čas toocomplete této lekci: **20 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma doplňkové lekce je součástí kurzu tabulkového modelování. Před provedením úlohy hello v této další lekci, by byly dokončeny všechny předchozí lekce nebo máte dokončený projekt modelu ukázkové společnosti Adventure Works Internet prodej. 

Pokud vytvoříte projekt AW Internet prodej hello v rámci kurzu hello modelu ještě neobsahuje data ani hierarchie, které jsou nepravidelné. toocomplete tento doplňkový lekce, musíte se napřed toocreate hello problém tak, že přidáte některé další tabulky, vytvořte relace, počítané sloupce, míry a nové hierarchie organizace. Tato část zabere přibližně 15 minut. Potom můžete získat toosolve ho za několik minut.  

## <a name="add-tables-and-objects"></a>Přidání tabulek a objektů
  
### <a name="tooadd-new-tables-tooyour-model"></a>tooadd nové tabulky tooyour modelu
  
1.  V Průzkumníku tabulkových modelů rozbalte položku **Zdroje Dat**, potom klikněte pravým tlačítkem myši na vaše připojení > **Importovat nové tabulky**.
  
2.  V Navigátoru vyberte **DimEmployee** a **FactResellerSales** a potom klikněte na **OK**.

3.  V Editoru dotazů klikněte na **Importovat**.

4.  Vytvořte následující hello [vztahy](../tutorials/aas-lesson-4-create-relationships.md):

    | Tabulka 1           | Sloupec       | Směr filtru   | Tabulka 2     | Sloupec      | Aktivní |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | FactResellerSales | OrderDateKey | Výchozí            | DimDate     | Datum        | Ano    |
    | FactResellerSales | DueDate      | Výchozí            | DimDate     | Datum        | Ne     |
    | FactResellerSales | ShipDateKey  | Výchozí            | DimDate     | Datum        | Ne     |
    | FactResellerSales | ProductKey   | Výchozí            | DimProduct  | ProductKey  | Ano    |
    | FactResellerSales | EmployeeKey  | tooBoth tabulky | DimEmployee | EmployeeKey | Ano    |

5. V hello **DimEmployee** tabulky, vytvořte následující hello [počítaných sloupců](../tutorials/aas-lesson-5-create-calculated-columns.md): 

    **Cesta** 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    **Úplný název** 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    **Level1** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    **Level2** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,2)) 
    ```

    **Level3** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,3)) 
    ```

    **Level4** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,4)) 
    ```

    **Level5** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,5)) 
    ```

6.  V hello **DimEmployee** tabulky, vytvořte [hierarchie](../tutorials/aas-lesson-9-create-hierarchies.md) s názvem **organizace**. Přidat hello následující sloupce v daném pořadí: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.

7.  V hello **FactResellerSales** tabulky, vytvořte následující hello [měr](../tutorials/aas-lesson-6-create-measures.md):

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  Použití [analyzovat v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) tooopen aplikace Excel a automaticky vytvořit kontingenční tabulku.

9.  V **pole kontingenční tabulky**, přidejte hello **organizace** hierarchie z hello **DimEmployee** tabulky příliš**řádky**a hello **ResellerTotalSales** míru z hello **FactResellerSales** tabulky příliš**hodnoty**.

    ![aas-lesson-detail-ragged-hierarchies-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    Jak vidíte v hello kontingenční tabulky, zobrazí hello hierarchie řádků, které jsou nepravidelné. Je tam množství řádků, které zobrazují prázdné členy.

## <a name="toofix-hello-ragged-hierarchy-by-setting-hello-hide-members-property"></a>toofix hello nepravidelné hierarchie logickými nastavením vlastnosti hello skrýt členy

1.  V **Průzkumníku tabulkových modelů** rozbalte **Tabulky** > **DimEmployee** > **Hierarchie** > **Organization**.

2.  V části **Vlastnosti** > **Skrýt členy** vyberte **Skrýt prázdné členy**. 

    ![aas-lesson-detail-ragged-hierarchies-hidemembers](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  Zpět v aplikaci Excel aktualizujte hello kontingenční tabulky. 

    ![aas-lesson-detail-ragged-hierarchies-pivottable-refresh](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    Teď to vypadá mnohem líp!

## <a name="see-also"></a>Viz také   
[Lekce 9: Vytvoření hierarchií](../tutorials/aas-lesson-9-create-hierarchies.md)  
[Doplňková lekce – Dynamické zabezpečení](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Doplňková lekce – Řádky podrobností](../tutorials/aas-supplemental-lesson-detail-rows.md)  